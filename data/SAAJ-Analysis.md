# Introduction
This analysis provide comprehensive overview of various aspects of the ```Open Dollar``` project that is developed as a stablecoin project that will be deployed on the Arbitrum, layer 2 blockchain.

The analysis is based on context of reviewing the project to identify any potential vulnerability that existed in any of contracts  that are in scope.


# Mechanism Review
The ```Open Dollar``` project is aimed at developing a stablecoin by taking a unique approach by introducing a working method based on collateralization through use of Non-Fungible Vaults (NFVs). The working method is based on shifting the ownership of vaults from users accounts to a particular non-fungible tokens (NFTs). The development of NFVs as a core working method introduces a new DeFi primitive that merges the liquidity of NFT markets with the stability of a stablecoin. Owners of vaults will be able to trade the NFVs on existing NFT marketplaces, creating new opportunities for users to increase their profitability. Automated systems will also have a significant role in the assisting user to sell their vaults to arbitrageurs without incurring liquidation penalties, effectively optimizing capital allocation in the ecosystem.

The merging concept of DeFi with NFTs, opens up huge potential for ```Open Dollar``` project by tapping into the existing infrastructure of the NFT space. NFTs representing vault ownership will be used in variety of ways, like providing collateral for various activities related to working of DeFi with having role in governance. The most significant advantage of developing this system related to NFT collaterals is by aspect of creation of a capital-efficient market for liquidatable vaults, which significantly reduces risk when users engage in leveraged positions. The Open Dollar (OD) ecosystem is developed by introducing various pivotal roles in the operation and governance in the protocol contracts making it an innovative financial platform.

The contracts in scopes are reviewed as below for their design and provide general context of the working and interaction mechanism which is followed by the protocol to work effectively.

**AccountingEngine**

```AccountingEngine``` contract acts as the main pillar within the Open Dollar ecosystem, as it receives both system surplus and system debt. This contract has a central role in maintaining the financial stability of the protocol a it initiates debt auctions to cover the shortfall when the system faces deficits. The contract is also responsible for orchestrating auctions or transfers the surplus to an external entity known as ```extraSurplusReceiver```.  The process of handling deficit and surplus ensures that the system remains balanced, which is the main fundamental requirement for smooth working of Open Dollar stablecoin ecosystem.

**CamelotRelayer**

```CamelotRelayer``` contract have an integral importance in creating the oracle system within Open Dollar.  The main primary function is related to fetching current market price of the system coin, OD, using a Camelot pool on the Arbitrum network. The price feed is essential for various operations within the ecosystem including the calculation of collateral values and stability fees.

**UniV3Relayer**

```UniV3Relayer```contract offers an alternative solution to fetching market prices for OD. Instead of using the Camelot pool, it taps into Uniswap V3 on the Arbitrum network. This flexibility provides the ecosystem with redundancy and options, enhancing the reliability of price data.

**ODGovernor**

```ODGovernor``` is a DAO-managed contract providing authority to modify protocol parameters. The Governance aspect is crucial for decentralist working of the Open Dollar ecosystem. Teh role of governor is linked to make changes in the protocol like adding new collateral tokens and altering PID (Protocol ID) settings. The contract main context is related to empowering the community to collectively play its role for providing direction to the project based on collective interest.

**ODProxy**

```ODProxy``` contract acts as a specialized updated version of the DSProxy, with notable difference of restricting ownership change. The feature ensures that only the ```Vault721``` contract has the capability to transfer ownership of a safe. The ODProxy contract safeguards the security of the system by ensuring prevention of unauthorized access and alterations to working of the system.

**Vault721**
 
```Vault721``` contact acts as a multi-purpose contract within the Open Dollar ecosystem. The contract has various roles by serving as ```Proxy Registry```, ```Proxy Factory``` and ```ERC721 "Non-fungible Vault"```.  This contract is core part in functionality of mechanism related to managing safe ownership, transferring and approvals by following specific ERC721 standard. When a safe is created or transferred to an account without a proxy, Vault721 deploys new proxies as needed, ensuring that each safe is managed efficiently.

**SAFEHandler**

```SAFEHandler``` contract plays a critical role in granting permission to the ODSafeManager to make modifications to a safe. In the Open Dollar ecosystem, a new SAFEHandler is deployed for each safe, and its address serves as a unique identifier within the SAFEEngine. This fine-grained control over safes is essential for maintaining the security and integrity of the platform.

