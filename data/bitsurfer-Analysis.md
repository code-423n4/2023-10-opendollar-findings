# Overview of Protocol

Open Dollar is a pegged stablecoin backed by LST with NFT controlled vaults deployed on Arbitrum. This protocol is based or forked of HAI stablecoin on Optimism, which is based on GEB framework (to issue stablecoin) which is also modified work of Maker DAO's [Multi Collateral Dai](https://github.com/makerdao/dss).

OD's feature include, (but not limited to), NFV (Non-Fungible Vault) which ties CDP ownership to a specific NFT, rather than using the traditional account-based ownership for CDPs.

# Audit Methodology

Since the dev of Open Dollar clearly explain that this is a modified work of HAI stablecoin, GEB framework, Maker DAO's MCD, the best way to audit is to understand what are some changes from the parent fork.

Looking at the parent fork also a known project, it's normal to read some information of those projects and find a previous audits and see what are some issue from this similar codebase.

The Dev also mention that auditors just need to focus on the changes they made, assuming the rest (complete architecture) is a well-audited.

Some of the files I'm focusing to but not limited to are:

- proxies/Vault721.sol
- proxies/ODSafeManager.sol
- proxies/ODProxy.sol
- proxies/SAFEHandler.sol

The rest of in scope files are basically a slightly modified version of existing parent project. 

# Code Quality

The code quality of overall project is well-structured, because it's basically a fork of a fork, which is a battle-tested code. Rooting as a project of Maker DAO, this definitely a best-practice code result.

The OD features including the NFV Vault, is a standard implementation of common ERC721 without any weird coding pattern. 

# Architecture Design Analysis & Recommendation

When I use a diff tools to see the changes of OD compared to HAI-on-op project, there are not so many differences. I don't have any comment on their overall design architecture of the protocol, but I noticed the OD changes are not 'weird', it's simply adding custom Camelot Relayer, adding Gov contract, and using NFV vault contract instead of internal accounting balance.

My recommendation to the project maybe not much, but I suggest for OD to implement more features to attract users. Forking a stablecoin project and add a simple feature may not attract users.

# Centralization & Systemic Risks

There is a specific Governor (ODGovernor.sol) contract which is not exist from parent fork project is a good way to minimize the centralization issue, which is a good feature to have. This Governor contract is just a standard OpenZeppelin governor contract which is just enough to work as a DAO-managed contract which can modify protocol parameters, eg. add new collateral types and change PID settings.

# Conclusion, Additional Information and Suggestions

After looking at the contract code, it's clear that it's mostly based on another stablecoin project, with a few extra features. It's good that the developer openly tells us that this project comes from HAI and has some changes. This means we, as auditors, only need to focus on these changes and extras, without having to understand the whole thing.

But, thinking about what the developer said, it makes me wonder if we're really getting the full picture. If auditors are only looking at a small part of the code, it might not be a thorough check, even if the original code has been checked before. My suggestion is to let auditors check the main code of the stablecoin. This way, we might find problems that were missed in the past checks of the modified version. This would make the project more secure and reliable overall.

### Time spent:
24 hours