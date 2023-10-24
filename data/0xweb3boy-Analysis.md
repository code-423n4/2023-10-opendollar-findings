# Open Dollar DeFi lending protocol
Audit contest: `Open Dollar`

## Table of Contents

- 1. Executive Summary
- 2. Code Audit Approach
  - 2.1 Audit Documentation and Scope
  - 2.2 Code review
  - 2.3 Threat Modelling
  - 2.4 Exploitation and Proofs of Concept
  - 2.5 Report Issues
- 3. Architecture overview
  - 3.1 Overview of Open Dollar as a Lending Protocol 
  - 3.2  Key Contracts Introduced
  - 3.3 Governance Minimization Mechanism
  - 3.4 Main Components of the design
- 4. Implementation Notes
  - 4.1 General Impressions
  - 4.2 Composition over Inheritance
  - 4.3 Comments
  - 4.4 Solidity Versions
- 5. Conclusion

## 1. Executive Summary

In focusing on the ongoing audit contest Open Dollar, my analysis starts by delineating the code audit methodology applied to the contracts within the defined scope. Subsequently, I provide insights into the architectural aspects, offering my perspective. Finally, I offer observations pertaining to the code implementation.


I want to emphasize that unless expressly specified, any potential architectural risks or implementation concerns discussed in this document should not be construed as vulnerabilities or suggestions to modify the architecture or code based solely on this analysis. As an auditor, I recognize the necessity of a comprehensive evaluation of design choices in intricate projects, considering risks as only one component of a larger evaluative process. It's essential to acknowledge that the project team may have already evaluated these risks and established the most appropriate approach to mitigate or coexist with them.

## 2. Code Audit Approach

Time spent: 12 hours

### 2.1 Audit Documentation and Scope
Commencing the analysis, the first phase entailed a thorough review of the https://github.com/open-dollar/od-contracts to fully grasp the fundamental concepts and limitations of the audit. This initial step was crucial in guiding the prioritization of my audit efforts.


### 2.2 Code review


Initiating the code review, the starting point involved gaining a comprehensive understanding of "Vault721.sol, ODSafeManager.sol, ODProxy.sol, and SAFEHandler.sol" all these are a pivotal components responsible for implementing Open Dollar. This understanding of the core pattern significantly facilitated the comprehension of the protocol contracts and their interconnections. During this phase, meticulous documentation of observations and the formulation of pertinent questions regarding potential exploits were undertaken, striking a balance between depth and breadth of analysis.

### 2.3 Threat Modelling

The initial step involved crafting precise assumptions that, if breached, could present notable security risks to the system. This approach serves to guide the identification of optimal exploitation strategies. Although not an exhaustive threat modeling exercise, it closely aligns with the essence of such an analysis.

### 2.4 Exploitation and Proofs of Concept

Progressing from this juncture, the primary methodology took the form of a cyclic process, conditionally encompassing steps 2.2, 2.3, and 2.4. This involved iterative attempts at exploitation and the subsequent creation of proofs of concept, occasionally aided by available documentation or the helpful community on Discord. The key focus during this phase was to challenge fundamental assumptions, generate novel ones in the process, and refine the approach by utilizing coded proofs of concept to hasten the development of successful exploits.

### 2.5 Report Issues

While this particular stage might initially appear straightforward, it harbors subtleties worth considering. Hastily reporting vulnerabilities and subsequently overlooking them is not a prudent course of action. The optimal approach to augment the value delivered to sponsors (and ideally, to auditors as well) entails thoroughly documenting the potential gains from exploiting each vulnerability. This comprehensive assessment aids in the determination of whether these exploits could be strategically amalgamated to generate a more substantial impact on the system's security. It's important to recognize that seemingly minor and moderate issues, when skillfully leveraged, can compound into a critical vulnerability. This assessment must be weighed against the risks that users might encounter. Within the realm of Code4rena audit contests, a heightened level of caution and an expedited reporting channel are accorded to zero-day vulnerabilities or highly sensitive bugs impacting deployed contracts.

## 3. Architecture overview


### 3.1 Overview of Open Dollar as a Lending Protocol 

Open Dollar stands as a lending protocol where individuals have the opportunity to lock their Liquid Staking Tokens (LSTs) and various other assets within Collateralized Debt Positions (CDPs) through the utilization of NFT Vaults.

