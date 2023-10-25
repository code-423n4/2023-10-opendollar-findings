# L01 : `initializeRenderer` should be under `onlyGovernor` modifier . 

Currently ,  `initializeRenderer` function lacks access control which makes it vulnerable to frontrunning attacks . 
It should be  under `onlyGovernor` modifier . 

```
function initializeRenderer() external {
    if (address(nftRenderer) == address(0)) _setNftRenderer(msg.sender);
  }
```

https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L63

# L02 : No event emitted in  function `addSAFE` :

 function `addSAFE ` is changing the state of the contract . Standard practice is emitting an event in case of state change but here no event is emitted .  an event should be emitted here 