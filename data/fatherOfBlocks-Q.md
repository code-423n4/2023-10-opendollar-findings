**contracts/oracles/CamelotRelayer.sol**
- L7/41 - There is commented code that should be removed, since this reduces the understanding of the code.

**contracts/proxies/actions/BasicActions.sol**
- L43/63 - Before performing a division by a value generated in line 37, it should be validated that it is != 0, so as not to generate an unhandled exception.