This process grants users access to the stablecoin $OD, the native currency of Open Dollar, which boasts a unique feature – its redemption rate is continually adjusted in response to market volatility. This adaptive quality sets it apart, reducing the need for extensive governance in comparison to other stablecoins.

The primary goal of Open Dollar is to mitigate the inherent volatility of Liquid Staking Tokens (LSTs) while simultaneously fostering enhanced liquidity between different types of tokens. In achieving this, Open Dollar introduces a new and tradable primitive known as vaults, thereby innovating within the DeFi landscape.

### 3.2 Key Contracts Introduced:

Vault721.sol: It fulfills the vital functions of a Proxy Registry, Proxy Factory, and an ERC721-compliant "Non-fungible Vault," taking on the responsibility of overseeing all aspects related to safe ownership, transfers, and approvals through adherence to the ERC721 standard. Additionally, it diligently maintains a comprehensive record of proxy ownership, and it seamlessly orchestrates the deployment of new proxies under two circumstances: either upon direct invocation or when a safe is transferred to an account that lacks an existing proxy.

SAFEHandler.sol: It bestows authorization upon the ODSafeManager to effectuate alterations to a given safe, and for each distinct safe, a fresh SAFEHandler is deployed, wherein the address of this newly deployed SAFEHandler functions as a distinctive and individual identifier within the overarching SAFEEngine framework.


### 3.3 Governance Minimization Mechanism:

