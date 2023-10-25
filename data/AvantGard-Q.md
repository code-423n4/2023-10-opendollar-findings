## Title

Separate functions and events needed to revoke SAFE and HANDLER permissions

# vulnerability details

## Impact

Currently there are no functions & events to deny already existing `SAFE` and `HANDLER` permissions, 

without using the already misleading `allowSAFE``` & ```allowHandler``` functions.

## Proof of Concept

Contract : `src/contracts/proxies/ODSafeManager.sol`

`allowSAFE` & `allowHandler` functions exist to allow access to another address.

What if an user wants to deny access to an already approved address, 
without emitting the incorrect AllowSAFE & AllowHandler events?

We need separate functions/events for revoking access.


## Tools Used

Manual Review

## Recommended Mitigation Steps

Implement separate functions & Events to revoke `SAFE` and `HANDLER` permissions

for reference 
the `safeEngine.sol` contract uses the following pattern to deny access

```
function denySAFEModification(address _account) external {
    safeRights[msg.sender][_account] = 0;
    emit DenySAFEModification(msg.sender, _account);
  }
```