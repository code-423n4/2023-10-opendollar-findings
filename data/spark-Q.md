# 1. Each Camelot may have multiple relayers
Since there is no limit to duplicate relayer creation, which will allow multiple relayers for a single camelot pair, which may generate chaos if the interaction party for `camelotRelayersList` is intended to have a unique relayer for each pair. 
```
  function deployCamelotRelayer(
    address _baseToken,
    address _quoteToken,
    uint32 _quotePeriod
  ) external isAuthorized returns (IBaseOracle _camelotRelayer) {
    _camelotRelayer = new CamelotRelayerChild(_baseToken, _quoteToken, _quotePeriod);//@audit multiple relayers for the same pair
    _camelotRelayers.add(address(_camelotRelayer));
    emit NewCamelotRelayer(address(_camelotRelayer), _baseToken, _quoteToken, _quotePeriod);
  }
```
Also, inside the relayer, the symbol for each relayer may have collision, since different token may share the same name.

Recommend using mapping instead to record the relayer and additional existence check may needed.

# 2. The governor setting not reliable
The initial setting for the voting period in `ODGovernor` is 15 block, which can be too tight for the governance. Based on Arbitrum block time, which is about ~0.26 seconds, which can be too short.
```
  constructor(
    address _token,
    TimelockController _timelock
  )
    Governor('ODGovernor')
    GovernorSettings(1, 15, 0) //@audit voting period too short -> 15 block
    GovernorVotes(IVotes(_token))
    GovernorVotesQuorumFraction(3)
    GovernorTimelockControl(_timelock)
  {}
```

Reference: https://officercia.mirror.xyz/d798TVQyA1ALq3qr1R9vvujdF7x-erXxK2wQWwbgRKY

# 3. Testnet address being used
In `CamelotRelayer`, `_CAMELOT_FACTORY` is assigned with Goerli testnet address, which may cause issue during the mainnet launch  

```
  address internal constant _CAMELOT_FACTORY = GOERLI_CAMELOT_V3_FACTORY; //@audit stale address may affect the contract during deployment

```

# 4. Missing contract existence check for the low level call
For the `execute` function in `ODProxy`, there is no validation if the `_target` is a contract.
Reference: https://solodit.xyz/issues/l-02-missing-contract-existence-checks-before-low-level-calls-code4rena-llama-llama-git