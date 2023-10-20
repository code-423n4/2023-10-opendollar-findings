# **Approach to Auditing**
  - We began by thoroughly reviewing the provided documentations, as well as reviewing the video provided. We compared them to the GEB framework on which the Opendollar protocol is based. Here, we made note of important information, noted down unclear parts and overall tried to fully understand how the protocol should function. We also took notes of integrated and inherited protocols and mapped out possible risk areas.

  - While we were studying the docs, we had ran static analyzers and linters through the contracts to detect basic vulnerabilities and other non critical errors. The result of our static analysis was then compared to the provided bot reports and those deemed known were removed. 

  - After the documentation review and static analysis, we started the process of manual code inspection. We noted how the contracts were divided into different modules and we inspected the contracts in each module carefully. We stuidied each of the contracts, tested how each function behaves and compared that to how they're expected to behave. We then tried out various attack ideas, including the ones provided by the sponsors. We noted the dependencies, inheritancies, and possible vulnerabilities that can arise from them. We made comparisons to issues found in protocols of the same kind (including older commits) to see if the same mistakes were repeated and how effective the provided fixes were. 

  - After this was done, we made notes on the issues we found and prepared the analysis report.

# **Architecture Review**
The codebase in scope is divided into four major modules.
### **The Proxy Module**
The Proxy module enhances the user experience when interacting with the protocol. It includes contract interfaces, proxies, and actions that are essential for vault management.
In scope, the module comprises
  - [ODProxy.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODProxy.sol) - is a non-transferrable modified version of the Maker Protocol's DSProxy. This ensures that only the Vault721 contract, which is controlled by the SAFEEngine, can transfer a safe. A user can execute functions through this proxy by passing in the bytecode for the target contract as well as the calldata for the function they want to call.
  - [Vault721.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol) - creates and registers proxies for needed accounts, and records their proxy ownership. It also oversees all safe ownership, transfers, and authorizations following the ERC721 protocol. When a user opens a safe, a NFT-Vault is minted to their account, which controls the ownership and transferability of the safe, where the tokenId is equal to the safeId.
  - [SAFEHandler.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/SAFEHandler.sol) - gives the ODSafeManager the permission to manage the safes. It is also responsible for handling individual safes (one SAFEHandler to one safe). It's address is used to uniquely identify the safe within the SAFEEngine.
  - [ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/ODSafeManager.sol) - allows the users to manage their safes. It's integrated to the Vault721 contract and uses it to move safes and mint tokens when a new safe is created.
  - [BasicActions.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/actions/BasicActions.sol) - defines the actions that can be executed on a safe. Actions include generating delta debt and delta repaid debt, modifying safe collteralizations, locking and freeing token collateralizations and so on.

### **The Oracle Module**
The Oracle module ensures that the system has access to up-to-date and accurate price information which are then used to set up system parameters. Contracts in scope here include
  - [CamelotRelayer.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol) - The Oracle Relayer this contract to find out how much the open dollar coin is currently worth from the Camelot pool on the Arbitrum network. The quote obtained from the pool is converted into an 18 decimals format.
  - [CamelotRelayerChild.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerChild.sol) - inherits the functionality of CamelotRelayer contract to be factory deployed
  - [CamelotRelayerFactory.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/factories/CamelotRelayerFactory.sol) - is the factory for creating/deploying CamelotRelayerChild contracts.
  - [UniV3Relayer.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol) - is the same as the CamelotRelayer contract, but it consults the UniswapV3 pool instead.

    
### **The Core Module**
The Core Module stores all the SAFE data, allows external actors to trigger liquidations in case SAFEs are underwater and also handles debt and surplus auctions. The AccountingEngine is the only contract here in scope.
  - [AccountingEngine.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/AccountingEngine.sol) - manages both the surplus and debt of a system. It does this by auctioning off debt to cover deficits and by auctioning off or transferring surplus to the extraSurplusReceiver
    
### **The Governance Module**
The Governance Module modifies and upgrades the protocol.
  - [ODGovernor.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/gov/ODGovernor.sol) - inherits the OZ governance contracts. It modifies protocol parameters.

# **Codebase Review**

  - The codebase contracts are well commented, although the documentation provided seems to be outdated. Some of the contracts in scope don't appear in the documentation. We recommend updating these for users and future audits. 

  - Unit tests were performed for these contracts, however some of the contracts appear to have low coverages. The AccountEngine contract has a quite complicated codebase, codebases like these are recommended to have invariant tests. We also recommend implementing and performing more tests to improve the coverages. This can help catch basic bugs and improve safety. 

  - Some basic contract safety measures also seemed to be ignored. Castings are done without unsafely, there are non zero checks when initializing the Vault721 and ODProxy contracts, some polymorphic functions and there seems to be a case of a loop in an external functions. These are by no means very serious issues, but they cause a host of inconvienieces to the protocol implementation.

  - Error handling is quite good. A mix of custom and require errors are used. We recommend using the same error approach throughout the codebase, preferably custom errors because they're more gas-efficient. Other improvements include adding descriptions and NatSpec comments to the errors (some are lacking) and making sure the descriptions are not cryptic. 

  - Events are emitted for important parameter changes, although in some cases, they seemed to not follow the checks-effects-interaction patterns. Also, to save gas, its recommended to use assembly to emint these events.

  - Overall, the codebase is solid, well written and is of high quality, however improvements can be made in certain areas. We recomment using linters and static analysis tools to clean up the codebase before deployment.

## **Centralization and Systemic Risks**

  - Risk from centralization exists and is quite great. Some contracts are controlled by governace and owners, every actions from these parties have impacts on the users and the protocols. They have to be trusted not to act maliciously, and to also be careful to not get hacked.

  - Risk from smart contracts also pose a risk to the system. Critical security flaws, access control errors, logical inconsistencies can lead to loss of assets or system manipulations. We recommend constant sytem upgrades and audits, and also taking into consideration auditors' recommendations before deploying these contracts.
  
  - Risks from third party contracts also exist. The contract imports OZ contracts,any vulnerability in these contracts are likely to have an effect in the system. The `ODGovernace` contract for instance, fully inherits the OZ Governance contracts. Any issue with these contracts means the `ODGovernace` is also vulnerable to that issue. Also, an older version of OZ is being used, 4.8.2, we recommend upgrading to version 5.0 which as of time of auditing is the latest version.  
  
  - Risks from Oracles. The protocol relies on a number of TWAPs to fetch token price data, this makes the contracts vulnerable to manipulation attacks, inaccurate data feeds and so on. 

# **Conclusion**
OpenDollar is a DeFi lending protocol that enables borrowing against liquid staking tokens while earning staking rewards and enabling liquidity. The contracts are built around the GEB framework, it uses collaterized debt positions (CDP) to allow accounts to generate debt agaisnt deposited collateral. As a modification of the GEB framework,  Non-Fungible Vaults were added. It ties a CDP to an NFT unlike the traditional based account to CDP (non transferable). These CDPs can be then sold through existing NFT market places. 

The OpenDollar in general codebase is well-designed, but identified risks need to be fixed. Measures should be implemented to protect the protocol from potential attacks. Finally, we recommend updating the documentation and comments to make it easier for users, developers and auditors to understand and work on the code.


### Time spent:
32 hours