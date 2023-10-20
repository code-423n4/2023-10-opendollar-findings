[L2 Bot]: https://github.com/code-423n4/2023-10-opendollar/blob/main/bot-report.md#l2-missing-checks-for-address-0-when-assigning-values-to-address-state-variables
[L1 Snippet]: https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L37-L39

## Low Issues
### Low Issue 1
This is another instance of the L2 bot reported [issue][L2 Bot] 

### Description
There should be a zero address check when adding new authorized address via ```AccountingEngine.sol::addAuthorization(address _account)``` see affected code [here][L1 Snippet]

### Recommendation
Update function to check address validity using assertNonNull():

```diff
  function addAuthorization(address _account) external override(Authorizable, IAuthorizable) isAuthorized whenEnabled {
-               _addAuthorization(_account);
+               if (_account == address(0)) revert AccEng_ZeroAddress;

          }
```
