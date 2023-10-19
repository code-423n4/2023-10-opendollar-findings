**contracts/proxies/ODSafeManager.sol**
- L89/90/91 - The _safes.length value is used multiple times, therefore gas could be saved by generating a variable in memory.
