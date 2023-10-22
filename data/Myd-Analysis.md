## Overview

Open Dollar is a decentralized stablecoin protocol that aims to maintain a soft peg to $1.00 USD. It utilizes a collateralized debt position (CDP) model where users deposit collateral assets into isolated positions called "vaults" to mint the Open Dollar stablecoin. 

Open Dollar has modified the standard CDP model by tying vault positions to non-fungible tokens (NFTs). This allows the vaults to be freely tradable. The protocol is designed to be used on Arbitrum for scalability.

## Architecture

The core contracts in the Open Dollar protocol are:

- **Vault721** - Implements ERC-721 standard and manages NFT vault tokens
- **ODSafeManager** - Manages CDP vaults and debt positions 
- **SAFEEngine** - Handles CDP stability fees and liquidations
- **ODProxy** - Proxy contract used by users to interact with protocol

### Vault Architecture

Each user has an ODProxy contract that handles their wallet interactions. Vaults are created via the ODSafeManager and assigned a unique ERC-721 token ID minted by Vault721. This ties the vault ownership to the NFT.

The NFTs can be freely traded, transferring control and ownership of the vaults. The proxy architecture enables the protocol to enforce controls on vault usage while keeping NFT ownership transferable.

![Vault architecture diagram](https://i.imgur.com/Fzj2qaY.png)

### Potential Issues

- Centralized governance through Vault721 owner role
**Centralized governance**

The Vault721 owner role has centralized control: [#L18](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L18), [#L133-L135](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L133-L135), [#L40-L43](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L40-L43)

```solidity
// Vault721.sol

address public governor; 

function setNftRenderer(address renderer) external onlyGovernor {
  // Set critical contract logic
}

  modifier onlyGovernor() {
    if (msg.sender != governor) revert NotGovernor();
    _;
  }
  // ...
}
```

The governor can set arbitrary contract implementations like the NftRenderer.


- Liquidation ratio thresholds need to be battle-tested 
**Liquidation thresholds**

Liquidation ratios are set in the SAFEEngine:

```solidity
// SAFEEngine.sol

uint constant LIQ_RATIO = 1.5; // 150%

function liquidate(uint safe) external {
  if (getCollateralRatio(safe) < LIQ_RATIO) { 
    // Liquidate
  }
}
```

The 1.5 ratio could be too high or low. Testing is needed to set proper levels.

- Reentrancy concerns around external calls
**Reentrancy concerns** 

External calls made before modifiers: [Rent](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L167-L179)

```solidity
// ODSafeManager.sol

function transferCollateral(uint safe, address dst, uint amt) external {

  SAFEEngine(engine).transferCollateral(safe, dst, amt);

  authenticated(safe); // Modifier after external call
}
```

This could allow reentrancy attacks.
- Setting proper debt limits on vault creation

## Mechanism Analysis

Users can deposit supported collateral assets into their vault to mint Open Dollar stablecoins against their collateral. This allows leveraging the collateral to access liquidity.

The ratio of collateral to generated stablecoins must be maintained to avoid liquidation. If the collateral ratio drops too low, the vault position can be liquidated. 

To maintain the peg, stability fees are charged on debt positions. This incentivizes closing debt positions to avoid paying growing fees. The protocol also implements discount rates on collateral redemption to encourage maintaining positions.

## Risk Evaluation

### Positive

- Innovative tradable vault model using NFT positions
- Good flexibility and modularity in code architecture
- Detailed events for transparency and monitoring 
- Overall sound mathematical foundations

### Concerns

- Highly complex system with many interdependent parameters
- Difficult to model precise risks across range of scenarios
- Stability relies heavily on liquidity for discount rates 
- Centralized control via owner admin keys
- Lack of formal verification and proofs

## At the end

The Open Dollar protocol represents an interesting new model for collateralized stablecoins. The architecture using NFT vaults enables powerful new use cases around trading and transferring positions. 

However, the added complexity also increases the chance of unforeseen edge cases that could threaten stability. Formal verification of the core mechanisms would give higher assurance. Overall the system shows promise but requires extensive real-world testing and monitoring before considering large-scale adoption.

### Time spent:
4 hours