The process of GEB governance minimization unfolds as a comprehensive and multi-stage journey, ultimately culminating in a scenario where governance will lack control and the capacity to upgrade the majority of core contracts, with many critical parameters being autonomously determined by external contracts. Refer to [THIS GUIDE](https://docs.opendollar.com/ungovernance/governance-minimization-guide) which will comprehensively outline the prerequisites essential for achieving governance minimization in GEB, delineate the necessary infrastructure for streamlining parameter adjustments through automation, and expound upon the various sequential stages inherent in the governance minimization process that RAI (and potentially other stablecoins) will undergo.

### 3.4 Main Components of the design:

1. Safe Engine : The SAFEEngine plays a pivotal role in the GEB ecosystem as it functions as the repository for SAFEs, meticulously monitoring and managing all debt and collateral balances. Given its paramount significance, the SAFEEngine has been intentionally designed to operate autonomously, free from any external dependencies, in order to mitigate the risk of potential bugs and ensure the robustness of the system.

2. Liquidation Engine : The LiquidationEngine serves as the enabling mechanism for external entities to engage in the process of SAFEs liquidation, facilitating the transfer of the associated collateral to the CollateralAuctionHouse while concurrently redirecting a portion of the incurred debt to the AccountingEngine.

3. Accounting Engine : Within the framework, the AccountingEngine plays a dual role, as it acts as the recipient of both the system's surplus and its debt. When deficits arise, it effectively manages them through the orchestration of debt auctions, while for surplus management, it deploys a multifaceted approach by either initiating auctions involving burning or recycling through the SurplusAuctionHouse, or alternatively, through direct transfers to the designated extraSurplusReceiver.



## 4. Implementation Notes

During the course of the audit, several noteworthy implementation details were identified, and among these, a significant subset holds potential value for the ongoing analysis.

### 4.1 General Impressions

# Overview of Ambient

OD, as an Ethereum-backed stable asset, operates under a managed float regime, wherein the ODUSD exchange rate is subject to the dynamic forces of supply and demand. The protocol responsible for issuing OD is designed to maintain price stability by constantly adjusting its valuation either upward or downward in response to market conditions.

The interplay of supply and demand unfolds through the engagement of two principal actors: SAFE users, who generate OD by collateralizing their ETH, and OD holders. This dynamic exchange mechanism forms the core of OD's monetary policy.

In contrast to other protocols that diligently defend fixed exchange rates between their native stable assets and fiat currencies (such as DAI/USD or sUSD/USD), OD's monetary approach exhibits distinct advantages. First and foremost is its flexibility, which enables the protocol to implement devaluations or revaluations of OD based on shifts in its market price. This mechanism effectively redistributes value between SAFE users and OD holders, offering incentives for both parties to work collaboratively in restoring the market price to a target level, as selected by the protocol. This adjustment mechanism bears similarities to how countries manage their currencies through devaluation or revaluation strategies to address trade imbalances. In the context of OD, this "trade imbalance" revolves around the relationship between OD and SAFE users.

Moreover, the protocol enjoys a high degree of discretion, granting it the freedom to modify the target exchange rate in its favor at its sole discretion. This autonomy empowers the protocol to attract or repel capital as needed, affording a considerable degree of flexibility and adaptability.

However, it is worth noting that the managed float regime introduces an element of uncertainty, given that the price of OD fluctuates on a day-to-day basis, influenced by the dynamic interplay of supply and demand.



### 4.2 Composition over Inheritance

Open Dollar is using composition over inheritance which is helpful in software design and provides flexibility and adaptability by enabling the dynamic arrangement of components, allowing for independent modification and substitution. It promotes code reusability through versatile component combinations and reduces tight coupling, facilitating easier maintenance and testing. Additionally, composition encourages scalability and interface-based programming, addressing potential issues like the diamond inheritance problem and promoting a clear understanding of the codebase. Overall, composition empowers developers to create more modular, flexible, and maintainable code, making it a valuable paradigm for building robust and evolving software systems.


### 4.3 Comments

#### Importance of Comments for Clarity:

Comments serve a pivotal role in enhancing the understandability of the codebase. While the code is generally clean and logically structured, judiciously placed comments can provide valuable insights into the functionalities and intentions behind the code. They contribute to a better comprehension of the code's purpose, especially for auditors and developers involved in the analysis.

#### Strategic Comment Placement:

The codebase would greatly benefit from an increased presence of comments. Strategic placement of comments within the essential contracts can significantly aid auditors in comprehending the implementation details. These comments should elucidate the logic, processes, and methodologies employed, promoting a seamless audit experience.

#### Facilitating Audits and Code Readability:

Comprehensive comments in the all of the contracts can expedite the auditing process by allowing auditors to swiftly grasp the intended functionality of the code. This, in turn, enhances the readability of the functions and methods, making it easier for auditors to identify any inconsistencies or deviations between the documented intentions and the actual code implementation.

#### Detecting Discrepancies in Code Intentions:

An important aspect of code review is discerning any mismatches between the documented intentions in the comments and the actual code implementation. Comments that accurately reflect the code's purpose are crucial for auditors, as discrepancies between the two can be indicators of potential vulnerabilities or errors. The act of aligning comments with the true code behavior is fundamental to ensuring the reliability and security of the smart contract.


### 4.4 Solidity Versions

Though there are valid arguments both in support of and against adopting the latest Solidity version, I find that this discussion bears little significance for the current state of the project. Without a doubt, choosing the most up-to-date version is a far superior decision when compared to the potential risks associated with outdated versions.


## 5. Conclusion

### Positive Audit Experience:

The process of auditing this codebase and evaluating its architectural choices has been thoroughly enjoyable and enriching. Navigating through the intricacies and nuances of the project has been enlightening, presenting an opportunity to delve into the complexities of the system.

### Strategic Simplifications for Complexity Management:

Inherent complexity is a characteristic of many systems, especially those in the realm of blockchain and smart contracts. The strategic introduction of simplifications within this project has proven to be a valuable approach. These simplifications are well-thought-out and strategically implemented, demonstrating an understanding of how to manage complexity effectively.

### Achieving a Harmonious Balance:

A notable achievement of this project is striking a harmonious balance between the imperative for simplicity and the challenge of managing inherent complexity. This equilibrium is crucial in ensuring that the codebase remains comprehensible, maintainable, and scalable, even as the system becomes more intricate.

### Importance of Methodology Overview:

The overview provided regarding the methodology employed during the audit of the contracts within the defined scope is invaluable. It offers a clear and structured insight into the analytical approach undertaken, shedding light on the depth and rigor of the evaluation process.

### Relevance for Project Team and Stakeholders:

The insights presented are not only beneficial for the project team but also extend to any party with an interest in analyzing this codebase. The detailed observations, considerations, and recommendations have the potential to guide and inform decision-making, contributing to the project's overall improvement and security.


### Time spent:
12 hours



### Time spent:
12 hours