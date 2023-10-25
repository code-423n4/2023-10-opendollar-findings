QA_1:
Changeable metadata should not be writing in contract.
https://github.com/open-dollar/od-contracts/blob/a6fc5524404acb56f9a69d93661fe99cf2bea877/src/contracts/proxies/Vault721.sol#L22-L23
```solidity
string public contractMetaData =
    '{"name": "Open Dollar Vaults","description": "Tradable Vaults for the Open Dollar stablecoin protocol. Caution! Trading this NFT means trading the ownership of your Vault in the Open Dollar protocol and all of the assets/collateral inside each Vault.","image": "https://app.opendollar.com/collectionImage.png","external_link": "https://opendollar.com"}';
```

Message is write  in contract directly,but it can be modified
```solidity
  function updateContractURI(string memory _metaData) external onlyGovernor {
    contractMetaData = _metaData;
  }
```
This may confuse people who check the contract code after metadata changes



QA_2:

`transferSAFEOwnership`  should  clear  `safeCan`  `handlercan`  data

https://github.com/open-dollar/od-contracts/blob/7dbe64b53caf4f7fb122fa94b2bde502c6dea8da/src/contracts/proxies/ODSafeManager.sol#L136-L152

https://github.com/open-dollar/od-contracts/blob/7dbe64b53caf4f7fb122fa94b2bde502c6dea8da/src/contracts/proxies/ODSafeManager.sol#L105-L109

```solidity 
 function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
    address _owner = _safeData[_safe].owner;
    safeCan[_owner][_safe][_usr] = _ok;
    emit AllowSAFE(msg.sender, _safe, _usr, _ok);
  }
```
 Actually `allowSAFE` is a bit like `approve` logic. But older approved `safecan` `handlercan` never gets cleared.  Since after token transfer, `_owner`will change, this older data remain useless, only if  token again transfer back to the origin owener,don't know design intent， remain QA.