**ODSafeManager**
 
```ODSafeManager``` contract act as a more restrictive Safe Manager allowing only the Vault721 contract to move a safe. The contract also initiate minting of Vault721 tokens when a new safe is created by taking a controlled approach that ensures system's safes are managed securely and efficiently.

# Codebase Quality Analysis

This section carry out analysis of quality of codebase for contracts that are in scope. The quality analysis is based on context of identifying areas that needs improvement in terms of carry out working of key functionalities and how they impact the overall concept of the protocol.

The analysis is conducted separately for each contracts to gain a clear and robust understanding of their workflow and how they impact the over protocol efficiency. The analysis is carried out mainly in context of security pitfalls that existed within the overall codebase of the project and may impact the whole protocol in a negative manner.

```AccountingEngine``` contract have a vital role in terms of managing surplus and debt within the protocol, ensuring the financial stability of the overall stablecoin ecosystem. The contract utilizes an authorization mechanism, allowing specific addresses to perform critical actions. It implements roles and access control for secure operations. However, the contract have various functions that are external and without proper access control which can result in possible unauthorized access to any vital process. The contract also ignores the importance of having proper control for functions that made external calls like ``` settleDebt``` and ``` auctionDebt``` that can lead to possible re-entrancy.

``` CamelotRelayer``` and ```UniV3Relayer```  contracts act as oracle that retrieves and processes price information from the CamelotRelayer system. The ```getResultWithValidity``` function queries the CamelotRelayer for the latest price result using historical data to ensure the validity of the result, otherwise it computes the TWAP (Time-Weighted Average Price) and returns the processed result. This contract ignores the importance of maintaining the concept of heartbeat to enforce an upper bound on how frequently updates are added to the history. This opens possibility of finding more profitable to skip a particular update, or front-run an unfavorable update, with a transaction that trades assets at the old price.

```ODGovernor``` contract have functions that collectively manage the on-chain governance process. The contract have settings define the timeframes for voting on proposals, ensuring that token holders have a fair window to participate in decision-making.  A quorum ensures that a minimum number of token holders must participate in the voting process, enhancing the legitimacy of decisions. The contract however does not implement limit for delay and timeline for conducting voting which can be set to longer than expected causing disturbance to the whole governace system. The ```_execute``` function also does not check for parameters limitation as parameters are passed as memory array which can grow extensively leading to gas error.


The``` Vault721``` contract is designed for managing a set of NFT (Non-Fungible Token) vaults. The contract is initialized with a governor's address upon deployment. The governor has certain privileges within the contract. However the contract does not have proper access control on some function that can lead to anyone using the functions which result in improper working of the protocol. The ```_afterTokenTransfer``` also transfer ownership after the transfer of NFTs which also open potential for issues that will cause loss to the new owner.

```ODSafeManager``` contract provides a comprehensive set of methods that facilitate the management of SAFEs as collateralized debt positions.The primary result of function is creation, management, and adjustment of SAFEs within the system. Users can open, close, transfer ownership, and make changes to their SAFEs, ultimately influencing the collateralization ratio. Additionally, the permission management features ensure that only authorized entities can perform specific actions on SAFEs. The lack of effectiveness is linked to ```modifySAFECollateralization``` that allows users to adjust the collateralization of their SAFEs by specifying changes in collateral and debt amounts. This can lead to change the collateral of safe with low price or malicious collateral that may impact the overall stability of the token.

```BasicActions``` contract plays a pivotal role in managing SAFEs by allowing users to increase the debt within a SAFE while transferring COIN tokens to their address. The contract also have functionality of locking collateral in a SAFE to maintain a collateralized position without generating additional debt. The contract have ``` delegateCall``` for every external function which can result in danger outcome if not used with proper caution.


# Centralization Risks
Centralization risks for the Open Dollar project is identified in areas related to ```governance control```, ```ownership restrictions```, ```Safe management```, ```collateral and debt manipulation``` and ```Oracles feeds```.  These issues have greater potential centralization risks that can undermine the core principles of decentralization and trust in the system.

