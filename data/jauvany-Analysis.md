**Introduction**

Open Dollar is a DeFi lending protocol that enables borrowing against liquid staking tokens while earning staking rewards and helping liquidity via Non-Fungible Vaults (NFVs).

# Approach taken in evaluating the codebase

> My analysis of the Open Dollar Protocol Included understanding the architecture, mechanism, overall codebase, and possible risks associated with the protocol.

- Day 1: I spent time reading the different available documentation in order to have a deep understanding of the protocol. 

- Day 2: I analyzed the codebase for better understanding, Performed a Mechanism review, and investigated possible systemic risks, and centralization risks. 

- Day 3: I dedicated this day to coming up with possible Architecture recommendations after identifying possible risks and preparing the final analysis report.


# Codebase quality analysis

- The Open Dollar codebase is well-structured, well-documented, and includes comprehensive tests. However, there are areas where improvements could be made, particularly in terms of gas efficiency and adherence to ERC20 standards.
 

# Centralization risks

- The Open Dollar ecosystem is designed to be decentralized, with multiple controllers managing a contract and various standards for decentralized ownership and execution. However, there are potential centralization risks, particularly if a single controller is granted multiple permissions. This could potentially allow the controller to bypass required permissions or lock the account. To mitigate these risks, it would be beneficial to implement additional checks and balances in the permission management system.

# Mechanism review

- The mechanisms implemented in the Open Dollar ecosystem, including the ERC721 standard, ERC20 standard, the various modules for ownership management, execution and extension, and the addition of NFV (Non-Fungeble Vaults), are innovative and well-designed. They provide a comprehensive range of features and capabilities, enabling a wide range of use cases in the NFT infrastructure. 

Nonetheless, there are some potential issues and risks associated with these mechanisms. For example, the use of decimals() which is not a part of the ERC-20 standard. These issues should be carefully considered and mitigated to ensure the security and reliability of the ecosystem.

# Systemic risks

- Like any smart contract-based system, Open Dollar is exposed to potential coding bugs or vulnerabilities. Exploiting these issues could result in the loss of funds or manipulation of the protocol. 

- Test Coverage: The test coverage provided by Open Dollar is 95%, however I recommend 100% test coverage.

- Governance Mechanism Security: Open Dollar’s governance mechanism (ODGovernor.sol) is critical for its operation. A poorly implemented governance mechanism could lead to system-wide issues.

- External Contract Dependencies: Open Dollar relies on a considerable number of external contracts e.g. Openzepplin and Uniswap. If any of these contracts have vulnerabilities, it would affect the protocol.

# Architecture recommendations

- I recommend omitting functions that are not part of the ERC20 standards, for example, decimals() and symbol() which appear multiple times. 

- Consider using OpenZeppelin’s SafeCast library to prevent unexpected overflows when casting from various type int/uint values

- Consider using descriptive constant s when passing zero as a function argument. Passing zero as a function argument can sometimes result in a security issue (e.g. passing zero as the slippage parameter). Consider using a constant variable with a descriptive name, so it's clear that the argument is intentionally being used, and for the right reasons.

- I recommend adding a list of blacklist functions. NFT thefts have increased recently. So with the addition of hacked NFTs to the platform, NFTs can be converted into liquidity.

**Gas Optimizations**

- Review data types: Analyze the data types used in your smart contracts and consider if they can be further optimized. For example, changing uint256 to uint128 or uint94 can save gas and storage slots.
 
- Struct packing: Look for opportunities to pack structs into fewer storage slots. By carefully selecting appropriate data types for struct members, you can reduce the overall storage usage.
 
- Use constant values: If certain values in your contracts are constant and do not change, declare them as constants rather than storing them as state variables. This can significantly save gas costs. 

- Avoid unnecessary storage: Examine your code and eliminate any unnecessary storage of variables or addresses that are not required for contract functionality. 

- Storage vs. memory usage: When working with arrays or structs, consider whether using storage instead of memory can save gas. Using storage allows direct access to the state variables and avoids unnecessary copying of data.
 
- Replacing the use of memory with calldata for read-only arguments in external functions.

**Other recommendations**

- Regular code reviews and adherence to best practices. 
- Conduct external audits by security experts. 
- Consider open sourcing the contract for community review. 
- Maintain comprehensive security documentation. 
- Establish a responsible disclosure policy for vulnerabilities. 
- Implement continuous monitoring for unusual activity. 
- Educate users about risks and best practices.

### Time spent:
24 hours