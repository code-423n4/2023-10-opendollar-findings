# Low findings

## 1

### Vulnerability details

The `@openzeppelin/contracts` used in the current project is v4.8.2 and a vulnerability in the `GovernorCompatibilityBravo.sol` was found where there is an authorization check missing. The attack can be performed as followed:
By frontrunning the creation of a proposal, an attacker can become the proposer and gain the ability to cancel it. The attacker can do this repeatedly to try to prevent a proposal from being proposed at all.

This impacts the `GovernorCompatibilityBravo.sol` contract since v4.3.0.

Here is a link of the discovered issue: https://security.snyk.io/vuln/SNYK-JS-OPENZEPPELINCONTRACTS-5672116

### Recommendations

Upgrade `@openzeppelin/contracts` to version 4.9.1 or higher.