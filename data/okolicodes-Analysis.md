# Analysis - Open Dollar
# Summary

| List |Head |Details|
|:--|:----------------|:------|
|1 | Overview of Open Dollar | overview of the key components and features of Open Dollar   |
|2 | Approach taken in evaluating the codebase | Process and steps I followed  |
|3 | Centralization risks | Concerns associated with centralized systems |
|4 | Mechanism review | Process of evaluating the design and implementation of a mechanism  |
|5 | Systemic risks | Possible systemic risks as per analysis |
|6 |Time spent on analysis  | The Over all time spend for this reports |

## 1. Overview
> ``Open Dollar`` is a floating $1.00 pegged ``stablecoin`` that is backed by ``Liquid Staking Tokens`` with ``NFT controlled vaults``. It is deployed on the ``Arbitrum`` chain and it is deployed using the ``GEB`` framework and to summarize, the ``GEB`` is a framework that issues ``Stablecoin``. 

The ``Open Dollar`` Stablecoin is usually minted whenever debt is created. ``Open Dollar`` ties the debt created to a ``SAFE`` and the ``SAFE IDs`` is usually linked to an ``NFT ID`` in  the ``NFV``  

 Open Dollar eliminates the need for an account based debt system by introducing ``NFT Vaults`` that give room for the creation of ``NFT`` marketplaces and transferring debt using ``NFTs``. 

## 2. Approach taken in evaluating the codebase

- ``Preliminary analysis``: I read the contest ``Readme.md`` file and took the following notes:

-  ``Open Dollar`` learnings,
  -  Composition 
  -  Test coverage is only 80.%
  -  Deals with transfers of debts in form of ``NFTs`` through the ``NFV``
 

- Areas I focused on
   - Access Control 
   - Transfer of ownership related issues when selling the ``Vault``
   - Minting debt on an already sold ``SAFE``.
  

- ``High-level overview`` : I analyzed the ``overall codebase`` in one iteration to get a high-level understanding of the ``code structure`` and ``functionality``.

- ``Documentation review`` : I studied the ``documentation`` to understand the purpose of ``each contract``, its ``functionality``, and how it is ``connected`` with other contracts.

- ``Testing setup`` : I set up my ``testing environment`` and ran the tests to ensure that ``all tests passed``. 

- ``Detailed analysis`` :  I began by conducting a ``detailed analysis`` of the codebase, going through it ``line by line``. I meticulously took notes to prepare questions for the ``sponsors``. I used ``@audit`` tags to identify and flag vulnerable and ``weak parts`` in the codebase. Subsequently, I delved into a thorough analysis and conducted all the necessary ``unit`` and ``fuzz tests`` to ensure the protocol functions as intended.

#### Core contracts I reviewed

- ``Vault721``: This Serves as the Proxy Registry, Proxy Factory, and the ERC721 "Non-fungible Vault". Manages all safe ownership, transfers, and approvals via the ERC721 standard.

- ``ODSafeManager`` : The Safe Manager only allows the Vault721 contract to move a safe.

- ``ODProxy``: This is the only way in which a user can interact with their SAFE. The purpose of this is to ensure that only the Vault721 contract has the ability to transfer a safe.

- ``SAFEHandler`` : This issues permission to the ODSafeManager to make modifications to a safe.

## 3. Centralization risks

The ``Open Dollar`` protocol leaves little to no room for centralization risks.

## 4.Mechanism review
The ``Open Dollar`` is a well-designed and implemented system for ``stable coins`` backed by ``LST`` The protocol implements the GEB framework and is deployed on a Layer 2 chain known as Arbitrum.

## 5. Systemic Risks

These risks are rooted in technical and operational aspects, and they have the potential to impact the protocol's stability and reliability.
Based on an analysis of the protocol's documentation and codebase implementations, I did not see any potential systemic risks.

## 6. Time spent on analysis 
``30 Hours``


### Time spent:
30 hours