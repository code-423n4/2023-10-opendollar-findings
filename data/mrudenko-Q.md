## QA Report

### Q-1
**Issue:** Missing address `require` check.

- [AccountingEngine.sol#L87](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L87) on variable `_debtAuctionHouse`.
- [ODSafeManager.sol#L189](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L189) Variable `_dst`.
- [BasicActions.sol#L37](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol#L37) Variables `_safeEngine` and `_safeHandler`.
- [ODSafeManager.sol#L205C32-L205C36](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L205C32-L205C36)

### Q-2
**Issue:** Unnecessary parentheses.

- [AccountingEngine.sol#L109C28-L109C43](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L109C28-L109C43)

### Q-3
**Recommendation:** Use enum instead of direct strings.

- [AccountingEngine.sol#L288](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol#L288)

### Q-4
**Issue:** Duplicated code.

- [CamelotRelayer.sol#L74-L84](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L74-L84)
- [CamelotRelayer.sol#L91-L100](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L91-L100)

**Recommendation:** Use `read` functions in `getResultWithValidity` function.

### Q-5
**Issue:** Missing inspections to check on address 0.

- [CamelotRelayerFactory.sol#L24-L25](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerFactory.sol#L24-L25)

### LOW

#### L-1
**Issue:** Probable DoS vulnerability. `_safes` length can be really long.

**Recommendation:** Use `_safes` as a parameter.

- [ODSafeManager.sol#L91](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol#L91)

