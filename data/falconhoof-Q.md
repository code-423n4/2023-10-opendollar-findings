[L2]: https://github.com/code-423n4/2023-10-opendollar/blob/main/bot-report.md#l2-missing-checks-for-address-0-when-assigning-values-to-address-state-variables
[L2 Snippet]: https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L37-L39

## Low Issues
### Low Issue 1
This is another instance of bot reported L2 issue; see [here][L2] 

### Summary
There should be a zero address check when adding new authorized address via ```AccountingEngine.sol::addAuthorization(address _account)``` see affected code [here][L2 Snippet]