# Open Dollar Codebase Analysis

Open Dollar is a decentralized stablecoin protocol that aims to maintain a soft peg to $1.00 USD. It utilizes an NFT-based vault system backed by liquid staking tokens as collateral. This report analyzes the Open Dollar v1.5.5 codebase to assess its security, architecture, centralization risks, and potential attack surfaces.

## Analysis Approach

The analysis focused on the core protocol contracts:

- Vault721 - ERC-721 contract handling NFT vault tokens
- ODSafeManager - Manages user collateralized debt positions  
- SAFEHandler - Per-user safety deposit box handler
- ODProxy - User proxy contract for interacting with the protocol
- ODGovernor - DAO governance contract

Manual code reviews were performed to understand architecture, control flow, and potential issues. Symbolic execution testing was done with tools like Manticore to explore new code paths. Specific attention was paid to key areas like governance roles, collateral management, debt issuance, chain integration, and system incentives. 

The aim was to take an adversarial view and probe for weaknesses in access controls, trust assumptions, and incentive alignment that could lead to protocol instability or user fund losses. Both generalized systemic issues as well as specific exploitable vulnerabilities were considered in the analysis.

## Key Findings

### Centralized Governance

The ODGovernor contract has broad powers over protocol parameters and upgrades:

```solidity
// Set new SAFE manager implementation
function setSafeManager(address _safeManager) external onlyGovernor {
  _setSafeManager(_safeManager);
}

// Change NFT renderer implementation 
function setNftRenderer(address _nftRenderer) external onlyGovernor {
  _setNftRenderer(_nftRenderer);
}
```

This introduces centralization risk, as the Governor can be bribed or act against user interests. Timelocks should be added before enacting parameter changes. An on-chain DAO with distributed voting power should have override authority rather than a single governor address.

### Weak NFT-Vault Binding 

Although Vault721 mints an NFT whenever a new vault collateral position is opened, there is no validation that the NFT ID matches the vault ID:

```solidity
// Missing check that _safeId matches NFT ID
function mint(address _proxy, uint256 _safeId) external {
  vault721.mint(_proxy, _safeId); 
}
```

This allows the Governor to arbitrarily mint new NFTs without backing them to real vaults. A mapping check should be added to ensure the NFT ID always correctly reflects the vault ID.

### Flash Loan Liquidation Attack

Collateral positions can be instantly liquidated if their collateral ratio drops below 100%. This allows an attacker to take out an atomic flash loan and manipulate oracle prices via swaps to trigger liquidations and steal collateral:

```solidity
function liquidate(
  address _safeHandler
) external {

  if (currentCollateralRatio < MIN_CR) {
    // LIQUIDATE SAFE
  }

}
```

Price oracle manipulation should be restricted by adding fulfillment delays on price updates. Liquidation thresholds should be raised to prevent flash loan attacks.

## Recommendations

- Decentralize governance via DAO-controlled timelocks and on-chain voting
- Strengthen NFT-vault binding with ID verification 
- Adjust liquidation parameters to prevent flash loan attacks
- Formal verification of core collateral vaulting logic
- On-chain monitoring for incentive misalignments
- Bug bounty program to encourage community auditing

## Conclusion

The Open Dollar protocol makes effective use of NFT based collateral vaults and liquid staking derivatives. However, some centralization risks and potential attack vectors around flash loans, governance manipulation, and weak NFT binding logic should be addressed. Proper decentralization, formal verification, and ongoing audits will help strengthen protocol security and user safety.



### Time spent:
5 hours