**Governance Control** risk for centralisation existed or the ```ODGovernor``` contract, as a small group or a single entity holds a significant proportion can get hold of governance tokens. This concentration of power would allow them to dictate protocol changes, potentially compromising the ecosystem's decentralization.
**Ownership Restrictions** existed for the ```ODProxy``` contract  as it restricts ownership changes, ensuring security. However, if the ownership of ODProxy becomes highly centralized or controlled by small group which can turn malicious the system's security could be at risk as witnessed from the ```Luna``` stablecoin collapse.

Centralisation risk also existed for **Safe management** and **collateral and debt manipulation** issues existed for ```ODSafeManager``` and ```BasicActions``` contracts play a vital role in managing SAFEs. These contracts have greater potential to be controlled by a limited group that would lead to disproportionate influence over the collateralized positions in the system, potentially leading to unfair liquidations or centralization of assets.
**Oracles Feeds** exists in terms of centralized control by governor which can lead to manipulation if a malicious address is set as oracle that could introduce vulnerabilities, such as price manipulation or data inaccuracies.

The Open Dollar team must focus on developing an effective mechanism that benefits in broad token distribution, enhance governance mechanisms to prevent concentration of power, and ensure that oracles use decentralized and reliable data sources. 

# Systemic Risks
The systemic risks identified for the project of Open Dollar revolves around contracts responsible for maintaining financial stability, governance, and collateralized debt positions. The identified systemic risks mainly revolve around governance vulnerabilities, incorrect price data, and unauthorized access to vital functions. Breach or exploitation in any area would impact stability and overall financial health of the project and must be considered to ensure safe working of the project.

The system risk is identified and related to authorization and control mechanisms if compromised, it could lead to unauthorized debt or surplus handling, jeopardizing the token stability.

Systemic risks also existed for system receiving incorrect or manipulated price data, potentially impacting collateral values and stability fees.
Systemic risks also linked to DAO-managed contracts, if the governance process is not secure, it will lead to malicious changes in collateral types or PID settings.

Systemic risk also exist for managing safe ownership, as an unauthorized entity can gain control of the ```ODProxy```, leading to improper transfer of ownership or disruptions in collateral management.

Systemic risk lies are linked to vulnerabilities that could allow malicious actors to manipulate SAFEs, leading to liquidations, under-collateralization, or loss of assets.

Risks exist for ``` BasicActions``` that include improper debt management, potentially leading to insolvency or misuse of leveraged positions.

# Architecture Recommendations

The recommendation for the ```Open Dollar``` project are developed in area of enhancing the effectiveness related to managing decentralized financial ecosystem with enabling the creation and management of SAFEs (collateralized debt positions) and ensuring the reliable operation of every function throughout the codebase.

The first recommendation is made in terms of ```Governance Flexibility``` as governance contract plays a pivotal role in decision-making process of project. It is necessary to enhance system's adaptability by considering implementing an effective governance structure that supports different voting mechanisms e.g., token-weighted, quadratic voting, or delegated voting. Variation in voting mechanism would allow for flexibility in tailoring governance according to the needs of the project and its users interest in the long term.

The second recommendation is made in terms of improving ```upgradeability``` and ```timelock``` mechanism as it is necessary to implement a robust upgradeability system with effective and measured timelocks to protect against unexpected or unauthorized changes. Smart contract upgrades should go through a well defined time-delayed process to ensure secure transition and prevent undue risk that may lead to loss for project or its user.

The third recommendation is made for enhancing ```Oracle Data Accuracy``` by considering diverse data sources and implementing mechanisms to verify and cross-reference data for increased reliability. Oracles feeds also need to be based on having reliable historical data based on heartbeat mechanism that ensures prevention of manipulation of funds using stale data. 

The last recommendation is made for ```emergencyshutdown``` process that must be developed a well defined and clear to safeguard user funds and system stability in the event of unforeseen circumstances. The shutdown must consider identifying areas of risk that will lead to stopping of the whole system like oracle out-dation, unauthorised access and system malfunctioning due to any state changes taking place for the project.

# Time Spent
The time spent for reviewing and analysing the code base is around 30 hours.

The audit review helps in identifying 01 high and 02 medium issues related to issues of re-entrancy, oracle manipulation and stale data feed.

The analysis benefits in assessing the project and identified various security pitfalls and areas of improvement that is necessary for smooth and secure working of project in the long term.



### Time spent:
30 hours