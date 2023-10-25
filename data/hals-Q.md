# Findings Summary

| ID            | Title                                                                                                                                | Severity |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| [L-01](#l-01) | Setting the addresses of `surplusAuctionHouse` & `debtAuctionHouse` to zero will require redeploying the `AccountingEngine` contract | Low      |
| [L-02](#l-02) | The governance will still have power over the `AccountingEngine` contract while it shouldn't as per the intended design              | Low      |
| [L-03](#l-03) | `UniV3Relayer` contract works only with tokens of decimals <= 18                                                                     | Low      |
| [L-04](#l-04) | `ODSafeManager.allowSAFE` function enables any allowed address to add/remove other allowed addresses                                 | Low      |

# Low

## [L-01] Setting the addresses of `surplusAuctionHouse` & `debtAuctionHouse` to zero will require redeploying the `AccountingEngine` contract <a id="l-01"></a>

## Details

- When deploying the `AccountingEngine` contract: if `surplusAuctionHouse` & `debtAuctionHouse` addresses are both zero address; they can never be re-set again due to the `_validateParameters` function that is invoked in the `validParams` modifier after the contract manager calls the inherited `modifyParameters` function.

- The `modifyParameters` function will allow setting one value only with each call; then the `_validateParameters` checks if neither of the `surplusAuctionHouse` or the `debtAuctionHouse` is a zero address, if anyone of them is set to zero; the function will revert.

- So setting these addresses to zero in the `constructor` upon deployment will require a redeployemnt of the `AccountingEngine` contract as these addresses can't be re-set again .

## Proof of Concept

[AccountingEngine.\_validateParameters function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L314-L317)

```solidity
  function _validateParameters() internal view override {
    address(surplusAuctionHouse).assertNonNull();
    address(debtAuctionHouse).assertNonNull();
  }
```

[AccountingEngine.modifyParameters function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/utils/Modifiable.sol#L22-L25)

```solidity
  function modifyParameters(bytes32 _param, bytes memory _data) external isAuthorized validParams {
    _modifyParameters(_param, _data);
    emit ModifyParameters(_param, _GLOBAL_PARAM, _data);
  }
```

[AccountingEngine.validParams modifier](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/utils/Modifiable.sol#L66-L69)

```solidity
  modifier validParams() {
    _;
    _validateParameters();
  }
```

## Recommended Mitigation Steps

In the `AccountingEngine.constructor`: check that neither of the assigned addresses are set equal t the zero address.

## [L-02] The governance will still have power over the `AccountingEngine` contract while it shouldn't as per the intended design <a id="l-02" ></a>

## Details

- In `AccountingEngine` contract: the contract deployer (the manager) can add authorize any address to have access on the privilaged functions of the contract by calling `addAuthorization` function.

- It was mentioned in the protocol documents that the governance doesn't have power over the contract (not an authorized entity), this is stated in the contracts docs as a comment on the `transferPostSettlementSurplus` function:
  > transferPostSettlementSurplus
  > Transfer any remaining surplus after the disable cooldown has passed. Meant to be a backup in case GlobalSettlement.processSAFE has a bug, governance doesn't have power over the system and there's still surplus left in the AccountingEngine which then blocks GlobalSettlement.setOutstandingCoinSupply.
- But the contract manager can authorize any address without checking if it's the governance address or not; thus breaking th eintended design.

## Proof of Concept

[AccountingEngine.addAuthorization function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L37-L39)

```solidity
  function addAuthorization(address _account) external override(Authorizable, IAuthorizable) isAuthorized whenEnabled {
    _addAuthorization(_account);
  }
```

## Recommended Mitigation Steps

Add acheck in the `addAuthorization` function to prevent granting the governance address authorization over `AccountingEngine` contract.

## [L-03] `UniV3Relayer` contract works only with tokens of decimals <= 18 <a id="l-03" ></a>

## Details

- When the `UniV3Relayer` contract is deployed; the `multiplier` state variable that's going to be used to parse the price result from the aggregator is calculated based on a maximum `_quoteToken` decimals of 18:

  ```solidity
  multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
  ```

- So if any `_quoteToken` with decimals > 18 will not be supported by the `UniV3Relayer` contract, and this will limit the number of tokens that can be integrated in/adopted by the protocol.

## Proof of Concept

[UniV3Relayer.constructor](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/UniV3Relayer.sol#L64-L64)

```solidity
multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```

## Recommended Mitigation Steps

Modify the `multiplier` variable calculation to account for `_quoteTokens` of decimals > 0.

## [L-04] `ODSafeManager.allowSAFE` function enables any allowed address to add/remove other allowed addresses<a id="l-04" ></a>

## Details

- The `ODSafeManager.allowSAFE` function is meant by design to allow/disallow any address to manage the safe; this can be first accessed by the SAFE owner to add an allowed address; then this allowed address will have the ability to add/remove (allow/disallow) any address on the SAFE.

- As the allowed addresses on the safe can call vital functions on the SAFE such as transferring its collateral and internal coins;any malicious address can overtake the SAFE and drain it.

- So it's recommended to enable the safe owner only from accessing the `ODSafeManager.allowSAFE` function on his own SAFE.

## Proof of Concept

[ODSafeManager.allowSAFE function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105-L109)

```solidity
  function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok;
    emit AllowSAFE(msg.sender, _safe, _usr, _ok);
  }
```

## Recommended Mitigation Steps

Update `ODSafeManager.allowSAFE` function to enable the SAFE owner only from allowing/disallowing any address on the SAFE (similar to the design of `allowHandler` function).
