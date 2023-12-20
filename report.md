---
sponsor: "Open Dollar"
slug: "2023-10-opendollar"
date: "2023-12-20"
title: "Open Dollar"
findings: "https://github.com/code-423n4/2023-10-opendollar-findings/issues"
contest: 297
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the Open Dollar smart contract system written in Solidity. The audit took place between October 18—October 25 2023.

## Wardens

80 Wardens contributed reports to Open Dollar:

  1. [hals](https://code4rena.com/@hals)
  2. [MrPotatoMagic](https://code4rena.com/@MrPotatoMagic)
  3. [yashar](https://code4rena.com/@yashar)
  4. [klau5](https://code4rena.com/@klau5)
  5. [nmirchev8](https://code4rena.com/@nmirchev8)
  6. [Saintcode\_](https://code4rena.com/@Saintcode_)
  7. [T1MOH](https://code4rena.com/@T1MOH)
  8. [falconhoof](https://code4rena.com/@falconhoof)
  9. [tnquanghuy0512](https://code4rena.com/@tnquanghuy0512)
  10. [Haipls](https://code4rena.com/@Haipls)
  11. [kutugu](https://code4rena.com/@kutugu)
  12. [hunter\_w3b](https://code4rena.com/@hunter_w3b)
  13. [immeas](https://code4rena.com/@immeas)
  14. [ZanyBonzy](https://code4rena.com/@ZanyBonzy)
  15. [pep7siup](https://code4rena.com/@pep7siup)
  16. [0xprinc](https://code4rena.com/@0xprinc)
  17. [fouzantanveer](https://code4rena.com/@fouzantanveer)
  18. [twicek](https://code4rena.com/@twicek)
  19. [0x6d6164616e](https://code4rena.com/@0x6d6164616e)
  20. [perseus](https://code4rena.com/@perseus)
  21. [twcctop](https://code4rena.com/@twcctop)
  22. [Baki](https://code4rena.com/@Baki) ([Viraz](https://code4rena.com/@Viraz) and [supernova](https://code4rena.com/@supernova))
  23. [Tendency](https://code4rena.com/@Tendency)
  24. [Arz](https://code4rena.com/@Arz)
  25. [lsaudit](https://code4rena.com/@lsaudit)
  26. [xAriextz](https://code4rena.com/@xAriextz)
  27. [COSMIC-BEE-REACH](https://code4rena.com/@COSMIC-BEE-REACH) ([pxng0lin](https://code4rena.com/@pxng0lin), [0xarno](https://code4rena.com/@0xarno), and [solsaver](https://code4rena.com/@solsaver))
  28. [Krace](https://code4rena.com/@Krace)
  29. [0xhacksmithh](https://code4rena.com/@0xhacksmithh)
  30. [bitsurfer](https://code4rena.com/@bitsurfer)
  31. [spark](https://code4rena.com/@spark)
  32. [josephdara](https://code4rena.com/@josephdara)
  33. [btk](https://code4rena.com/@btk)
  34. [phoenixV110](https://code4rena.com/@phoenixV110)
  35. [0xMosh](https://code4rena.com/@0xMosh)
  36. [nican0r](https://code4rena.com/@nican0r)
  37. [0xVolcano](https://code4rena.com/@0xVolcano)
  38. [mrudenko](https://code4rena.com/@mrudenko)
  39. [0xPsuedoPandit](https://code4rena.com/@0xPsuedoPandit)
  40. [Stormreckson](https://code4rena.com/@Stormreckson)
  41. [ni8mare](https://code4rena.com/@ni8mare)
  42. [Kaysoft](https://code4rena.com/@Kaysoft)
  43. [fibonacci](https://code4rena.com/@fibonacci)
  44. [holydevoti0n](https://code4rena.com/@holydevoti0n)
  45. [JCK](https://code4rena.com/@JCK)
  46. [Bughunter101](https://code4rena.com/@Bughunter101)
  47. [Beosin](https://code4rena.com/@Beosin)
  48. [clara](https://code4rena.com/@clara)
  49. [jauvany](https://code4rena.com/@jauvany)
  50. [SAAJ](https://code4rena.com/@SAAJ)
  51. [wei3erHase](https://code4rena.com/@wei3erHase)
  52. [0xweb3boy](https://code4rena.com/@0xweb3boy)
  53. [Myd](https://code4rena.com/@Myd)
  54. [0xbrett8571](https://code4rena.com/@0xbrett8571)
  55. [HChang26](https://code4rena.com/@HChang26)
  56. [merlin](https://code4rena.com/@merlin)
  57. [0xmystery](https://code4rena.com/@0xmystery)
  58. [m4k2](https://code4rena.com/@m4k2)
  59. [0xAadi](https://code4rena.com/@0xAadi)
  60. [niki](https://code4rena.com/@niki)
  61. [0xsurena](https://code4rena.com/@0xsurena)
  62. [cryptothemex](https://code4rena.com/@cryptothemex)
  63. [0xWaitress](https://code4rena.com/@0xWaitress)
  64. [Giorgio](https://code4rena.com/@Giorgio)
  65. [ge6a](https://code4rena.com/@ge6a)
  66. [Greed](https://code4rena.com/@Greed)
  67. [0xlemon](https://code4rena.com/@0xlemon)
  68. [0x11singh99](https://code4rena.com/@0x11singh99)
  69. [naman1778](https://code4rena.com/@naman1778)
  70. [dharma09](https://code4rena.com/@dharma09)
  71. [unique](https://code4rena.com/@unique)
  72. [nailkhalimov](https://code4rena.com/@nailkhalimov)
  73. [0xDemon](https://code4rena.com/@0xDemon)
  74. [okolicodes](https://code4rena.com/@okolicodes)
  75. [Al-Qa-qa](https://code4rena.com/@Al-Qa-qa)
  76. [8olidity](https://code4rena.com/@8olidity)
  77. [eeshenggoh](https://code4rena.com/@eeshenggoh)

This audit was judged by [MiloTruck](https://code4rena.com/@MiloTruck).

Final report assembled by PaperParachute.

# Summary

The C4 analysis yielded an aggregated total of 17 unique vulnerabilities. Of these vulnerabilities, 2 received a risk rating in the category of HIGH severity and 15 received a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 25 reports detailing issues with a risk rating of LOW severity or non-critical. There were also 8 reports recommending gas optimizations.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 Open Dollar repository](https://github.com/code-423n4/2023-10-opendollar), and is composed of 11 smart contracts written in the Solidity programming language and includes 998 lines of Solidity code.

In addition to the known issues identified by the project team, a Code4rena bot race was conducted at the start of the audit. The winning bot, **vuln-detector** from warden [oualidpro](https://code4rena.com/@oualidpro), generated the [Automated Findings report](https://gist.github.com/code423n4/ff4868d6946db4c2242a7c5594baece6) and all findings therein were classified as out of scope.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (2)
## [[H-01] Incorrect calculations for Surplus Auction creation cause massive surplus imbalances](https://github.com/code-423n4/2023-10-opendollar-findings/issues/26)
*Submitted by [falconhoof](https://github.com/code-423n4/2023-10-opendollar-findings/issues/26), also found by pep7siup ([1](https://github.com/code-423n4/2023-10-opendollar-findings/issues/384), [2](https://github.com/code-423n4/2023-10-opendollar-findings/issues/368)), [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/281), [0x6d6164616e](https://github.com/code-423n4/2023-10-opendollar-findings/issues/206), [twicek](https://github.com/code-423n4/2023-10-opendollar-findings/issues/186), [immeas](https://github.com/code-423n4/2023-10-opendollar-findings/issues/174), [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/83), [Krace](https://github.com/code-423n4/2023-10-opendollar-findings/issues/78), [Baki](https://github.com/code-423n4/2023-10-opendollar-findings/issues/295), [bitsurfer](https://github.com/code-423n4/2023-10-opendollar-findings/issues/214), and [Beosin](https://github.com/code-423n4/2023-10-opendollar-findings/issues/118)*

There are two big issues in `AccountingEngine.sol::auctionSurplus()` when calculating values for the creating a Surplus Auction; specifically in [Lines 213 - 217](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L213-L217).

1.  The first issue is the `if` statement at [line 213](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L213-L217) which incorrectly checks if `_params.surplusTransferPercentage` is less than `ONE_HUNDRED_WAD` when it should check if it is less than `WAD`.

```

        if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD) {
```

The maximum value for `_params.surplusTransferPercentage`, as checked by the function at [line 199](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L199), is `1e18` so the check at [line 213](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L213-L217) will always return `TRUE`.

However, the check should return `FALSE` when `_params.surplusTransferPercentage` is `1e18` or `100%` because in that case an auction shouldn't be created; rather the full `surplusAmount` should be transferred to `extraSurplusReceiver` in the next code block.

2.  The second issue is the use of `ONE_HUNDRED_WAD` to calculate `_amountToSell` at [Line 215](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L213-L217) which results in a hugely inflated figure in the newly created surplus auction.

The use of `ONE_HUNDRED_WAD` causes the calculated figure to be `100 times` greater than it should be.

        _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage)

### Impact

Issue 1. For the first issue; when `_params.surplusTransferPercentage` is `100%`, a ghost surplus Auction will be created and the entire `surplusAmount` amount will also be sent to the `extraSurplusReceiver` essentially double-counting a large amount of the surplus. This double accounting can destabilise the system and lead to underflows elsewhere.

Issue 2. Surplus auctions are created with massively inflated figures for `_amountToSell`. This has the potential to cause massive price imbalances and crash the protocol. There is potential here for a malicious actor to leverage the vulnerability to their advantage by creating lots of false surplus in system coins which they purchase cheaply.

### Proof of Concept

The following combines both issues into one PoC to show the worst case scenario.

Given the following values and assuming the initial all checks pass in function before reaching Line 213 `AccountingEngine.sol::auctionSurplus()`:

    WAD = 1e18
    surplusTransferPercentage = 1e18 (representative of 100%)
    surplusAmount = 3e18
    ONE_HUNDRED_WAD = 100e18

The following condition at Line 213 will always return TRUE:

    if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD)

So an auction will be created with `_amountToSell` 100 times higher than it should be:

        _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage),

    Returns 297e18; where coin surplusAmount is only 3e18:
    _amountToSell = 3e18 * (100e18 - 1e18) / 1e18
                  = 3e18 * 99e18 / 1e18
                  = 297e18
                  = 297 WAD

Following this, a coin amount of `surplusAmount` is also sent to the `extraSurplusReceiver` calculated in [Lines 224-231](https://github.com/open-dollar/od-contracts/blob/dev/src/contracts/AccountingEngine.sol#L224-L231) which is actually the intended behaviour.

### Tools Used

Foundry

### Recommended Mitigation Steps

Update the function on the two affected lines to use `WAD` instead of `ONE_HUNDRED_WAD` as:

<details>

```diff
  function auctionSurplus() external returns (uint256 _id) {
    if(_params.surplusTransferPercentage > WAD) revert AccEng_surplusTransferPercentOverLimit();
    if (_params.surplusAmount == 0) revert AccEng_NullAmount();
    if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
    if (block.timestamp < lastSurplusTime + _params.surplusDelay) revert AccEng_SurplusCooldown();

    uint256 _coinBalance = safeEngine.coinBalance(address(this));
    uint256 _debtBalance = safeEngine.debtBalance(address(this));
    (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _unqueuedUnauctionedDebt(_debtBalance));

    if (_coinBalance < _debtBalance + _params.surplusAmount + _params.surplusBuffer) {
      revert AccEng_InsufficientSurplus();
    }

    // auction surplus percentage
-   if (_params.surplusTransferPercentage < ONE_HUNDRED_WAD) {
+   if (_params.surplusTransferPercentage < WAD) {
      _id = surplusAuctionHouse.startAuction({
-        _amountToSell: _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage),
+        _amountToSell: _params.surplusAmount.wmul(WAD - _params.surplusTransferPercentage),
        _initialBid: 0
      });

      lastSurplusTime = block.timestamp;
      emit AuctionSurplus(_id, 0, _params.surplusAmount.wmul(ONE_HUNDRED_WAD - _params.surplusTransferPercentage));
    }

    // transfer surplus percentage
    if (_params.surplusTransferPercentage > 0) {
      if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();

      safeEngine.transferInternalCoins({
        _source: address(this),
        _destination: extraSurplusReceiver,
        _rad: _params.surplusAmount.wmul(_params.surplusTransferPercentage)
      });

      lastSurplusTime = block.timestamp;
      emit TransferSurplus(extraSurplusReceiver, _params.surplusAmount.wmul(_params.surplusTransferPercentage));
    }
  }
```
</details>

**[RaymondFam (Lookout) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/26#issuecomment-1780191087):**
 > `_amountToSell` is indeed very close to 100 times (99% plus).

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/26#issuecomment-1783550386)**

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/26#issuecomment-1792490617):**
 > The warden has demonstrated how due to the incorrect use of `ONE_HUNDRED_WAD` instead of `WAD` when calculating percentages, surplus auctions will be created with massively inflated values, breaking the accounting of the protocol. As such, I agree with high severity.
> 
> Selected this report for best as it outlines the issues, impacts and recommended mitigation really well despite the lack of a coded PoC.

**[pi0neerpat (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/26#issuecomment-1861161709)**
 >Resolved [here](https://github.com/open-dollar/od-contracts/pull/254).

***

## [[H-02] Missing debt check lets users start a debt auction of non-existent debt](https://github.com/code-423n4/2023-10-opendollar-findings/issues/24)
*Submitted by [Saintcode\_](https://github.com/code-423n4/2023-10-opendollar-findings/issues/24), also found by [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/87) and [falconhoof](https://github.com/code-423n4/2023-10-opendollar-findings/issues/27)*

The `AccountingEngine.sol` contract serves as the protocol's central component responsible for initializing both debt and surplus auctions. Debt auctions are consistently initiated with a predefined minimum bid referred to as debtAuctionBidSize. This is done to ensure that the protocol can only auction debt that is not currently undergoing an auction and is not locked within the debt queue, as articulated in the comment found on `IAccountingEngine:248`: "It can only auction debt that has been popped from the debt queue and is not already being auctioned". This necessity prompts the check on AccountingEngine:181:

`if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance))`

This check verifies that there is a sufficient amount of bad debt available to auction.

The issue stems in line 183, where `_settleDebt` is called, this aims to ensure that only bad debt is considered for the auction. However, if the remaining bad debt, after settlement, falls below the specified threshold `(debtAuctionBidSize <= unqueuedUnauctionedDebt())`, the auction still starts with an incorrect amount of bad debt coverage, diluting the protocol Token when it is not yet needed.

### Impact

Non-existent debt gets auctioned when it is not necesary which dilutes the protocol token.

### PoC

The attached Proof of Concept (PoC) demonstrates two test cases:

*   In the initial test case, a two-call flow is implemented. The "settleDebt" function must be externally triggered before initiating an auction. This design ensures that a check for insufficient debt occurs prior to creating an auction. Consequently, as after calling settleDebt there is inadequate debt, the operation will revert.
*   In the second case, the `auctionDebt` function is invoked directly. It is expected to behave in the same manner as in the first test case (because the funciton calls `_settleDebt` internally). However, in this second test case, the execution follows a different path. Rather than replicating the behavior of the initial test case by reverting not starting the auction, the execution succeeds, resulting in the creation of a debt auction even when there is no existing debt.

The following tests have been created using the protocol's test suite:

First test case (original two call flow):

```solidity
  function test_missing_insuficient_debt_check_part2() public {
    accountingEngine.modifyParameters("surplusTransferPercentage", abi.encode(1));
    accountingEngine.modifyParameters("extraSurplusReceiver", abi.encode(1));

    safeEngine.createUnbackedDebt(address(0), address(accountingEngine), rad(100 ether));

    _popDebtFromQueue(100 ether);

    accountingEngine.settleDebt(100 ether);

    vm.expectRevert();
    uint id = accountingEngine.auctionDebt();

  }
```

Second test case (vulnerability):

```solidity
  function test_missing_insuficient_debt_check_part2() public {
    accountingEngine.modifyParameters("surplusTransferPercentage", abi.encode(1));
    accountingEngine.modifyParameters("extraSurplusReceiver", abi.encode(1));

    safeEngine.createUnbackedDebt(address(0), address(accountingEngine), rad(100 ether));

    _popDebtFromQueue(100 ether);

    
    uint id = accountingEngine.auctionDebt();

  }
```

Both test cases should revert and not let a user create a debt Auction under insufficient debt circumstances, but as stated on the report the second test case succeeds and creates the Auction.

### Recommended Mitigation Steps

To mitigate this risk, I suggest introducing the check `(_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance))` after calling \_settleDebt to ensure there exists enough amount of bad in the contract after the settling.

**[RaymondFam (Lookout) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/24#issuecomment-1780182988):**
 > Seems like `_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)` check is well in place prior to settling the debt in the second case, but will let the sponsor look into it.

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/24#issuecomment-1793315875):**
 > Due to the misplacement of the `_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)` check before `_settleDebt()` is called, the protocol will create debt auctions even when the amount of bad debt is below `params.debtAuctionBidSize`. This leads to dilution of the protocol token as bidders can buy non-existent debt, thereby destabilizing the value of protocol's token. As such, I agree with high severity.

**[pi0neerpat (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/24#issuecomment-1861163388)**
 >Resolved [here](https://github.com/open-dollar/od-contracts/pull/253).

***

 
# Medium Risk Findings (15)
## [[M-01] `ODSafeManager#allowSAFE()` cannot be executed either by the proxy contract or any other address.](https://github.com/code-423n4/2023-10-opendollar-findings/issues/429)
*Submitted by [0xAadi](https://github.com/code-423n4/2023-10-opendollar-findings/issues/429), also found by [Giorgio](https://github.com/code-423n4/2023-10-opendollar-findings/issues/447), [perseus](https://github.com/code-423n4/2023-10-opendollar-findings/issues/421), [Arz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/322), [ge6a](https://github.com/code-423n4/2023-10-opendollar-findings/issues/315), [0xprinc](https://github.com/code-423n4/2023-10-opendollar-findings/issues/313), [yashar](https://github.com/code-423n4/2023-10-opendollar-findings/issues/294), [Greed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/263), [0xlemon](https://github.com/code-423n4/2023-10-opendollar-findings/issues/241), [m4k2](https://github.com/code-423n4/2023-10-opendollar-findings/issues/231), [btk](https://github.com/code-423n4/2023-10-opendollar-findings/issues/205), [xAriextz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/191), [MrPotatoMagic](https://github.com/code-423n4/2023-10-opendollar-findings/issues/170), [nmirchev8](https://github.com/code-423n4/2023-10-opendollar-findings/issues/157), [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/76), and [0xDemon](https://github.com/code-423n4/2023-10-opendollar-findings/issues/248)*

"According to the GEB framework, the proxy contracts (`ODProxy`) are designed to interact with the Safe Manager (`ODSafeManager`) through the Proxy Action contract (`BasicActions`). The pivotal function, `allowSAFE()`, is responsible for granting an address the capability to manage the Safe. Unfortunately, the `BasicActions` contract lacks the implementation of the `allowSAFE()` function. Consequently, the proxy contract is unable to execute `allowSAFE()`, leading to the inaccessibility of several critical functions for authorized users.

The functionality of essential operations such as `modifySAFECollateralization()`, `transferCollateral()`, `transferInternalCoins()`, `quitSystem()`, `enterSystem()`, `moveSAFE()`, `removeSAFE()`, and `protectSAFE()` is currently halted due to the inability to allow users to manage the Safe.

In addition to the absent `allowSAFE()` implementation, the following functions are also not yet implemented in the `BasicActions` contract and these functions are not directly executable by proxy contract: `allowHandler()`, `modifySAFECollateralization()`, `transferCollateral()`, `transferInternalCoins()`, `quitSystem()`, `enterSystem()`, `moveSAFE()`, `removeSAFE()`, and `protectSAFE()`."

### Proof of Concept

Place the following test file in `test/nft/anvil/` and run `forge t --fork-url http://127.0.0.1:8545 --match-path test/nft/anvil/NFTTestAnvil.t.sol -vvv`

<details>

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.19;

import 'forge-std/console.sol';
import {AnvilFork} from '@test/nft/anvil/AnvilFork.t.sol';
import {Vault721} from '@contracts/proxies/Vault721.sol';
import {IODSafeManager} from '@interfaces/proxies/IODSafeManager.sol';
import {ODProxy} from '@contracts/proxies/ODProxy.sol';

contract NFTTestAnvil is AnvilFork {

  function test_setup() public {
    assertEq(totalVaults, vault721.totalSupply());
    checkProxyAddress();
    checkVaultIds();
  }

// Function to test open safe and allow safe with direct call
  function test_allowSafeDirectCall() public {
    address _proxy = proxies[0];
    bytes32 cType = cTypes[0];
    address user = users[0];

    vm.startPrank(user);  

    uint256 safeId = safeManager.openSAFE(cType,_proxy);
    IODSafeManager.SAFEData memory sData = safeManager.safeData(safeId);
    assertEq(_proxy,sData.owner);

    // allowSAFE call will fail
    vm.expectRevert(IODSafeManager.SafeNotAllowed.selector);
    safeManager.allowSAFE(safeId, user, 1);

    vm.stopPrank();
  }

  // Function to test open safe and allow safe with proxy delegation call
  function test_allowSafeWithProxyExecute() public {
    address _proxy = proxies[0];
    bytes32 cType = cTypes[0];
    address user = users[0];

    vm.startPrank(user);  

    uint256 safeId = openSafe2(cType,_proxy);
    IODSafeManager.SAFEData memory sData = safeManager.safeData(safeId);
    assertEq(_proxy,sData.owner);

    // allowSAFE call will fail
    vm.expectRevert();
    allowSafe(_proxy, safeId, user, 1);

    vm.stopPrank();
  }

  function openSafe2(bytes32 _cType, address _proxy) public returns (uint256 _safeId) {
    bytes memory payload = abi.encodeWithSelector(basicActions.openSAFE.selector, address(safeManager), _cType, _proxy);
    bytes memory safeData = ODProxy(_proxy).execute(address(basicActions), payload);
    _safeId = abi.decode(safeData, (uint256));
  }

  function allowSafe(address _proxy, uint256 _safe, address _usr, uint256 _ok) public  {
    bytes memory payload = abi.encodeWithSelector(safeManager.allowSAFE.selector,_safe, _usr, _ok);
    ODProxy(_proxy).execute(address(safeManager), payload);
  }  
}
```

</details>

Here you can see, both call to the `allowSAFE()` are failing, ie proxy contract cannot able to allow an address to manage the safe.

See the second test case, when we are calling the `allowSAFE()` with `delegatecall` the context of the target(`ODSafeManager`) has changed and the call get failed.

### Tools Used

Foundry

### Recommended Mitigation Steps

Implement necessary functions in the `BasicActions` contract to execute necessary functions in the `ODSafeManager` contract.

**[MiloTruck (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/429#issuecomment-1800716203):**
 > Since `BasicActions.sol` does not have a function to call `allowSafe()`, users will not be able to interact with `ODSafeManager` through their proxies.
> 
> However, since this can be addressed operationally by deploying another implementation contract, there isn't any permanent DOS of the protocol's functionality. Therefore, medium severity is appropriate.

***

## [[M-02] Old permissions in handlerCan mapping are still attached to the safeHandler of a transferred safe](https://github.com/code-423n4/2023-10-opendollar-findings/issues/382)
*Submitted by [MrPotatoMagic](https://github.com/code-423n4/2023-10-opendollar-findings/issues/382)*

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136> 

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L41>

A safe is associated with a unique safeHandler. This safeHandler can give permissions to addresses through the [allowHandler()](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L112) function which stores these approvals/disapprovals in the [handlerCan](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L41) mapping. Now let's say there is a safeHandler which has permissioned some addresses in the handlerCan mapping. When this safe is transferred to a new owner, the previous permissions that were added to the safeHandler are still attached to it without the new owner realizing it.

### Proof of Concept

Here is the whole process:

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L112C1-L115C4>

1.  Safe handler of a safe (owned by owner A) approves addresses \[X,Y,Z] to the [handlerCan](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L41) mapping through the [allowHandler() function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L112C1-L115C4).

```solidity
File: ODSafeManager.sol
112:   function allowHandler(address _usr, uint256 _ok) external {
113:     handlerCan[msg.sender][_usr] = _ok;
114:     emit AllowHandler(msg.sender, _usr, _ok);
115:   }
```

2.  Owner A now transfers the safe (associated with the safe handler) to an Owner B through the [safeTransferFrom() function](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/7c8b7a27284f503ce8ae23d63ac9403096dcf6fe/contracts/token/ERC721/ERC721.sol#L152) in the Vault721.sol contract which [inherits the ERC721Enumerable contract](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L13) (that inherits the ERC721 contract). During the call, the [\_afterTokenTransfer()](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L187) hook is called ([overridden in the Vault721.sol contract](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L187)), which further calls the [transferSAFEOwnership() function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136C1-L152C4). In the function, we can see that the safe is transferred but the [handlerCan](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L41) mapping is not updated for the safeHandler, which means the old permissions (addresses \[X,Y,Z]) for the safeHandler are still attached without the new owner B realizing it.

```solidity
File: ODSafeManager.sol
136:   function transferSAFEOwnership(uint256 _safe, address _dst) external {
137:     require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');
138: 
139: 
140:     if (_dst == address(0)) revert ZeroAddress();
141:     SAFEData memory _sData = _safeData[_safe];
142:     if (_dst == _sData.owner) revert AlreadySafeOwner();
143: 
144: 
145:     _usrSafes[_sData.owner].remove(_safe);
146:     _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
147: 
148: 
149:     _usrSafes[_dst].add(_safe);
150:     _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);
151: 
152:
153:     _safeData[_safe].owner = _dst;
154: 
155: 
156:     emit TransferSAFEOwnership(msg.sender, _safe, _dst);
157:   }
```

### Recommended Mitigation Steps

It is not possible to remove the [handlerCan](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L41) permissions in the [transferSAFEOwnership() function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136C1-L152C4) since it is stored in a mapping. The only solution to this would be to add another key field (named \_owner) to update the safeHandler permissions correctly whenever a safe is transferred. We can see this pattern implemented for the [safeCan mapping](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L39), which correctly updates the safe permissions on transfer.

Solution (use this mapping instead):

```solidity
File: ODSafeManager.sol
41:   mapping(address _owner => (address _safeHandler => mapping(address _caller => uint256 _ok))) public handlerCan;
```

**[MiloTruck (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/382#issuecomment-1791395890):**
 > The warden has demonstrated how handler permissions in `handlerCan` are not cleared on transfer, which allows the previous owner of the safe to bypass `handlerAllowed()` checks for a safe handler he previously owned.
> 
> There is probably an impact worthy of high severity due to this bug (eg. a malicious owner can call `quitSystem()` with his own safe and new owner's safe handler to transfer his debt to the new owner). However, since this report does not have any elaboration on how this bug can be exploited to cause harm to the protocol/users, I don't think it is deserving of high severity.

***

## [[M-03] Updating `SafeManager` address in the `Vault721` will disable NFV minting](https://github.com/code-423n4/2023-10-opendollar-findings/issues/381)
*Submitted by [hals](https://github.com/code-423n4/2023-10-opendollar-findings/issues/381), also found by [perseus](https://github.com/code-423n4/2023-10-opendollar-findings/issues/433), [yashar](https://github.com/code-423n4/2023-10-opendollar-findings/issues/326), [0xprinc](https://github.com/code-423n4/2023-10-opendollar-findings/issues/233), and [nmirchev8](https://github.com/code-423n4/2023-10-opendollar-findings/issues/88)*

*   `Vault721` contract is the ERC721 "Non-fungible Vault" (NFV) that manages all SAFEs ownership, transfers, and approvals via the ERC721 standard, the minter of the NFV is the `ODSafeManager` contract.

*   `ODSafeManager` contract acts as interface to the SAFEEngine to facilitate the management of SAFEs; such as opening safes,modifying SAFE collateral, moving SAFE collateral, etc.

*   When a user opens a SAFE via `ODSafeManager.openSAFE` function; a NFV will be minted to the user where it will have the same id as the SAFE id so that the ids of any of them can be used interchangeably in the system, and the `_safeId` is the state variable that tracks the number of the opened safes/and minted NFV by the `ODSafeManager` contract.

*   The initial value of the `_safeId` is zero; and it will be incremented with each opened SAFE.

*   But a problem will be caused if the governance change the `ODSafeManager` address via `Vault721.setSafeManager`; as this will result in adding a new deployed safe manager where it will have the `_safeId` set to zero, which will result in breaking the synchronization between the `_safeId` and the last minted NFV id (breaking an invariant).

*   But how could this be a problem?
    Well, this will result in **disabling SAFE opening and NFV minting:**

    1.  The old safe manager contract has a `_safeId` of 1000, and this matches the id of the last minted NFV, so the next NFV id supposed to be 1001, and that should match the value of `_safeId`.
    2.  The governance decided to change the safe manager address (with a new deployed one) with extra/updated features; so the `_safeId` of the new `ODSafeManager` will be zero.
    3.  Now a user tries to open a SAFE; so the `_safeId` will be incremeted by 1 (and will equal one) and when the `Vault721.mint` is called to mint a NFV; the function will revert as it's requested to mint a NFV with id=`_safeId`=1, and this NFV has been already deployed by the old safe manager contract.

### Proof of Concept

[Vault721.setSafeManager function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L126-L128)

```solidity
  function setSafeManager(address _safeManager) external onlyGovernor {
    _setSafeManager(_safeManager);
  }
```

[Vault721.\_setSafeManager function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L172-L174)

```solidity
  function _setSafeManager(address _safeManager) internal nonZero(_safeManager) {
    safeManager = IODSafeManager(_safeManager);
  }
```

[ODSafeManager.openSAFE function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L118-L133)

```solidity
  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {
   //some code...

    ++_safeId;

   //some code...

    vault721.mint(_usr, _safeId);

    //some code...
  }
```

[Vault721.mint function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L94-L99)

```solidity
function mint(address _proxy, uint256 _safeId) external {
    require(msg.sender == address(safeManager), 'V721: only safeManager');
    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
    address _user = _proxyRegistry[_proxy];
    _safeMint(_user, _safeId);
  }
```

### Recommended Mitigation Steps

Add a mechanism in the `ODSafeManger` to initialize the `_safeId` based on the total number of previously minted NFV + 1 (another variable to be added to the `Vault721` to track the total number of minted NFVs).

**[MiloTruck (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/381#issuecomment-1800655999):**
 > Since the current implementation of `ODSafeManager` doesn't have the ability to migrate information to a newer version, it will break the `Vault721` contract if the newer version is not modified.
> 
> Note that the issue of a new `ODSafeManager` not having the state of the previous contract can be addressed in the new contract itself (eg. add functions in the new `ODSafeManager` implementation to set `_safeId`, `_safeData`, etc... to the same values as the current one).
> 
> However, given that the sponsor was not aware of the bug beforehand (see #326), I believe that this report and its duplicates has provided value to the sponsor and therefore medium severity is appropriate. 

***

## [[M-04] Mismatch between the SAFE generated debt and the amount of the system tokens minted for the user](https://github.com/code-423n4/2023-10-opendollar-findings/issues/371)
*Submitted by [hals](https://github.com/code-423n4/2023-10-opendollar-findings/issues/371)*

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L94-L115> 

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L31-L47>

*   When the user generates a debt against his collateral (when he locks a collateral first) in `BasicActions.generateDebt`:

    *   First the user requests a debt of a value `_deltaWad`.

    *   Then this requested value is modified by the `_getGeneratedDeltaDebt` function: where the requested debt by the user is updated by comparing the coin balance of the SAFE (which is minted when the user generates a debt) with the requested debt; if the requested debt exceeds the coin balance of the SAFE; the requested debt value is modified by calculating the needed `deltaDebt` that would be enough to exit wad amount of COIN tokens with the current coins balance of the SAFE.

    *   Then a debt is going to be generated for the SAFE with this modified debt value (by updating the SAFE's collateralization by the `SAFEEngine`):

        ```solidity
        _modifySAFECollateralization(
              _manager,
              _safeId,
              0,
              _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
            );
        ```

    *   Then the user (EOA) will be minted system tokens of a value equals to the original value requested by him not the updated value generated by the `_getGeneratedDeltaDebt` function, and this is done via `_exitSystemCoins` function which will in turn call the `__coinJoin.exit` function:

        ```solidity
        _exitSystemCoins(_coinJoin, _deltaWad * RAY);
        ```

*   So as can be seen; the values doesn't match; as the modified `_deltaDebt` that is used to update the `safe.generatedDebt` (via `BasicActions._modifySAFECollateralization` function) might be of a value lesser/larger (depending on the rate and coins balance of the safe) than the actual minted tokens; so when the user wants to repay the SAFE debt his minted tokens might not be enough to do so.

### Proof of Concept

[BasicActions.\_generateDebt function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L94-L115)

```solidity
function _generateDebt(
    address _manager,
    address _taxCollector,
    address _coinJoin,
    uint256 _safeId,
    uint256 _deltaWad
  ) internal {
    address _safeEngine = ODSafeManager(_manager).safeEngine();
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

    // Generates debt in the SAFE
    _modifySAFECollateralization(
      _manager,
      _safeId,
      0,
      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad) //  @audit : will not equal the input _deltaWad
    );

    // Moves the COIN amount to user's address
    _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);// @audit : _deltaWad is different from the one used to generate dabt in safeEngine
  }
```

[BasicActions.\_getGeneratedDeltaDebt function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L31-L47)

```solidity
  function _getGeneratedDeltaDebt(
    address _safeEngine,
    bytes32 _cType,
    address _safeHandler,
    uint256 _deltaWad
  ) internal view returns (int256 _deltaDebt) {
    uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
    uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);

    // If there was already enough COIN in the safeEngine balance, just exits it without adding more debt
    if (_coinAmount < _deltaWad * RAY) {
      // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
      _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
      // This is neeeded due lack of precision. It might need to sum an extra deltaDebt wei (for the given COIN wad amount)
      _deltaDebt = uint256(_deltaDebt) * _rate < _deltaWad * RAY ? _deltaDebt + 1 : _deltaDebt;
    }
  }
```
### Recommended Mitigation Steps

Update `_generateDebt` function to use the same modified `_deltaWad` value for generating debt and minting system tokens:

```diff
function _generateDebt(
    address _manager,
    address _taxCollector,
    address _coinJoin,
    uint256 _safeId,
    uint256 _deltaWad
  ) internal {
    address _safeEngine = ODSafeManager(_manager).safeEngine();
    ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
    ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

+   int256 _UpdatedDeltaDebt=_getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad);

    // Generates debt in the SAFE
    _modifySAFECollateralization(
      _manager,
      _safeId,
      0,
-     _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
+     _UpdatedDeltaDebt
    );

    // Moves the COIN amount to user's address
-   _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);
+   _collectAndExitCoins(_manager, _coinJoin, _safeId, _UpdatedDeltaDebt);
  }
```

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/371#issuecomment-1787839429)**

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/371#issuecomment-1792581760):**
 > In `_generateDebt()`, `_collectAndExitCoins()` is called to transfer system coins equivalent to the requested amount of collateral instead of the actual amount used for debt. This will cause the function to revert should `_deltaWad` be higher than the amount of collateral the user has in his safe to generate debt. 
> 
> Since this is a case of the function not working as intended, I believe medium severity is appropriate.

***

## [[M-05] Decimal Limitation in CamelotRelayer and UniV3Relayer Contract Deployment](https://github.com/code-423n4/2023-10-opendollar-findings/issues/323)
*Submitted by [0xmystery](https://github.com/code-423n4/2023-10-opendollar-findings/issues/323), also found by [phoenixV110](https://github.com/code-423n4/2023-10-opendollar-findings/issues/446), [hals](https://github.com/code-423n4/2023-10-opendollar-findings/issues/439), [ni8mare](https://github.com/code-423n4/2023-10-opendollar-findings/issues/437), [Tendency](https://github.com/code-423n4/2023-10-opendollar-findings/issues/407), [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/327), [spark](https://github.com/code-423n4/2023-10-opendollar-findings/issues/298), [0x6d6164616e](https://github.com/code-423n4/2023-10-opendollar-findings/issues/257), [niki](https://github.com/code-423n4/2023-10-opendollar-findings/issues/238), [twcctop](https://github.com/code-423n4/2023-10-opendollar-findings/issues/215), [lsaudit](https://github.com/code-423n4/2023-10-opendollar-findings/issues/147), [0xsurena](https://github.com/code-423n4/2023-10-opendollar-findings/issues/58), [ZanyBonzy](https://github.com/code-423n4/2023-10-opendollar-findings/issues/43), [cryptothemex](https://github.com/code-423n4/2023-10-opendollar-findings/issues/34), and [0xWaitress](https://github.com/code-423n4/2023-10-opendollar-findings/issues/18)*

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L58> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L103-L105> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L64> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L110-L112>

The current design of the CamelotRelayer and UniV3Relayer contracts limits its compatibility to only those `_quoteTokens` that have a decimal count of 18 or fewer. If an attempt is made to deploy the contract with a token having more than 18 decimals as the `_quoteToken`, the contract deployment will fail due to an underflow issue during the multiplier calculation. This poses no financial risk but restricts the contract's adaptability in the wider DeFi ecosystem, preventing its use with tokens that have more than 18 decimals.

### Proof of Concept

The restriction emerges from the constructor, where the `multiplier` is deduced as `18 - IERC20Metadata(_quoteToken).decimals()`.

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L58> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/UniV3Relayer.sol#L64>

```solidity
    multiplier = 18 - IERC20Metadata(_quoteToken).decimals();
```

For tokens like [`YAMv2`, which possess 24 decimals](https://etherscan.io/token/0xaba8cac6866b83ae4eec97dd07ed254282f6ad8a), the computation would attempt `18 - 24`, which results in an underflow, making the contract deployment unsuccessful.

### Recommended Mitigation Steps

1.  Alter the datatype of `multiplier` to `int256` to account for both positive and negative values.

2.  Adjust the multiplier's computation in the constructor to handle situations where token decimals might be greater or less than 18.

```solidity
    int8 decimalsDifference = 18 - int8(IERC20Metadata(_quoteToken).decimals());
    multiplier = int256(decimalsDifference);
```

3.  Revise the `_parseResult` function to either multiply or divide the `_quoteResult` depending on the `multiplier` value.

```solidity
    function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {
        if (multiplier > 0) {
            return _quoteResult * (10 ** uint256(multiplier));
        } else if (multiplier < 0) {
            return _quoteResult / (10 ** uint256(-multiplier));
        } else {
            return _quoteResult;
        }
    }
```

Note: It will require additional code refactoring to make `baseAmount` and its value assignment as `int256` as well.

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/323#issuecomment-1787830556)**

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/323#issuecomment-1790123541):**
 > The warden has demonstrated how `CamelotRelayer.sol` and `UniV3Relayer.sol` cannot be deployed for tokens with more than 18 decimals, which limits the functionality of the protocol unnecessarily. As such, medium severity is appropriate. 


***

## [[M-06] SafeHandler contract doesn't have any method to call to `ODSafeManager.allowHandler()`, lead to DOS in some function](https://github.com/code-423n4/2023-10-opendollar-findings/issues/297)
*Submitted by [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/297), also found by [0xprinc](https://github.com/code-423n4/2023-10-opendollar-findings/issues/414), [MrPotatoMagic](https://github.com/code-423n4/2023-10-opendollar-findings/issues/314), [Baki](https://github.com/code-423n4/2023-10-opendollar-findings/issues/307), [Tendency](https://github.com/code-423n4/2023-10-opendollar-findings/issues/303), and [COSMIC-BEE-REACH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/249)*

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/SAFEHandler.sol#L1-L19> 

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L112-L115> 

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L59-L62>

SafeHandler contract doesn't have any method to call to `ODSafeManager.allowHandler()`. Because of that, functions in ODSafeManager contract which use `handlerAllowed` modifier will never be called successfully. There're two function `quitSystem()` and `enterSystem()` that use `handlerAllowed` modifier.

### Proof of Concept

This is the whole SAFEHandler contract, it doesn't have any method that call to `ODSafeManager.allowHandler()`

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.19;

import {ISAFEEngine} from '@interfaces/ISAFEEngine.sol';

contract SAFEHandler {
  constructor(address _safeEngine) {
    ISAFEEngine(_safeEngine).approveSAFEModification(msg.sender);
  }
}
```

Hence, it can't call to `allowHandler()`, which give users permission to execute action within the safe

```solidity
  function allowHandler(address _usr, uint256 _ok) external {
    handlerCan[msg.sender][_usr] = _ok;
    emit AllowHandler(msg.sender, _usr, _ok);
  }
```

Because of that, no one can surpass the `handlerAllowed` modifier with valid handler address:

```solidity
  modifier handlerAllowed(address _handler) {
    if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
    _;
  }
```

`enterSystem()` and `quitSystem()` use `handlerAllowed` modifier:

```solidity
function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe)
function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst)
```

### Recommended Mitigation Steps

I can think of two way to achieve this:

1.  Add a function in SafeHandler contract that can call to `ODSafeManager.allowHandler()`
2.  Add an immutable variable `SAFE_ID` in SafeHandler contract and change this to `ODSafeManager.allowHandler()`:

```diff
-  function allowHandler(uint256 _safeId, address _usr, uint256 _ok) external {
+  function allowHandler(address _usr, uint256 _ok) external {

+   SAFEData memory _sData = _safeData[_safeId];
+   require(_sData.owner == msg.sender);

-   handlerCan[msg.sender][_usr] = _ok;
+   handlerCan[_sData.safeHandler][_usr] = _ok;

-   emit AllowHandler(msg.sender, _usr, _ok);
  }
```

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/297#issuecomment-1791358001):**
 > The warden has demonstrated how `allowHandler()` can never be called with `msg.sender` as a user's safe handler, causing `enterSystem()` and `quitSystem()` to become uncallable. As this affects the functionality of the protocol but does not cause any loss of assets or affect the protocol's core functionality, I believe medium severity is appropriate.

***

## [[M-07] Malicious users are able to bypass the Tax payment using making a Fake BasicActions Contract](https://github.com/code-423n4/2023-10-opendollar-findings/issues/286)
*Submitted by [yashar](https://github.com/code-423n4/2023-10-opendollar-findings/issues/286)*

Users can interact with the protocol without paying any taxes.

### Proof of Concept

Whenever a user wants to interact with the protocol (E.g: lock collateral, generate debt, repay debt, etc) he/she should make a call to their proxy contract and the proxy contract will make a delegatecall to BasicActions contract. BasicActions contract will calculate and pay the tax on every:

*   \_generateDebt: [Link to code](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L103)
*   \_repayDebt: [Link to code](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L130)
*   \_lockTokenCollateralAndGenerateDebt: [Link to code](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L181)
*   repayAllDebt: [Link to code](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L290)
*   repayDebtAndFreeTokenCollateral: [Link to code](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L356)
*   repayAllDebtAndFreeTokenCollateral: [Link to code](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L384)

Given that this is a delegatecall to BasicActions contract and it can be any other contract, a malicious user will be able to deploy a Fake BasicActions contract that doesn't contain any Tax payment mechanism and simply bypass the Tax payment.

To test the scenario please make a file named `FakeBasicActions.sol` in this path: `test/nft/anvil` and copy/paste the following contract code that has no Tax payment mechanism in it:

<details>

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity 0.8.19;

    import {ODSafeManager} from '@contracts/proxies/ODSafeManager.sol';
    import {ODProxy} from '@contracts/proxies/ODProxy.sol';

    import {ISAFEEngine} from '@interfaces/ISAFEEngine.sol';
    import {ICoinJoin} from '@interfaces/utils/ICoinJoin.sol';
    import {ITaxCollector} from '@interfaces/ITaxCollector.sol';
    import {ICollateralJoin} from '@interfaces/utils/ICollateralJoin.sol';
    import {IERC20Metadata} from '@openzeppelin/token/ERC20/extensions/IERC20Metadata.sol';
    import {IBasicActions} from '@interfaces/proxies/actions/IBasicActions.sol';

    import {Math, WAD, RAY, RAD} from '@libraries/Math.sol';

    import {CommonActions} from '@contracts/proxies/actions/CommonActions.sol';

    contract FakeBasicActions {
       using Math for uint256;

      function lockTokenCollateralAndGenerateDebt(
        address _manager,
        address _taxCollector,
        address _collateralJoin,
        address _coinJoin,
        uint256 _safeId,
        uint256 _collateralAmount,
        uint256 _deltaWad
      ) public {
        address _safeEngine = ODSafeManager(_manager).safeEngine();
        ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);

        // Takes token amount from user's wallet and joins into the safeEngine
        _joinCollateral(_collateralJoin, _safeInfo.safeHandler, _collateralAmount);

        // Locks token amount into the SAFE and generates debt
        _modifySAFECollateralization(
          _manager,
          _safeId,
          _collateralAmount.toInt(),
          _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
        );

        // Exits and transfers COIN amount to the user's address
        _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);
      }

      function generateDebt(
        address _manager,
        address _taxCollector,
        address _coinJoin,
        uint256 _safeId,
        uint256 _deltaWad
      ) public {
        address _safeEngine = ODSafeManager(_manager).safeEngine();
        ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);

        // Generates debt in the SAFE
        _modifySAFECollateralization(
          _manager,
          _safeId,
          0,
          _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
        );

        // Moves the COIN amount to user's address
        _collectAndExitCoins(_manager, _coinJoin, _safeId, _deltaWad);
      }

      function _modifySAFECollateralization(
        address _manager,
        uint256 _safeId,
        int256 _deltaCollateral,
        int256 _deltaDebt
      ) internal {
        ODSafeManager(_manager).modifySAFECollateralization(_safeId, _deltaCollateral, _deltaDebt);
      }

      function _getGeneratedDeltaDebt(
        address _safeEngine,
        bytes32 _cType,
        address _safeHandler,
        uint256 _deltaWad
      ) internal view returns (int256 _deltaDebt) {
        uint256 _rate = ISAFEEngine(_safeEngine).cData(_cType).accumulatedRate;
        uint256 _coinAmount = ISAFEEngine(_safeEngine).coinBalance(_safeHandler);

        // If there was already enough COIN in the safeEngine balance, just exits it without adding more debt
        if (_coinAmount < _deltaWad * RAY) {
          // Calculates the needed deltaDebt so together with the existing coins in the safeEngine is enough to exit wad amount of COIN tokens
          _deltaDebt = ((_deltaWad * RAY - _coinAmount) / _rate).toInt();
          // This is neeeded due lack of precision. It might need to sum an extra deltaDebt wei (for the given COIN wad amount)
          _deltaDebt = uint256(_deltaDebt) * _rate < _deltaWad * RAY ? _deltaDebt + 1 : _deltaDebt;
        }
      }

      function _collectAndExitCoins(address _manager, address _coinJoin, uint256 _safeId, uint256 _deltaWad) internal {
        // Moves the COIN amount to proxy's address
        _transferInternalCoins(_manager, _safeId, address(this), _deltaWad * RAY);
        // Exits the COIN amount to the user's address
        _exitSystemCoins(_coinJoin, _deltaWad * RAY);
      }

      function _transferInternalCoins(address _manager, uint256 _safeId, address _dst, uint256 _rad) internal {
        ODSafeManager(_manager).transferInternalCoins(_safeId, _dst, _rad);
      }

      function _exitSystemCoins(address _coinJoin, uint256 _coinsToExit) internal virtual {
        if (_coinsToExit == 0) return;

        ICoinJoin __coinJoin = ICoinJoin(_coinJoin);
        ISAFEEngine __safeEngine = __coinJoin.safeEngine();

        if (!__safeEngine.canModifySAFE(address(this), _coinJoin)) {
          __safeEngine.approveSAFEModification(_coinJoin);
        }

        // transfer all coins to msg.sender (proxy shouldn't hold any system coins)
        __coinJoin.exit(msg.sender, _coinsToExit / RAY);
      }

      function _joinCollateral(address _collateralJoin, address _safe, uint256 _wad) internal {
        ICollateralJoin __collateralJoin = ICollateralJoin(_collateralJoin);
        IERC20Metadata _token = __collateralJoin.collateral();

        // Transforms the token amount into ERC20 native decimals
        uint256 _decimals = _token.decimals();
        uint256 _wei = _wad / 10 ** (18 - _decimals);
        if (_wei == 0) return;

        // Gets token from the user's wallet
        _token.transferFrom(msg.sender, address(this), _wei);
        // Approves adapter to take the token amount
        _token.approve(_collateralJoin, _wei);
        // Joins token collateral into the safeEngine
        __collateralJoin.join(_safe, _wei);
      }
      
    }

</details>

And also make another file called `NinjaTests.t.sol` in this path: `test/nft/anvil` and copy/paste the following test code in it:

<details>

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity 0.8.19;
    import 'forge-std/Test.sol';
    import {AnvilFork} from '@test/nft/anvil/AnvilFork.t.sol';
    import {WSTETH, ARB, CBETH, RETH, MAGIC} from '@script/GoerliParams.s.sol';
    import {IERC20} from '@openzeppelin/token/ERC20/IERC20.sol';
    import {SafeERC20} from '@openzeppelin/token/ERC20/utils/SafeERC20.sol';
    import {Vault721} from '@contracts/proxies/Vault721.sol';
    import {IODSafeManager} from '@interfaces/proxies/IODSafeManager.sol';
    import {ISAFEEngine} from '@interfaces/ISAFEEngine.sol';
    import {ODSafeManager} from '@contracts/proxies/ODSafeManager.sol';
    import {ODProxy} from '@contracts/proxies/ODProxy.sol';
    import {FakeBasicActions} from '@test/nft/anvil/FakeBasicActions.sol';

    contract NinjaTests is AnvilFork {
      using SafeERC20 for IERC20;

      FakeBasicActions fakeBasicActions;

      function test_setup() public {
        assertEq(totalVaults, vault721.totalSupply());
        checkProxyAddress();
        checkVaultIds();
      }

      function test_GenerateDebtWithoutTax() public {
        fakeBasicActions = new FakeBasicActions();
        address proxy = proxies[1];
        bytes32 cType = cTypes[1];
        uint256 vaultId = vaultIds[proxy][cType];

        bytes memory payload = abi.encodeWithSelector(
          fakeBasicActions.lockTokenCollateralAndGenerateDebt.selector,
          address(safeManager),
          address(taxCollector),
          address(collateralJoin[cType]),
          address(coinJoin),
          vaultId,
          1,
          0
        );
        vm.startPrank(users[1]);

        // Proxy makes a delegatecall to Malicious BasicAction contract and bypasses the TAX payment
        ODProxy(proxy).execute(address(fakeBasicActions), payload);
        genDebt(vaultId, 10, proxy);
        
        vm.stopPrank();

        IODSafeManager.SAFEData memory sData = safeManager.safeData(vaultId);
        address safeHandler = sData.safeHandler;
        ISAFEEngine.SAFE memory SafeEngineData = safeEngine.safes(cType, safeHandler);
        assertEq(1, SafeEngineData.lockedCollateral);
        assertEq(10, SafeEngineData.generatedDebt);
      }
    }

</details>

Test commands: `forge test --fork-url $ANVIL_RPC --match-test test_GenerateDebtWithoutTax`

### Tools Used

VSCode, Foundry

### Recommended Mitigation Steps

Move the Tax payment mechanism into ODSafeManager contract. E.g:

```diff
  function modifySAFECollateralization(
    uint256 _safe,
    int256 _deltaCollateral,
    int256 _deltaDebt
  ) external safeAllowed(_safe) {
+   ODSafeManager.SAFEData memory _safeInfo = _safeData[_safe];
+   ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);
    SAFEData memory _sData = _safeData[_safe];
    ISAFEEngine(safeEngine).modifySAFECollateralization(
      _sData.collateralType, _sData.safeHandler, _sData.safeHandler, _sData.safeHandler, _deltaCollateral, _deltaDebt
    );
    emit ModifySAFECollateralization(msg.sender, _safe, _deltaCollateral, _deltaDebt);
  }
```

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/286#issuecomment-1783581168)**

**[MiloTruck (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/286#issuecomment-1790469484):**
 > The warden has demonstrated how users can bypass tax collection by interacting with `ODSafeManager` through their own implementation contracts. Since this results in a loss of yield (protocol fees) and not assets (user funds), I believe medium severity is appropriate.

***

## [[M-08] Collateral could be transferred to an address, which is not `SAFEHandler` managed by the `SAFEManager`](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267)
*Submitted by [nmirchev8](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267)*

In the current implementation each safe has corresponding proxy contract inside `ODSafeManager` and corresponding `SAFEHandler` "proxy" which is deployed for each safe and used as safe's address inside `SAFEEngine`.
The problem is that inside `ODSafeManager` `transferCollateral()` function there is no check wether the provided address is a valid safe and so the collateral could be "transferred" to any ordinary address.

### Impact

This could result in contracts missbehaviour, or lost of funds as it breaks the invariant of "Each safe has a corresponding SAFEHandler" and so other accounting errors could appear as the new owner of the contract can transfer it where he wants directly calling `SafeEngine.sol` contract, which is also unwanted behaviour (To execute safe operations, bypassing `SafeManager.sol`)

### Proof of Concept

1.  Alice has a safe with X amount of collateral and want to transfer it to Bob and passes his address.
2.  The transaction passes and now inside SAFEEngine Bob's amount of collateral has increased, but if he want to do something with safe/s he has to have a proxy and a safe, for which he would be owner. So those "funds" which are for accounting don't have corresponding vault and safe. They are assigned to an EOA

### Recommended Mitigation Steps

The issue is confirmed by sponsor, who suggested adding in a `safeHandler exists` check. 

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1787699833)**

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1791245351):**
 > The warden has demonstrated how due to a lack of validation on the `_dst` parameter in `transferCollateral()` and `transferInternalCoins()`, the internal accounting of collateral and coins in the `SAFEEngine` contract can accrue to any arbitrary EOA, such as an attacker's own address. This can be exploited to call functions in the `SAFEEngine` contract directly, which should not be possible.
> 
> Since this report does not demonstrate how this could lead to a potential loss of funds for the protocol/users, I believe medium severity is appropriate.

**[hals (Warden) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1806403276):**
 > Hi, I think this is intended by design to transfer SAFE collateral to any address that doesn't represent a SAFE (`safeHandler`).

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1806741319):**
 > @pi0neerpat, could you confirm if users should be able to call `transferCollateral()` and `transferInternalCoins()` in `ODSafeManager` to transfer their internal coin/collateral balance in `SAFEEngine` to any arbitrary address?
> 
> The internal coins/collateral will not be stuck in the protocol as users can call `exit()` in `CollateralJoin` or `CoinJoin` to mint system coins or transfer out collateral.
> 
> However, allowing internal balances to accrue to any arbitrary address means that users can call functions in the `SAFEEngine` contract directly, instead of through `ODSafeManager`. This was my original reason for medium severity, so it would be great if you could clarify if this should not be allowed.

**[nmirchev8 (Warden) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1806749683):**
 > In such way users can call functions on SAFEEngine contract directly, which breaks the invariants provided by the team.

**[daopunk (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1809389017):**
 > I agree that user calls directly to the SafeEngine should not be allowed, thus a check that the destination address is an existing safeHandler should be made.

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/267#issuecomment-1809508810):**
 > Will maintain medium severity since this is unintended behavior, as mentioned by the sponsor, and seems to violate the following invariant from the [Audit's README](https://github.com/code-423n4/2023-10-opendollar#main-invariants):
> 
> > Users must exclusively use the ODProxy to interact with their safes.

***

## [[M-09] Vault721.tokenURI does not comply with ERC721 - Metadata specification](https://github.com/code-423n4/2023-10-opendollar-findings/issues/243)
*Submitted by [Haipls](https://github.com/code-423n4/2023-10-opendollar-findings/issues/243), also found by [kutugu](https://github.com/code-423n4/2023-10-opendollar-findings/issues/445) and [twcctop](https://github.com/code-423n4/2023-10-opendollar-findings/issues/210)*

According to the standard, the `tokenURI` method must be reverted if a non-existent tokenId is passed. In the Vault721 contract, this point was ignored. What leads to a [violation of the EIP721 spec](https://eips.ethereum.org/EIPS/eip-721#:\~:text=function%20tokenURI\(uint256%20\_tokenId\)%20external%20view%20returns%20\(string\)%3B).

### Proof of Concept

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/proxies/Vault721.sol#L140-L142>

```js
File: contracts/proxies/Vault721.sol

  /**
   * @dev generate URI with updated vault information
   */
  function tokenURI(uint256 _safeId) public view override returns (string memory uri) {
    // @audit should throw if safeId does not exist according to the ERC721-Metadata specification
    uri = nftRenderer.render(_safeId);
  }
```

The responsibility for checking whether a token exists may be put on the nftRenderer depending on the implementation, and may also be missing, changed or added in the future. But the underlying Vault721 contract that is supposed to comply with the standard does not follow the specification.

Similar problem with violation of ERC721 specification: <https://github.com/code-423n4/2023-04-caviar-findings/issues/44>

### Tools Used

<https://eips.ethereum.org/EIPS/eip-721#specification>

### Recommended Mitigation Steps

Add a token existence check at the Vault721 level, example:

```js
  /**
   * @dev generate URI with updated vault information
   */
  function tokenURI(uint256 _safeId) public view override returns (string memory uri) {
+++ _requireMinted(_safeId);
    uri = nftRenderer.render(_safeId);
  }

```

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/243#issuecomment-1789731499):**
 > Although the impact is extremely limited, the function does violate the ERC-721 spec as shown [here](https://eips.ethereum.org/EIPS/eip-721#:~:text=function%20tokenURI(uint256%20_tokenId)%20external%20view%20returns%20(string)%3B). As such, I agree with Medium Severity.

**[pi0neerpat (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/243#issuecomment-1805207884):**
 > This is a valid finding and recommendation is accurate.

***

## [[M-10] Due to extremely short `votingDelay` and `votingPeriod`, governance is practically impossible](https://github.com/code-423n4/2023-10-opendollar-findings/issues/202)
*Submitted by [Kaysoft](https://github.com/code-423n4/2023-10-opendollar-findings/issues/202), also found by [spark](https://github.com/code-423n4/2023-10-opendollar-findings/issues/441), [immeas](https://github.com/code-423n4/2023-10-opendollar-findings/issues/438), [Arz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/416), [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/354), [perseus](https://github.com/code-423n4/2023-10-opendollar-findings/issues/331), [btk](https://github.com/code-423n4/2023-10-opendollar-findings/issues/213), [holydevoti0n](https://github.com/code-423n4/2023-10-opendollar-findings/issues/207), [fibonacci](https://github.com/code-423n4/2023-10-opendollar-findings/issues/139), [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/74), and [hals](https://github.com/code-423n4/2023-10-opendollar-findings/issues/369)*

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/gov/ODGovernor.sol#L41> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/gov/ODGovernor.sol#L31-L41>

It is practically impossible for enough users to vote within a voting period of 4 seconds from the voting start time.

### Proof of Concept

The problem lies in the `votingDelay` and `votingPeriod` that are set in the ODGovernor.sol this way.

    contract ODGovernor is
      Governor,
      GovernorSettings,
      GovernorCompatibilityBravo,
      GovernorVotes,
      GovernorVotesQuorumFraction,
      GovernorTimelockControl
    {
    ...
    constructor(
        address _token,
        TimelockController _timelock
      )
        Governor('ODGovernor')//@audit larger voting delay gives users the time to unstake tokens if necessary.
        GovernorSettings(1, 15, 0)//@audit voting period 15blocks too small and no function to update. quorum will never be reached.
        GovernorVotes(IVotes(_token))
        GovernorVotesQuorumFraction(3)
        GovernorTimelockControl(_timelock)
      {}//@audit votingPeriod: how long does a proposal remain open to votes.
    ...
    }

In the constructor above the GovernorSettings constructor is used to set the votingDelay and votingPeriod.

```solidity
GovernorSettings(initialVotingDelay, initialVotingPeriod, initialProposalThreshold) measured in blocks
GovernorSettings(1, 15, 0)
```

The voting delay and voting period are measured in blocks. Since this project is built \`specifically for Arbitrum, we will consider Arbitrum particularly to set this values.

*   The average `blocktime` on `arbitrum` is `0.26 seconds`.
*   so setting 1 voting delay in the code above implies that there is just 0.26 seconds delay from proposal to voting start time.
*   And setting `15` voting period above implies that users have just 15 &ast; 0.26 = `3.9 seconds` to cast their vote.

It will be more practical to set atleast 1 day voting delay and atleast 1 week voting period as set in OZ governance examples.<br>
To convert these times to blocks on Arbitrum based on 0.26 secs avg blocktime:<br>
1 day = 86400 / 0.26 = \~332, 308 blocks per day<br>
So 1 week will be 332, 308 &ast; 7 = 2,326,156 blocks per week.

So it will be more practical for GovernorSettings parameters to be set this way:

```solidity
//votingDelay = 1day and votingPeriod = 1week.
GovernorSettings(332308, 2326156, 0); //particulary for Arbitrum based on block numbers.
```

Calculation reference from OZ docs: <https://docs.openzeppelin.com/contracts/4.x/governance#governor>

In the inherited OZ's Governor.sol, the voting delay and voting period are used to set each proposal's voting start time and deadline in the propose function this way.

    function propose(
            address[] memory targets,
            uint256[] memory values,
            bytes[] memory calldatas,
            string memory description
        ) public virtual override returns (uint256) {
    ...
     ProposalCore storage proposal = _proposals[proposalId];
     require(proposal.voteStart.isUnset(), "Governor: proposal already exists");
    uint64 snapshot = block.number.toUint64() + votingDelay().toUint64();//@audit voting delay.
            uint64 deadline = snapshot + votingPeriod().toUint64();

            proposal.voteStart.setDeadline(snapshot);
            proposal.voteEnd.setDeadline(deadline);
    ...
    }

### Tools Used

Openzeppelin Governance docs, OZ's smart contract wizard. 

<https://docs.openzeppelin.com/contracts/4.x/governance#governor>

### Recommended Mitigation Steps

*   Short term: Use Oz's examples but do your calculations based on Arbitrum block time:

```

    GovernorSettings(332308, 2326156, 0); //particulary for Arbitrum based on block numbers.
```

OZ's wizard example: <https://wizard.openzeppelin.com/#governor>

*   Long term: Use latest Oz's from 4.9 and above that have time based voting instead of block number. read here: <https://docs.openzeppelin.com/contracts/4.x/governance#disclaimer>

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/202#issuecomment-1790189096):**
 > The warden has demonstrated how the configured values for `GovernorSettings` are far too small for any effective governance to take place, since users only have ~4 seconds to cast votes. Therefore, all governor-related functionality in the `Vault721` contract will be unaccessible.
> 
> Since this only impacts setter functions and does not affect the protocol's core functionality, I believe medium severity is appropriate.

**[pi0neerpat (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/202#issuecomment-1805209209):**
 > The governance settings currently set are for testing, not production; however, finding is valid as this was not explicitly stated before the audit. Additionally, recommendation for both short and long-term solutions is appreciated.

***

## [[M-11] Test addresses and incorrect interface in code prevent integration with UniswapV3 and Camelot](https://github.com/code-423n4/2023-10-opendollar-findings/issues/187)
*Submitted by [twicek](https://github.com/code-423n4/2023-10-opendollar-findings/issues/187), also found by [kutugu](https://github.com/code-423n4/2023-10-opendollar-findings/issues/444), [spark](https://github.com/code-423n4/2023-10-opendollar-findings/issues/442), 0xMosh ([1](https://github.com/code-423n4/2023-10-opendollar-findings/issues/401), [2](https://github.com/code-423n4/2023-10-opendollar-findings/issues/399)), [pep7siup](https://github.com/code-423n4/2023-10-opendollar-findings/issues/397), [0xhacksmithh](https://github.com/code-423n4/2023-10-opendollar-findings/issues/347), [ni8mare](https://github.com/code-423n4/2023-10-opendollar-findings/issues/312), [Arz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/292), [btk](https://github.com/code-423n4/2023-10-opendollar-findings/issues/236), [bitsurfer](https://github.com/code-423n4/2023-10-opendollar-findings/issues/224), and xAriextz ([1](https://github.com/code-423n4/2023-10-opendollar-findings/issues/121), [2](https://github.com/code-423n4/2023-10-opendollar-findings/issues/119))*

Testing addresses for Camelot and UniswapV3 factories are still used in the code:

[CamelotRelayer.sol#L20](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L20)

```solidity
  address internal constant _CAMELOT_FACTORY = GOERLI_CAMELOT_V3_FACTORY;
```

[UniV3Relayer.sol#L18](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/UniV3Relayer.sol#L18)

```solidity
  address internal constant _UNI_V3_FACTORY = GOERLI_UNISWAP_V3_FACTORY;
```

Additionally, the correct interface to get the `camelotPair` is commented out:

[CamelotRelayer.sol#L41-L42](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L41-L42)

```solidity
    // camelotPair = ICamelotFactory(_CAMELOT_FACTORY).getPair(_baseToken, _quoteToken);
    camelotPair = IAlgebraFactory(_CAMELOT_FACTORY).poolByPair(_baseToken, _quoteToken);
```

### Impact

It will prevent integration with UniswapV3 and Camelot.

### Recommended Mitigation Steps

Make the changes necessary to be compatible with Arbitrum One.

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/187#issuecomment-1790139769):**
 > While the protocol team is most likely aware of this and these addresses are probably set to testnet ones currently for testing purposes, it is an undeniable fact that if the current code was to be deployed without any modifications, the `CamelotRelayer` and `UniV3Relayer` contracts would not function as expected.
> 
> As such, I believe medium severity is appropriate.

**[pi0neerpat (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/187#issuecomment-1805211162):**
 > The problem stated is that the Camelot and Uniswap factory addresses set in the Registry are set to Goerli addresses, not Mainnet. In production, we will swap Goerli addresses for Mainnet. Perhaps there is a better way to manage this than manually changing the constant variable in the Registry, however the recommendation does provide a helpful suggestion.

***

## [[M-12] Approved address can approve other addresses for an owner's safe](https://github.com/code-423n4/2023-10-opendollar-findings/issues/171)
*Submitted by [MrPotatoMagic](https://github.com/code-423n4/2023-10-opendollar-findings/issues/171), also found by [hals](https://github.com/code-423n4/2023-10-opendollar-findings/issues/451), [Tendency](https://github.com/code-423n4/2023-10-opendollar-findings/issues/396), [HChang26](https://github.com/code-423n4/2023-10-opendollar-findings/issues/373), [merlin](https://github.com/code-423n4/2023-10-opendollar-findings/issues/252), [COSMIC-BEE-REACH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/247), [twcctop](https://github.com/code-423n4/2023-10-opendollar-findings/issues/219), [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/211), [Bughunter101](https://github.com/code-423n4/2023-10-opendollar-findings/issues/194), [josephdara](https://github.com/code-423n4/2023-10-opendollar-findings/issues/183), [Stormreckson](https://github.com/code-423n4/2023-10-opendollar-findings/issues/114), [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/77), and [klau5](https://github.com/code-423n4/2023-10-opendollar-findings/issues/449)*

An owner of a safe can give permissions/approval of their safe to another address (let's say address B) through the [allowSafe() function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105) in the ODSafeManager.sol contract. But this other address (address B) also gets the power to approve other addresses for the owner's safe. This is a permissioning problem in the `allowSafe()` function (specifically the `safeAllowed()` modifier) which creates a security risk for the owner's safe.

### Proof of Concept

Here is the whole process:

<https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L105C1-L109C4>

1.  Owner of the safe gives permission/approval to `address _usr` (address 0x01 for example purposes) for `uint256 _safe` through the `allowSafe()` function.

*   On Line 107, `safeCan[_owner][_safe][_usr] = _ok;` is set to any value other than 0 to represent approval.

```solidity
File: ODSafeManager.sol
105:   function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
106:     address _owner = _safeData[_safe].owner;
107:     safeCan[_owner][_safe][_usr] = _ok;
108:     emit AllowSAFE(msg.sender, _safe, _usr, _ok);
109:   }
```

2.  The previously set `address _usr` (address 0x01) can now call the `allowSafe()` function with parameters `uint256 _safe` which will be the owner's safe and another `address _usr` (address 0x02), which will give address 0x02 permissions/approval for the owner's safe.

3.  This issue arises because of how the checks are evaluated in the [safeAllowed() modifier](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L49C1-L53C4). Here is what happens:

*   On Line 50, the owner of the safe is extracted.
*   On Line 51, there are two conditions present that are separated by the && operator.
*   On Line 51, the first check evaluates to true, since the msg.sender (address 0x01) is not the owner of the safe
*   On Line 51, the second check evaluates to false, since the msg.sender (address 0x01) was previously approved by the owner in step 1 above.
*   Since true && false = false, we do not revert and this gives address 0x02 permissions to the owner's safe in the `allowSafe()` function.

```solidity
File: ODSafeManager.sol
49:   modifier safeAllowed(uint256 _safe) {
50:     address _owner = _safeData[_safe].owner;
51:     if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
52:     _;
53:   } 
```

### Recommended Mitigation Steps

Consider implementing a separate modifier for the `allowSafe()` function that only checks if the msg.sender is the owner. If true, then allow execution but if not then revert.

Solution:

```solidity
File: ODSafeManager.sol
modifier onlySafeOwner(uint256 _safe) {
    address _owner = _safeData[_safe].owner;
    if (msg.sender != _owner) revert SafeNotAllowed();
    _;
}
```

**[pi0neerpat (OpenDollar) confirmed, but disagreed with severity and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/171#issuecomment-1783564235):**
 > Medium vulnerability recommended. Safe access is still limited, requiring user confirmation. The design could be improved, as described here, to remove the unexpected behavior of allowing additional approvals beyond the initial recipient.

**[MiloTruck (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/171#issuecomment-1790107095):**
 > The warden has demonstrated how incorrect use of the `safeAllowed()` modifier on the `allowSAFE()` function allows permissioned addresses that are not the safe owner to give permissions to other addresses, which is not intended.
> 
> As this finding is contingent on the owner granting permissions to an address that becomes malicious, I believe medium severity is appropriate.


***

## [[M-13] ODSafeManager.enterSystem - Transfer wrong amount of collateral, debt](https://github.com/code-423n4/2023-10-opendollar-findings/issues/159)
*Submitted by [klau5](https://github.com/code-423n4/2023-10-opendollar-findings/issues/159)*

If src has less lockedCollateral and generatedDebt than safeHandler, `enterSystem` cannot succeed.

The `enterSystem` function is behaving differently than intended.

### Proof of Concept

The `ODSafeManager.enterSystem` function retrieves the amount of safeHandler's `lockedCollateral` and `generatedDebt` and sends it to safeHandler. The purpose of this function is to send \_src's `lockedCollateral` and `generatedDebt` to safeHandler, so it is sending wrong amounts.

```solidity
  function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
    SAFEData memory _sData = _safeData[_safe];
@>  ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
@>  int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
@>  int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
@>    _sData.collateralType, _src, _sData.safeHandler, _deltaCollateral, _deltaDebt
    );
    emit EnterSystem(msg.sender, _src, _safe);
  }
```

### Recommended Mitigation Steps

Use `lockedCollateral` and `generatedDebt` of \_src.

```diff
function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
  SAFEData memory _sData = _safeData[_safe];
+ ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _src);
- ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
  int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
  int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
  ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
    _sData.collateralType, _src, _sData.safeHandler, _deltaCollateral, _deltaDebt
  );
  emit EnterSystem(msg.sender, _src, _safe);
}
```

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/159#issuecomment-1791419211):**
 > The warden has demonstrated how `enterSystem()` calls `transferSAFECollateralAndDebt()` with collateral and debt amounts of the wrong safe handler, causing its functionality to be broken. As such, I agree with medium severity.

**[pi0neerpat (OpenDollar) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/159#issuecomment-1805212633):**
 > This appears to be accurate, as the collateral and debt amounts are being recorded from the destination address instead of the source address.

***

## [[M-14] Unable to retrieve price information with CamelotRelayer contract](https://github.com/code-423n4/2023-10-opendollar-findings/issues/156)
*Submitted by [klau5](https://github.com/code-423n4/2023-10-opendollar-findings/issues/156), also found by [nican0r](https://github.com/code-423n4/2023-10-opendollar-findings/issues/436), [Arz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/388), [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/316), [0x6d6164616e](https://github.com/code-423n4/2023-10-opendollar-findings/issues/244), [josephdara](https://github.com/code-423n4/2023-10-opendollar-findings/issues/181), [immeas](https://github.com/code-423n4/2023-10-opendollar-findings/issues/173), and [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/75)*

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L70> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L74> 

<https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/src/contracts/oracles/CamelotRelayer.sol#L93>

The price lookup feature of the CamelotRelayer contract does not work.

### Proof of Concept

The CamelotRelayer contract uses Uniswap V3's OracleLibrary to retrieve price information from Camelot pool. However, Camelot pool does not have the exact same interface as Uniswap V3, so the transaction will be reverted

The `getResultWithValidity` function of the CamelotRelayer contract calls `OracleLibrary.getOldestObservationSecondsAgo` and `OracleLibrary.consult`. And `read` function of the CamelotRelayer contract calls `OracleLibrary.consult` .

```solidity
function getResultWithValidity() external view returns (uint256 _result, bool _validity) {
    // If the pool doesn't have enough history return false
@>  if (OracleLibrary.getOldestObservationSecondsAgo(camelotPair) < quotePeriod) {
      return (0, false);
    }
    // Consult the query with a TWAP period of quotePeriod
@>  (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
    // Calculate the quote amount
    uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
      tick: _arithmeticMeanTick,
      baseAmount: baseAmount,
      baseToken: baseToken,
      quoteToken: quoteToken
    });
    // Process the quote result to 18 decimal quote
    _result = _parseResult(_quoteAmount);
    _validity = true;
  }

function read() external view returns (uint256 _result) {
    // This call may revert with 'OLD!' if the pool doesn't have enough cardinality or initialized history
@>  (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
    uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
      tick: _arithmeticMeanTick,
      baseAmount: baseAmount,
      baseToken: baseToken,
      quoteToken: quoteToken
    });
    _result = _parseResult(_quoteAmount);
  }
```

This is the OracleLibrary code from Uniswap V3. The `getOldestObservationSecondsAgo` function calls the pool contract's `slot0` , `observations`. The `consult` function calls the `observe` function of pool contract.

```solidity
function getOldestObservationSecondsAgo(address pool) internal view returns (uint32 secondsAgo) {
@>  (, , uint16 observationIndex, uint16 observationCardinality, , , ) = IUniswapV3Pool(pool).slot0();
    require(observationCardinality > 0, 'NI');

    (uint32 observationTimestamp, , , bool initialized) =
@>      IUniswapV3Pool(pool).observations((observationIndex + 1) % observationCardinality);

    // The next index might not be initialized if the cardinality is in the process of increasing
    // In this case the oldest observation is always in index 0
    if (!initialized) {
@>      (observationTimestamp, , , ) = IUniswapV3Pool(pool).observations(0);
    }

    secondsAgo = uint32(block.timestamp) - observationTimestamp;
}

function consult(address pool, uint32 secondsAgo)
    internal
    view
    returns (int24 arithmeticMeanTick, uint128 harmonicMeanLiquidity)
{
    require(secondsAgo != 0, 'BP');

    uint32[] memory secondsAgos = new uint32[](2);
    secondsAgos[0] = secondsAgo;
    secondsAgos[1] = 0;

    (int56[] memory tickCumulatives, uint160[] memory secondsPerLiquidityCumulativeX128s) =
@>      IUniswapV3Pool(pool).observe(secondsAgos);

    ...
}
```

<https://github.com/Uniswap/v3-periphery/blob/697c2474757ea89fec12a4e6db16a574fe259610/contracts/libraries/OracleLibrary.sol#L74>

<https://github.com/Uniswap/v3-periphery/blob/697c2474757ea89fec12a4e6db16a574fe259610/contracts/libraries/OracleLibrary.sol#L16>

However, the functions `slot0`, `observations`, and `observe` do not exist in Camelot's Pool. I found Camelot's pool through CAMELOT\_V3\_FACTORY (0x1a3c9B1d2F0529D97f2afC5136Cc23e58f1FD35B) at [Registry.s.sol](https://github.com/open-dollar/od-contracts/blob/v1.5.5-audit/script/Registry.s.sol#L58).

If you call `poolDeployer` view function at CAMELOT\_V3\_FACTORY, it returns [0x6Dd3FB9653B10e806650F107C3B5A0a6fF974F65](https://arbiscan.io/address/0x6dd3fb9653b10e806650f107c3b5a0a6ff974f65), and you can see the Pool code in this contract. <https://arbiscan.io/address/0x6dd3fb9653b10e806650f107c3b5a0a6ff974f65#code>

### Recommended Mitigation Steps

Don’t use Uniswap V3 OracleLibrary for Camelot. You need to implement your own logic to communicate with the Camelot pool. These are functions/variables in Camelot pool that are similar to Uniswap V3 Pool’s.

*   `slot0` →  `globalState` : <https://arbiscan.io/address/0x6dd3fb9653b10e806650f107c3b5a0a6ff974f65#code#F15#L24>
*   `observations` → `timepoints` : <https://arbiscan.io/address/0x1a3c9B1d2F0529D97f2afC5136Cc23e58f1FD35B#code#F3#L22>
    *   DataStorage is deployed at `createPool` and attached at new pool. <https://arbiscan.io/address/0x1a3c9B1d2F0529D97f2afC5136Cc23e58f1FD35B#code#F1#L67>
*   `observe` → `getTimepoints`  <https://arbiscan.io/address/0x6dd3fb9653b10e806650f107c3b5a0a6ff974f65#code#F3#L176>

**[pi0neerpat (OpenDollar) confirmed via duplicate issue 75](https://github.com/code-423n4/2023-10-opendollar-findings/issues/75)**

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/156#issuecomment-1790064695):**
 > The warden has demonstrated how the implementation of `CamelotRelayer.sol` is incompatible with Camelot pools, thereby breaking the functionality of the contract. Since there isn't any loss of funds, medium severity is appropriate.
> 
> This report was selected as best since its recommended mitigation is the most detailed.

***

## [[M-15] `transferSAFEOwnership()` does not fully transfer ownership](https://github.com/code-423n4/2023-10-opendollar-findings/issues/142)
*Submitted by [lsaudit](https://github.com/code-423n4/2023-10-opendollar-findings/issues/142), also found by [klau5](https://github.com/code-423n4/2023-10-opendollar-findings/issues/448), [twicek](https://github.com/code-423n4/2023-10-opendollar-findings/issues/443), [tnquanghuy0512](https://github.com/code-423n4/2023-10-opendollar-findings/issues/393), [0xPsuedoPandit](https://github.com/code-423n4/2023-10-opendollar-findings/issues/188), [xAriextz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/133), [0xhacksmithh](https://github.com/code-423n4/2023-10-opendollar-findings/issues/440), [Stormreckson](https://github.com/code-423n4/2023-10-opendollar-findings/issues/161), and [ZanyBonzy](https://github.com/code-423n4/2023-10-opendollar-findings/issues/41)*

Function `transferSAFEOwnership()` is only callable by `Only Vault721`: `require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');` and its purpose is to give the safe ownership to a `dst` address.
However, `transferSAFEOwnership()` does not reset `safeCan` mapping which is used by `safeAllowed` modifier.

This leads to some security issues. The modifier `safeAllowed` is used to verify if user has permission to the safe. Multiple of functions which perform critical operations on the safe use this modifier to verify is user has permission to call that operation on a given safe.

Since `transferSAFEOwnership()` transfer ownership to a different address, it should also reset all previously set privileges by the previous owner. However, `safeCan` mapping is not being reset in `transferSAFEOwnership()`

### Proof of Concept

Firstly, let's take a look how `safeAllowed` modifier is defined:

[File: ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L49-L53)

      modifier safeAllowed(uint256 _safe) {
        address _owner = _safeData[_safe].owner;
        if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
        _;
      }

For this PoC, let's consider user `A`, who's the owner of safe 123.<br>
He gives access to the safe to `X`, `Y`, so he calls:<br>
`allowSAFE(123, X, 1), allowSAFE(123, Y, 1)`;

[File: ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L49-L53)

     function allowSAFE(uint256 _safe, address _usr, uint256 _ok) external safeAllowed(_safe) {
        address _owner = _safeData[_safe].owner;
        safeCan[_owner][_safe][_usr] = _ok;
        emit AllowSAFE(msg.sender, _safe, _usr, _ok);
      }

After calling this function, `safeCan` mapping looks like this: `safeCan[A][123][X] = 1` and `safeCan[A][123][Y] = 1`.

Now, there's a call to transfer safe ownership to `_dst` B: `transferSAFEOwnership(123, B)`.

[File: ODSafeManager.sol](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L49-L53)

    function transferSAFEOwnership(uint256 _safe, address _dst) external {
        require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

        if (_dst == address(0)) revert ZeroAddress();
        SAFEData memory _sData = _safeData[_safe];
        if (_dst == _sData.owner) revert AlreadySafeOwner();

        _usrSafes[_sData.owner].remove(_safe);
        _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

        _usrSafes[_dst].add(_safe);
        _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);

        _safeData[_safe].owner = _dst;

        emit TransferSAFEOwnership(msg.sender, _safe, _dst);
      }

It modifies only `_usrSafes`, `_usrSafesPerCollat` and  `_safeData` mappings and does not modify `safeCan` mapping.
After `transferSAFEOwnership(123, B)` call, those mappings look like this:

    _usrSafes[B] = [123]
    _usrSafesPerCollat[B][_sData.collateralType] = [123]
    _safeData[123].owner = B;

    safeCan[A][123][X]  // notice this is from previous allowSAFE() call
    safeCan[A][123][Y]  // notice this is from previous allowSAFE() call

Now, at some point in the future, the safe goes back to user A. Function `transferSAFEOwnership()` is being called once again: `transferSAFEOwnership(123, A)`.

User `A` get access to the safe. However, he does not remember that he previously sets access to users `X` and `Y`. Those mappings:

    safeCan[A][123][X]
    safeCan[A][123][Y]

are still there - meaning that `X` and `Y` has still access to that safe, even though the safe was just transferred back to `A` and safe's permissions should be cleared.

Now, when user `X` or `Y` will try to call any function with `safeAllowed` modifier - they will be allowed, since `safeAllowed` will return true for them, because `X` and `Y` are still in `safeCan` mapping.

### Recommended Mitigation Steps

Whenever transfer ownership - make sure to reset `safeCan[old_owner][safe]` mapping.

**[pi0neerpat (OpenDollar) confirmed and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/142#issuecomment-1783599545):**
 > Agreed, we would expect that permissions on a safe are cleared upon transfer. It would be unexpected for permissions to persist through such transfers, even if you received back a safe you previously owned.

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/142#issuecomment-1790238191):**
 > The warden has demonstrated how `transferSAFEOwnership()` does not clear safe permissions in `safeCan`, which will cause previously "approved" addresses to still have access under the following conditions:
> 
> - An owner regains ownership of the safe after transferring it away. 
> - The previously "approved" address becomes malicious.
> - The owner does not call `allowSAFE()` beforehand to remove the "approved" address.
> 
> This is unintended functionality, but given that it requires multiple unlikely conditions for this to become a problem, I believe medium severity is appropriate.

***

# Low Risk and Non-Critical Issues

For this audit, 25 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2023-10-opendollar-findings/issues/140) by **MrPotatoMagic** received the top score from the judge.

*The following wardens also submitted reports: [hals](https://github.com/code-423n4/2023-10-opendollar-findings/issues/385), [phoenixV110](https://github.com/code-423n4/2023-10-opendollar-findings/issues/221), [immeas](https://github.com/code-423n4/2023-10-opendollar-findings/issues/175), [lsaudit](https://github.com/code-423n4/2023-10-opendollar-findings/issues/169), [mrudenko](https://github.com/code-423n4/2023-10-opendollar-findings/issues/130), [0xMosh](https://github.com/code-423n4/2023-10-opendollar-findings/issues/426), [Tendency](https://github.com/code-423n4/2023-10-opendollar-findings/issues/417), [okolicodes](https://github.com/code-423n4/2023-10-opendollar-findings/issues/392), [0xhacksmithh](https://github.com/code-423n4/2023-10-opendollar-findings/issues/345), [Baki](https://github.com/code-423n4/2023-10-opendollar-findings/issues/328), [spark](https://github.com/code-423n4/2023-10-opendollar-findings/issues/320), [twicek](https://github.com/code-423n4/2023-10-opendollar-findings/issues/300), [m4k2](https://github.com/code-423n4/2023-10-opendollar-findings/issues/274), [fibonacci](https://github.com/code-423n4/2023-10-opendollar-findings/issues/261), [Al-Qa-qa](https://github.com/code-423n4/2023-10-opendollar-findings/issues/255), [kutugu](https://github.com/code-423n4/2023-10-opendollar-findings/issues/246), [Bughunter101](https://github.com/code-423n4/2023-10-opendollar-findings/issues/200), [xAriextz](https://github.com/code-423n4/2023-10-opendollar-findings/issues/192), [0xPsuedoPandit](https://github.com/code-423n4/2023-10-opendollar-findings/issues/185), [Stormreckson](https://github.com/code-423n4/2023-10-opendollar-findings/issues/122), [T1MOH](https://github.com/code-423n4/2023-10-opendollar-findings/issues/120), [8olidity](https://github.com/code-423n4/2023-10-opendollar-findings/issues/110), [Krace](https://github.com/code-423n4/2023-10-opendollar-findings/issues/80), and [eeshenggoh](https://github.com/code-423n4/2023-10-opendollar-findings/issues/69).*

## Summary

| QA issues | Issues                                                                                                                                 | Instances |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------|-----------|
| [L-01]    | Image field does not point to an image but the testnet website                                                                         | 1         |
| [L-02]    | Missing check in constructor to see if quotePeriod is not zero                                                                         | 1         |
| [L-03]    | Missing cardinality check in function read()                                                                                           | 1         |
| [L-04]    | Rounding down in `_exitCollateral()` function can cause loss of precision leading to loss of funds for users                           | 1         |
| [N-01]    | Remove redundant condition in `_isNotProxy()` function.                                                                                | 1         |
| [N-02]    | Remove `if (_dst == address(0)) revert ZeroAddress();` redundant check since it is already checked in `_afterTokenTransfer()` function | 1         |
| [N-03]    | Function read() does not revert with "OLD!" as mentioned in comments                                                                   | 1         |
| [N-04]    | Remove `if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();` redundant check                                  | 1         |
| [R-01]    | Consider modifying the build() function which allows anyone to create an ODProxy for any user                                          | 1         |
| [R-02]    | Use `user` instead of `usr` in mappings to improve readability                                                                         | 2         |
| [R-03]    | Use `safeAllow` and `handlerAllow` instead of `safeCan` and `handlerCan` to better match the intention of the mappings                 | 2         |
| [R-04]    | Add brackets around `10 ** multiplier` to improve code readability and provide clarity in which operation takes precedence first       | 1         |

**Note: N = Non-Critical, L = Low-severity, R = Recommendation**

## [L-01] Image field does not point to an image but the testnet website

There is 1 instance of this issue:

In the below string variable contractMetaData, we can observe the image field points as such `"image": "https://app.opendollar.com/collectionImage.png"`. If we follow the link it leads us to the testnet website and not an image.
```solidity
File: Vault721.sol
22:   string public contractMetaData =
23:     '{"name": "Open Dollar Vaults","description": "Tradable Vaults for the Open Dollar stablecoin protocol. Caution! Trading this NFT means trading the ownership of your Vault in the Open Dollar protocol and all of the assets/collateral inside each Vault.","image": "https://app.opendollar.com/collectionImage.png","external_link": "https://opendollar.com"}';
```

## [L-02] Missing check in constructor to see if quotePeriod is not zero

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L59

```solidity
File: CamelotRelayer.sol
59:     quotePeriod = _quotePeriod;
```

## [L-03] Missing cardinality check in function read()

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L91C1-L101C4

On Line 93, the comment mentions that the read() function should revert with "OLD!" if the pool does not have enough cardinality or initialized history. But there is no check done for the cardinality, which can return an incorrect quote.
```solidity
File: CamelotRelayer.sol
092:   function read() external view returns (uint256 _result) {
093:     // This call may revert with 'OLD!' if the pool doesn't have enough cardinality or initialized history
094:     (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
095:     uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
096:       tick: _arithmeticMeanTick,
097:       baseAmount: baseAmount,
098:       baseToken: baseToken,
099:       quoteToken: quoteToken
100:     });
101:     _result = _parseResult(_quoteAmount);
102:   }
```
Solution:
```solidity
File: CamelotRelayer.sol
69:   function read() external view returns (uint256 _result) {
70:     // If the pool doesn't have enough history return false
71:     if (OracleLibrary.getOldestObservationSecondsAgo(camelotPair) < quotePeriod) {
72:       return (0, false);
73:     }
74:     // Consult the query with a TWAP period of quotePeriod
75:     (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
76:     // Calculate the quote amount
77:     uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
78:       tick: _arithmeticMeanTick,
79:       baseAmount: baseAmount,
80:       baseToken: baseToken,
81:       quoteToken: quoteToken
82:     });
83:     // Process the quote result to 18 decimal quote
84:     _result = _parseResult(_quoteAmount);
85:   }
```

## [L-04] Rounding down in \_exitCollateral() function can cause loss of precision leading to loss of funds for users

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/CommonActions.sol#L118

In the function [_exitCollateral()](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/CommonActions.sol#L118), users can experience loss of funds if the wad amount is smaller than the decimals representation in the denominator.

The below code snippet is from the CommonActions.sol contract is inherited in the BasicActions.sol contract. This _exitCollateral() function is called when the freeTokenCollateral() function in the BasicActions.sol contract is called. This function does the operation below based on the decimals of the ERC20 token being used. In case the numerator i.e. the _wad amount is smaller than the denominator, the final _weiAmount rounds down to zero. This can lead to loss of funds for the user who tries to exit with his collateral.
```solidity
File: CommonActions.sol
118:     uint256 _weiAmount = _wad / 10 ** (18 - _decimals);
119:     __collateralJoin.exit(msg.sender, _weiAmount);
```

## [N-01] Remove redundant condition in \_isNotProxy() function

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L154C1-L156C4

The condition `ODProxy(_userRegistry[_user]).OWNER() != _user` is redundant and the first condition itself is sufficient to determine whether a user has an ODProxy or not. There's never a situation where the second condition could evaluate to true. Look at the table below:

| First condition | Second condition                |
|-----------------|---------------------------------|
| True            | Not checked since first is true |
| False           | False                           |

```solidity
File: Vault721.sol
163:   function _isNotProxy(address _user) internal view returns (bool) {
164:     return _userRegistry[_user] == address(0) || ODProxy(_userRegistry[_user]).OWNER() != _user;
165:   }
```

## [N-02] Remove `if (_dst == address(0)) revert ZeroAddress();` redundant check since it is already checked in `_afterTokenTransfer()` function

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L139

Remove below check:
```solidity
File: src/contracts/proxies/ODSafeManager.sol
139: if (_dst == address(0)) revert ZeroAddress();
```
Check already done before in _afterTokenTransfer() function:
```solidity
File: Vault721.sol
206:       if (_isNotProxy(to)) {
207:         proxy = _build(to);
208:       } else {
209:         proxy = payable(_userRegistry[to]);
210:       }
```

## [N-03] Function read() does not revert with "OLD!" as mentioned in comments

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/CamelotRelayer.sol#L91C1-L101C4

Function does not revert with "OLD!" since there is no such revert message in the consult function.
```solidity
File: CamelotRelayer.sol
092:   function read() external view returns (uint256 _result) {
093:     // This call may revert with 'OLD!' if the pool doesn't have enough cardinality or initialized history 
094:     (int24 _arithmeticMeanTick,) = OracleLibrary.consult(camelotPair, quotePeriod);
095:     uint256 _quoteAmount = OracleLibrary.getQuoteAtTick({
096:       tick: _arithmeticMeanTick,
097:       baseAmount: baseAmount,
098:       baseToken: baseToken,
099:       quoteToken: quoteToken
100:     });
101:     _result = _parseResult(_quoteAmount);
102:   }
```

## [N-04] Remove `if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();` redundant check

There is 1 instance of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L225

Remove check below:
```solidity
225: if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
```
Check already [done before in the same function](https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L201):
```solidity
201: if (extraSurplusReceiver == address(0)) revert AccEng_NullSurplusReceiver();
```

## [R-01] Consider modifying the build() function which allows anyone to create an ODProxy for any user

There is 1 instance of this issue:

The build() function on Line 90 below allows anyone to create an ODProxy for any user. Although this is a convenient method, it should still implement some sort of approval mechanism where the caller can deploy an ODProxy only on the approval of the user (for whom the proxy is being created). That way the method is less prone to frontrunning attacks that could DOS the user's call (since the Proxy would already be created but user call fails when trying to build one).
```solidity
File: Vault721.sol
81:   function build() external returns (address payable _proxy) {
82:     if (!_isNotProxy(msg.sender)) revert ProxyAlreadyExist();
83:     _proxy = _build(msg.sender);
84:   }
85: 
86:   /**
87:    * @dev allows user without an ODProxy to deploy a new ODProxy
88:    */
89:   //@audit Low - allows anyone to build an ODProxy for any user
90:   function build(address _user) external returns (address payable _proxy) {
91:     if (!_isNotProxy(_user)) revert ProxyAlreadyExist();
92:     _proxy = _build(_user);
93:   }
```

## [R-02] Use `user` instead of `usr` in mappings to improve readability

There are 2 instances of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L32C2-L34C110

Instead of `_usrSafes` use `_userSafes` and `_userSafesPerCollat` instead of `_usrSafesPerCollat`
```solidity
File: ODSafeManager.sol
32:   mapping(address _safeOwner => EnumerableSet.UintSet) private _usrSafes;
33:   /// @notice Mapping of user addresses to their enumerable set of safes per collateral type
34:   mapping(address _safeOwner => mapping(bytes32 _cType => EnumerableSet.UintSet)) private _usrSafesPerCollat; 
```

## [R-03] Use `safeAllow` and `handlerAllow` instead of `safeCan` and `handlerCan` to better match the intention of the mappings

There are 2 instances of this issue:

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L39C1-L42C1

These mappings represent the allowances the `_owner` gives to other addresses. To suit this intention of allowance, it would be better to rename the mappings to `safeAllow` and `handlerAllow`.
```solidity
File: ODSafeManager.sol
40:   mapping(address _owner => mapping(uint256 _safeId => mapping(address _caller => uint256 _ok))) public safeCan;
41:   /// @inheritdoc IODSafeManager
42:   mapping(address _safeHandler => mapping(address _caller => uint256 _ok)) public handlerCan;
```

## [R-04] Add brackets around `10 ** multiplier` to improve code readability and provide clarity in which operation takes precedence first

There is 1 instance of this issue:

Instead of this:
```solidity
File: CamelotRelayer.sol
104:   function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {
105:     return _quoteResult * 10 ** multiplier;
106:   }
```
Use this:
```solidity
File: CamelotRelayer.sol
104:   function _parseResult(uint256 _quoteResult) internal view returns (uint256 _result) {
105:     return _quoteResult * (10 ** multiplier);
106:   }
```

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/140#issuecomment-1788045525)**

**[MiloTruck (Judge) commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/140#issuecomment-1792916764):**
 > Note that the warden also has [#288](https://github.com/code-423n4/2023-10-opendollar-findings/issues/288), [#172](https://github.com/code-423n4/2023-10-opendollar-findings/issues/172), [#104](https://github.com/code-423n4/2023-10-opendollar-findings/issues/104) as low severity findings.

***

# Gas Optimizations

For this audit, 8 reports were submitted by wardens detailing gas optimizations. The [report highlighted below](https://github.com/code-423n4/2023-10-opendollar-findings/issues/386) by **0xVolcano** received the top score from the judge.

*The following wardens also submitted reports: [0xhacksmithh](https://github.com/code-423n4/2023-10-opendollar-findings/issues/311), [0x11singh99](https://github.com/code-423n4/2023-10-opendollar-findings/issues/357), [JCK](https://github.com/code-423n4/2023-10-opendollar-findings/issues/349), [naman1778](https://github.com/code-423n4/2023-10-opendollar-findings/issues/344), [dharma09](https://github.com/code-423n4/2023-10-opendollar-findings/issues/280), [unique](https://github.com/code-423n4/2023-10-opendollar-findings/issues/271), and [nailkhalimov](https://github.com/code-423n4/2023-10-opendollar-findings/issues/180).*

## Summary

The findings from the bot should be reviewed as they contain some solid issues.<br>
The following findings were missed by the bot (in some cases, bot has identified some instances but missed others, I try to explain whenever this happens).

Gas estimates are done using the opcodes involved since most of this functions were not covered by tests.

## [G-01] Using immutable on variables that are only set in the constructor and never after (Save 14.7K Gas)

Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.

**Note: These were not found by the bot**

**Total Instances: 7**<br>
**Gas Per Instance: 2.1 K**<br>
**Total Gas Saved: 14.7K gas**

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L58
```solidity
File: /src/contracts/AccountingEngine.sol
58:  AccountingEngineParams public _params;
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/oracles/UniV3Relayer.sol#L23
```solidity
File: /src/contracts/oracles/UniV3Relayer.sol
23:  address public baseToken;

25:  address public quoteToken;

33:  uint128 public baseAmount;

35:  uint256 public multiplier;

37:  uint32 public quotePeriod;
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L26
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
26:  address public safeEngine;
```

## [G-02] Avoid making external calls if we don't have to

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/AccountingEngine.sol#L175-L181
```solidity
File: /src/contracts/AccountingEngine.sol
175:  function auctionDebt() external returns (uint256 _id) {
176:    if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

178:    uint256 _coinBalance = safeEngine.coinBalance(address(this));
179:    uint256 _debtBalance = safeEngine.debtBalance(address(this));

181:    if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
```
The if statement on the line 182 only depends on the variable `_debtBalance` and therefore we do not need to load `_coinBalance` before performing the check. 
In case of a revert on the if check, the gas used to do the external call `safeEngine.coinBalance(address(this));` would be wasted. We can minimize this cost by performing the check earlier as shown below.

```diff
diff --git a/src/contracts/AccountingEngine.sol b/src/contracts/AccountingEngine.sol
index 0f96326..1735f93 100644
--- a/src/contracts/AccountingEngine.sol
+++ b/src/contracts/AccountingEngine.sol
@@ -175,10 +175,10 @@ contract AccountingEngine is Authorizable, Modifiable, Disableable, IAccountingE
   function auctionDebt() external returns (uint256 _id) {
     if (_params.debtAuctionBidSize == 0) revert AccEng_DebtAuctionDisabled();

-    uint256 _coinBalance = safeEngine.coinBalance(address(this));
     uint256 _debtBalance = safeEngine.debtBalance(address(this));
-
     if (_params.debtAuctionBidSize > _unqueuedUnauctionedDebt(_debtBalance)) revert AccEng_InsufficientDebt();
+
+    uint256 _coinBalance = safeEngine.coinBalance(address(this));

     (_coinBalance, _debtBalance) = _settleDebt(_coinBalance, _debtBalance, _coinBalance);
     totalOnAuctionDebt += _params.debtAuctionBidSize;
```

## [G-03] Avoid Making any state reads if we can revert early (Save 2100 Gas)

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136-L152
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
136:  function transferSAFEOwnership(uint256 _safe, address _dst) external {
137:    require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

139:    if (_dst == address(0)) revert ZeroAddress();
140:    SAFEData memory _sData = _safeData[_safe];
141:    if (_dst == _sData.owner) revert AlreadySafeOwner();
```
Avoid reading from storage if we might revert early. The first check involves making a state read, `vault721` 
The second check, the if statement, checks if  a function parameter `_dst`  is equal to `address(0)` and reverts if it happens to be zero.
As it is, if `_dst` is zero address, we would consume gas in the require statement to make the state read(2100 Gas for the state read) and then revert on the second check
we can save some gas in the case of such a revert by moving the cheap check to the top and only reading state variable after validating the function parameter.

```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODS
afeManager.sol
index b3ab2f5..33cc7db 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -134,9 +134,10 @@ contract ODSafeManager is IODSafeManager {

   // Give the safe ownership to a dst address.
   function transferSAFEOwnership(uint256 _safe, address _dst) external {
+    if (_dst == address(0)) revert ZeroAddress();
+
     require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

-    if (_dst == address(0)) revert ZeroAddress();
     SAFEData memory _sData = _safeData[_safe];
     if (_dst == _sData.owner) revert AlreadySafeOwner();

```

## [G-04] Variable already cached but in the wrong order - refactor to save 1 SLOAD(100 Gas)

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L94-L99
```solidity
File: /src/contracts/proxies/Vault721.sol
94:  function mint(address _proxy, uint256 _safeId) external {
95:    require(msg.sender == address(safeManager), 'V721: only safeManager');
96:    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
97:    address _user = _proxyRegistry[_proxy];
98:    _safeMint(_user, _safeId);
99:  }
```

```diff
   function mint(address _proxy, uint256 _safeId) external {
     require(msg.sender == address(safeManager), 'V721: only safeManager');
-    require(_proxyRegistry[_proxy] != address(0), 'V721: non-native proxy');
-    address _user = _proxyRegistry[_proxy];
+     address _user = _proxyRegistry[_proxy];
+    require(_user != address(0), 'V721: non-native proxy');
+
     _safeMint(_user, _safeId);
   }
```

## [G-05] Modifier execution order can help save an entire SLOAD(Save 2100 Gas)

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L104-L114

**Note: This would only make sense if `governor` remains a state variable, bot has suggested making it immutable.**

```solidity
File: /src/contracts/proxies/Vault721.sol
104:  function updateNftRenderer(
105:    address _nftRenderer,
106:    address _oracleRelayer,
107:    address _taxCollector,
108:    address _collateralJoinFactory
109:  ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {
110:    address _safeManager = address(safeManager);
111:    require(_safeManager != address(0));
112:    _setNftRenderer(_nftRenderer);
113:    nftRenderer.setImplementation(_safeManager, _oracleRelayer, _taxCollector, _collateralJoinFactory);
114:  }
```

Our function makes use of 2 modifers `onlyGovernor` and `nonZero`. The modifiers are defined as follows
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L40-L43
```solidity
40:  modifier onlyGovernor() {
41:    if (msg.sender != governor) revert NotGovernor();
42:    _;
43:  }
```
https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/Vault721.sol#L48-L51
```solidity
48:  modifier nonZero(address _addr) {
49:    if (_addr == address(0)) revert ZeroAddress();
50:    _;
51:  }
```

The first modifier `onlyGovernor` which is also the first one to be executed in our function involves reading a state variable `governor`, Since this is the first SLOAD we consume 2100 Gas for the SLOAD.
The second modifier `nonZero` does not read any state variable, only parameters being passed to it, in our case , some function parameters.

According to the docs,multiple modifiers are evaluated in the order presented.

Due to how they are order in our calling function `updateNftRenderer` ie 
`  ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {`
The `onlyGovernor` is checked first and if `msg.sender` == `governor` we proceed to check the second modifier `nonZero` passing our function parameter `_oracleRelayer` and again proceed to the next or revert if the passed parameter `==` `address(0)`.

In case we do revert on the check `nonZero` it means, the gas cost used to do the SLOAD in the `onlyGovernor` check, would be wasted.
We can save some gas (2100 Gas) for the SLOAD if we reorder the modifier execution to first check for `nonZero` and only do `onlyGovernor` if all our parameters are not `zero`.

```diff
diff --git a/src/contracts/proxies/Vault721.sol b/src/contracts/proxies/Vault721.sol
index 54dc8f7..eca21a1 100644
--- a/src/contracts/proxies/Vault721.sol
+++ b/src/contracts/proxies/Vault721.sol
@@ -106,7 +106,7 @@ contract Vault721 is ERC721Enumerable {
     address _oracleRelayer,
     address _taxCollector,
     address _collateralJoinFactory
-  ) external onlyGovernor nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) {
+  ) external  nonZero(_oracleRelayer) nonZero(_taxCollector) nonZero(_collateralJoinFactory) onlyGovernor {
     address _safeManager = address(safeManager);
     require(_safeManager != address(0));
     _setNftRenderer(_nftRenderer);

```

## [G-06] Using storage instead of memory for structs/arrays saves gas (**Not found by the bot**)

### ODSafeManager.sol.transferSAFEOwnership(): Use storage for `_sData`

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L136-L152
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
136:  function transferSAFEOwnership(uint256 _safe, address _dst) external {

140:    SAFEData memory _sData = _safeData[_safe];
141:    if (_dst == _sData.owner) revert AlreadySafeOwner();

143:    _usrSafes[_sData.owner].remove(_safe);
144:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);

146:    _usrSafes[_dst].add(_safe);
147:    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);

149:    _safeData[_safe].owner = _dst;

151:    emit TransferSAFEOwnership(msg.sender, _safe, _dst);
152:  }
```
After changing to storage, we cache the fields that are being read several times
```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODS
afeManager.sol
index b3ab2f5..0b2346b 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -137,14 +137,16 @@ contract ODSafeManager is IODSafeManager {
     require(msg.sender == address(vault721), 'SafeMngr: Only Vault721');

     if (_dst == address(0)) revert ZeroAddress();
-    SAFEData memory _sData = _safeData[_safe];
-    if (_dst == _sData.owner) revert AlreadySafeOwner();
+    SAFEData storage _sData = _safeData[_safe];
+    address _owner = _sData.owner;
+    if (_dst == _owner) revert AlreadySafeOwner();

-    _usrSafes[_sData.owner].remove(_safe);
-    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
+    _usrSafes[_owner].remove(_safe);
+    bytes32 _collateralType = _sData.collateralType;
+    _usrSafesPerCollat[_owner][_collateralType].remove(_safe);

     _usrSafes[_dst].add(_safe);
-    _usrSafesPerCollat[_dst][_sData.collateralType].add(_safe);
+    _usrSafesPerCollat[_dst][_collateralType].add(_safe);

+     _sData.owner = _dst;

```

### ODSafeManager.sol.modifySAFECollateralization(): Use storage for `_sData` and cache fields being called repeatedly

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L155-L165
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
155:  function modifySAFECollateralization(
156:    uint256 _safe,
157:    int256 _deltaCollateral,
158:    int256 _deltaDebt
159:  ) external safeAllowed(_safe) {
160:    SAFEData memory _sData = _safeData[_safe];
161:    ISAFEEngine(safeEngine).modifySAFECollateralization(
162:      _sData.collateralType, _sData.safeHandler, _sData.safeHandler, _sData.safeHandler, _deltaCollateral, _deltaDebt
163:    );
164:    emit ModifySAFECollateralization(msg.sender, _safe, _deltaCollateral, _deltaDebt);
165: }
```

```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODS
afeManager.sol
index b3ab2f5..4d6e7ab 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -157,9 +157,10 @@ contract ODSafeManager is IODSafeManager {
     int256 _deltaCollateral,
     int256 _deltaDebt
   ) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
+    SAFEData storage _sData = _safeData[_safe];
+    address _safeHandler = _sData.safeHandler;
     ISAFEEngine(safeEngine).modifySAFECollateralization(
-      _sData.collateralType, _sData.safeHandler, _sData.safeHandler, _sData.safeHandler, _deltaCollateral, _deltaDebt
+      _sData.collateralType, _safeHandler, _safeHandler, _safeHandler, _deltaCollateral, _deltaDebt
     );
     emit ModifySAFECollateralization(msg.sender, _safe, _deltaCollateral, _deltaDebt);
   }
```

### ODSafeManager.sol.transferCollateral(): Use storage for `_sData`

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L168-L172
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
168:  function transferCollateral(uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {
169:    SAFEData memory _sData = _safeData[_safe];
170:    ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);
171:    emit TransferCollateral(msg.sender, _safe, _dst, _wad);
172:  }
```

```diff
   function transferCollateral(uint256 _safe, address _dst, uint256 _wad) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
+    SAFEData storage _sData = _safeData[_safe];
     ISAFEEngine(safeEngine).transferCollateral(_sData.collateralType, _sData.safeHandler, _dst, _wad);
     emit TransferCollateral(msg.sender, _safe, _dst, _wad);
   }
```

### ODSafeManager.sol.quitSystem(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L189-L202

**The bot only suggested changing `_safeInfo`** - `We can actually optimize more` 

```solidity
File: /src/contracts/proxies/ODSafeManager.sol
189:  function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {
190:    SAFEData memory _sData = _safeData[_safe];
191:    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
192:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
193:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
194:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
195:      _sData.collateralType, _sData.safeHandler, _dst, _deltaCollateral, _deltaDebt
196:    );

198:    // Remove safe from owner's list (notice it doesn't erase safe ownership)
199:    _usrSafes[_sData.owner].remove(_safe);
200:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
201:    emit QuitSystem(msg.sender, _safe, _dst);
202:  }
```

```diff
   function quitSystem(uint256 _safe, address _dst) external safeAllowed(_safe) handlerAllowed(_dst) {
-    SAFEData memory _sData = _safeData[_safe];
-    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
+    SAFEData storage _sData = _safeData[_safe];
+    bytes32 _collateralType= _sData.collateralType;
+    address _safeHandler = _sData.safeHandler;
+    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_collateralType, _safeHandler);
     int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
     int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
-      _sData.collateralType, _sData.safeHandler, _dst, _deltaCollateral, _deltaDebt
+      _collateralType, _safeHandler, _dst, _deltaCollateral, _deltaDebt
     );
+    address _owner = _sData.owner;

     // Remove safe from owner's list (notice it doesn't erase safe ownership)
-    _usrSafes[_sData.owner].remove(_safe);
-    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
+    _usrSafes[_owner].remove(_safe);
+    _usrSafesPerCollat[_owner][_collateralType].remove(_safe);
     emit QuitSystem(msg.sender, _safe, _dst);
   }

```

### ODSafeManager.sol.enterSystem(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L205-L214

**The bot only suggested changing `_safeInfo`** - `We can actually optimize more` 

```solidity
File: /src/contracts/proxies/ODSafeManager.sol
205:  function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
206:    SAFEData memory _sData = _safeData[_safe];
207:    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
208:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
209:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
210:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
211:      _sData.collateralType, _src, _sData.safeHandler, _deltaCollateral, _deltaDebt
212:    );
213:    emit EnterSystem(msg.sender, _src, _safe);
214:  }
```

```diff
   function enterSystem(address _src, uint256 _safe) external handlerAllowed(_src) safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
-    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_sData.collateralType, _sData.safeHandler);
+    SAFEData storage _sData = _safeData[_safe];
+    bytes32 _collateralType = _sData.collateralType;
+    address _safeHandler = _sData.safeHandler;
+    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_collateralType, _safeHandler);
     int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
     int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
-      _sData.collateralType, _src, _sData.safeHandler, _deltaCollateral, _deltaDebt
+      _collateralType, _src, _safeHandler, _deltaCollateral, _deltaDebt
     );
     emit EnterSystem(msg.sender, _src, _safe);
   }
```

### ODSafeManager.sol.moveSAFE(): Use storage for `_srcData,_dstData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L217-L232

**The bot only suggested changing `_safeInfo`** - `We can actually optimize more` 

```solidity
File: /src/contracts/proxies/ODSafeManager.sol
217:  function moveSAFE(uint256 _safeSrc, uint256 _safeDst) external safeAllowed(_safeSrc) safeAllowed(_safeDst) {
218:    SAFEData memory _srcData = _safeData[_safeSrc];
219:    SAFEData memory _dstData = _safeData[_safeDst];
220:    if (_srcData.collateralType != _dstData.collateralType) revert CollateralTypesMismatch();
221:    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);
222:    int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
223:    int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
224:    ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
225:      _srcData.collateralType, _srcData.safeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
226:    );

228:    // Remove safe from owner's list (notice it doesn't erase safe ownership)
229:    _usrSafes[_srcData.owner].remove(_safeSrc);
230:    _usrSafesPerCollat[_srcData.owner][_srcData.collateralType].remove(_safeSrc);
231:    emit MoveSAFE(msg.sender, _safeSrc, _safeDst);
232:  }
```

```diff
   function moveSAFE(uint256 _safeSrc, uint256 _safeDst) external safeAllowed(_safeSrc) safeAllowed(_safeDst) {
-    SAFEData memory _srcData = _safeData[_safeSrc];
-    SAFEData memory _dstData = _safeData[_safeDst];
-    if (_srcData.collateralType != _dstData.collateralType) revert CollateralTypesMismatch();
-    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcData.collateralType, _srcData.safeHandler);
+    SAFEData storage _srcData = _safeData[_safeSrc];
+    SAFEData storage _dstData = _safeData[_safeDst];
+
+    bytes32 _srcDataCollateralType= _srcData.collateralType;
+
+    if (_srcDataCollateralType != _dstData.collateralType) revert CollateralTypesMismatch();
+    address _srcDataOwner = _srcData.owner;
+    address _srcDataSafeHandler = _srcData.safeHandler;
+
+    ISAFEEngine.SAFE memory _safeInfo = ISAFEEngine(safeEngine).safes(_srcDataCollateralType, _srcDataSafeHandler);
     int256 _deltaCollateral = _safeInfo.lockedCollateral.toInt();
     int256 _deltaDebt = _safeInfo.generatedDebt.toInt();
     ISAFEEngine(safeEngine).transferSAFECollateralAndDebt(
-      _srcData.collateralType, _srcData.safeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
+      _srcDataCollateralType, _srcDataSafeHandler, _dstData.safeHandler, _deltaCollateral, _deltaDebt
     );

     // Remove safe from owner's list (notice it doesn't erase safe ownership)
-    _usrSafes[_srcData.owner].remove(_safeSrc);
-    _usrSafesPerCollat[_srcData.owner][_srcData.collateralType].remove(_safeSrc);
+    _usrSafes[_srcDataOwner].remove(_safeSrc);
+    _usrSafesPerCollat[_srcDataOwner][_srcDataCollateralType].remove(_safeSrc);
     emit MoveSAFE(msg.sender, _safeSrc, _safeDst);
   }
```

### ODSafeManager.sol.removeSAFE(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L242-L246
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
242:  function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
243:    SAFEData memory _sData = _safeData[_safe];
244:    _usrSafes[_sData.owner].remove(_safe);
245:    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
246:  }
```

```diff
   function removeSAFE(uint256 _safe) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
-    _usrSafes[_sData.owner].remove(_safe);
-    _usrSafesPerCollat[_sData.owner][_sData.collateralType].remove(_safe);
+    SAFEData storage _sData = _safeData[_safe];
+    address _owner = _sData.owner;
+    _usrSafes[_owner].remove(_safe);
+    _usrSafesPerCollat[_owner][_sData.collateralType].remove(_safe);
   }
```

### ODSafeManager.sol.protectSAFE(): Use storage for `_sData` and cache repeatedly read fields

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L249-L253
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
249:  function protectSAFE(uint256 _safe, address _liquidationEngine, address _saviour) external safeAllowed(_safe) {
250:    SAFEData memory _sData = _safeData[_safe];
251:    ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);
252:    emit ProtectSAFE(msg.sender, _safe, _liquidationEngine, _saviour);
253:  }
```

```diff
   function protectSAFE(uint256 _safe, address _liquidationEngine, address _saviour) external safeAllowed(_safe) {
-    SAFEData memory _sData = _safeData[_safe];
+    SAFEData storage _sData = _safeData[_safe];
     ILiquidationEngine(_liquidationEngine).protectSAFE(_sData.collateralType, _sData.safeHandler, _saviour);
     emit ProtectSAFE(msg.sender, _safe, _liquidationEngine, _saviour);
   }
```

## [G-07] Leverage mapping and dot notation for struct assignment

In dot notation, values are directly written to storage variable, When we use the current method in the code the compiler will allocate some memory to store the struct instance first before writing it to storage.

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/ODSafeManager.sol#L118-L124
```solidity
File: /src/contracts/proxies/ODSafeManager.sol
118:  function openSAFE(bytes32 _cType, address _usr) external returns (uint256 _id) {

124:    _safeData[_safeId] = SAFEData({owner: _usr, safeHandler: _safeHandler, collateralType: _cType});
```

```diff
diff --git a/src/contracts/proxies/ODSafeManager.sol b/src/contracts/proxies/ODSafeManager.sol
index b3ab2f5..ed1a730 100644
--- a/src/contracts/proxies/ODSafeManager.sol
+++ b/src/contracts/proxies/ODSafeManager.sol
@@ -120,8 +120,11 @@ contract ODSafeManager is IODSafeManager {

     ++_safeId;
     address _safeHandler = address(new SAFEHandler(safeEngine));
+    SAFEData storage _SAFEData = _safeData[_safeId];

-    _safeData[_safeId] = SAFEData({owner: _usr, safeHandler: _safeHandler, collateralType: _cType});
+    _SAFEData.owner = _usr;
+    _SAFEData.safeHandler = _safeHandler;
+    _SAFEData.collateralType = _cType;

     _usrSafes[_usr].add(_safeId);
     _usrSafesPerCollat[_usr][_cType].add(_safeId);
```

## [G-08] Don't cache if used only once

Caching here will simply increase gas cost, since it doesn't affect readability, we should not cache since we only reference the cached variables only once.

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L101-L111
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
101:    address _safeEngine = ODSafeManager(_manager).safeEngine();

105:    // Generates debt in the SAFE
106:    _modifySAFECollateralization(
107:      _manager,
108:      _safeId,
109:      0,
110:      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
111:    );
```

```diff
diff --git a/src/contracts/proxies/actions/BasicActions.sol b/src/contracts/proxies/actions/BasicActions.sol
index e45fd41..8ad98f9 100644
--- a/src/contracts/proxies/actions/BasicActions.sol
+++ b/src/contracts/proxies/actions/BasicActions.sol
@@ -98,7 +98,6 @@ contract BasicActions is CommonActions, IBasicActions {
     uint256 _safeId,
     uint256 _deltaWad
   ) internal {
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -107,7 +106,7 @@ contract BasicActions is CommonActions, IBasicActions {
       _manager,
       _safeId,
       0,
-      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
+      _getGeneratedDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
     );
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L128-L139
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
128:    address _safeEngine = ODSafeManager(_manager).safeEngine();


135:	// Paybacks debt to the SAFE
136:    _modifySAFECollateralization(
137:      _manager, _safeId, 0, _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
138:    );
139:  }
```

```diff
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -134,7 +133,7 @@ contract BasicActions is CommonActions, IBasicActions {

     // Paybacks debt to the SAFE
     _modifySAFECollateralization(
-      _manager, _safeId, 0, _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
+      _manager, _safeId, 0, _getRepaidDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler)
     );
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L179-L192
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
179:    address _safeEngine = ODSafeManager(_manager).safeEngine();


186:    // Locks token amount into the SAFE and generates debt
187:    _modifySAFECollateralization(
188:      _manager,
189:      _safeId,
190:      _collateralAmount.toInt(),
191:      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
192:    );
```

```diff
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -188,7 +187,7 @@ contract BasicActions is CommonActions, IBasicActions {
       _manager,
       _safeId,
       _collateralAmount.toInt(),
-      _getGeneratedDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
+      _getGeneratedDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler, _deltaWad)
     );
```

https://github.com/open-dollar/od-contracts/blob/f4f0246bb26277249c1d5afe6201d4d9096e52e6/src/contracts/proxies/actions/BasicActions.sol#L354-L367
```solidity
File: /src/contracts/proxies/actions/BasicActions.sol
354:    address _safeEngine = ODSafeManager(_manager).safeEngine();

361:    // Paybacks debt to the SAFE and unlocks token amount from it
362:    _modifySAFECollateralization(
363:      _manager,
364:      _safeId,
365:      -_collateralWad.toInt(),
366:      _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
367:    );
```

```diff
-    address _safeEngine = ODSafeManager(_manager).safeEngine();
     ODSafeManager.SAFEData memory _safeInfo = ODSafeManager(_manager).safeData(_safeId);
     ITaxCollector(_taxCollector).taxSingle(_safeInfo.collateralType);

@@ -363,7 +362,7 @@ contract BasicActions is CommonActions, IBasicActions {
       _manager,
       _safeId,
       -_collateralWad.toInt(),
-      _getRepaidDeltaDebt(_safeEngine, _safeInfo.collateralType, _safeInfo.safeHandler)
+      _getRepaidDeltaDebt(ODSafeManager(_manager).safeEngine(), _safeInfo.collateralType, _safeInfo.safeHandler)
     );
```

**[pi0neerpat (OpenDollar) confirmed](https://github.com/code-423n4/2023-10-opendollar-findings/issues/386#issuecomment-1788046606)**

***


# Audit Analysis

For this audit, 11 analysis reports were submitted by wardens. An analysis report examines the codebase as a whole, providing observations and advice on such topics as architecture, mechanism, or approach. The [report highlighted below](https://github.com/code-423n4/2023-10-opendollar-findings/issues/330) by **hunter_w3b** received the top score from the judge.

*The following wardens also submitted reports: [fouzantanveer](https://github.com/code-423n4/2023-10-opendollar-findings/issues/230), [ZanyBonzy](https://github.com/code-423n4/2023-10-opendollar-findings/issues/28), [clara](https://github.com/code-423n4/2023-10-opendollar-findings/issues/418), [jauvany](https://github.com/code-423n4/2023-10-opendollar-findings/issues/404), [SAAJ](https://github.com/code-423n4/2023-10-opendollar-findings/issues/351), [JCK](https://github.com/code-423n4/2023-10-opendollar-findings/issues/350), [wei3erHase](https://github.com/code-423n4/2023-10-opendollar-findings/issues/164), [0xweb3boy](https://github.com/code-423n4/2023-10-opendollar-findings/issues/154), [Myd](https://github.com/code-423n4/2023-10-opendollar-findings/issues/66), and [0xbrett8571](https://github.com/code-423n4/2023-10-opendollar-findings/issues/65).*

### Overview of the Open Dollar Audit

Open Dollar is a DeFi lending protocol designed for liquidity and stability. Users can lock Liquid Staking Tokens (LSTs) and assets into Collateralized Debt Positions (CDPs) through NFT Vaults, allowing them to borrow the &#36;OD stablecoin. Unlike other stablecoins, &#36;OD adjusts its redemption rate based on market volatility, enhancing flexibility and minimizing governance. Open Dollar aims to reduce LST volatility, boost token liquidity, and offers a unique tradable vaults system. It's a floating &#36;1.00 pegged stablecoin backed by LSTs, primarily for Arbitrum, and employs the GEB framework for CDPs. This protocol enables borrowing against staked assets, earning staking rewards, and liquidity through Non-Fungible Vaults (NFVs).

**The key contracts of Open Dollar protocol for this Audit are**:

These contracts are central to the functionality, security, and governance of the Open Dollar protocol. Focusing on them first will provide a solid foundation for understanding the protocol's operation and how it manages user assets and stability.

- **ODGovernor.sol**:
  ODGovernor contract manages the governance of the protocol, enabling modifications to key parameters. Understanding how governance functions is crucial for ensuring the protocol's stability and adaptability.

- **AccountingEngine.sol**:
  The AccountingEngine contract handles the surplus, debt, and auctions. It's essential to comprehend how the protocol manages these aspects as they directly impact the stability and liquidity of the platform.

- **Vault721.sol**:
  This contract serves as the Proxy Registry and Proxy Factory and is responsible for managing safe ownership, transfers, and approvals. Understanding how safes are managed is vital to ensure the security of users' assets.

- **ODProxy.sol**:
  ODProxy is critical for ensuring the safe and secure transfer of assets within the protocol. It's important to understand its functionality and restrictions.

- **ODSafeManager.sol**:
  As it manages safe movement and creation, it's essential to know how this contract operates, especially in terms of the interaction with Vault721 and safe management.

### System Overview

### Scope

- **AccountingEngine.sol**: AccountingEngine contract in the Open Dollar protocol manages the protocol's surplus and debt. It facilitates surplus and debt auctions, tracks debt queues, and enables settlement. Additionally, it can initiate surplus auctions, transfer surplus to designated receivers, and be disabled to handle post-settlement debt and surplus management.

- **CamelotRelayer.sol**: The CamelotRelayer contract in the Open Dollar protocol bridges data from a CamelotRelayer TWAP pool to a standard IBaseOracle format. It converts the price into a 18-decimal format. The contract is initialized with base and quote tokens, symbol, and other parameters. It offers functions to retrieve results from the CamelotRelayer pool, with checks for data validity. This adaptation is vital for integrating price data within the Open Dollar protocol.

- **CamelotRelayerFactory.sol**: The CamelotRelayerFactory contract in the Open Dollar protocol deploys CamelotRelayers, which fetch and transform price data. It's initialized with authorization controls and provides a method to create new CamelotRelayers with specific token pairs and periods. The contract maintains a list of deployed CamelotRelayers, essential for obtaining reliable price data within the Open Dollar system.

- **CamelotRelayerChild.sol**: The CamelotRelayerChild contract inherits all the functionality of the "CamelotRelayer" contract and is specifically designed to be deployed by the factory. It serves as a CamelotRelayer within the Open Dollar protocol, providing price data transformation and compatibility features for base and quote tokens with specified quote periods.

- **UniV3Relayer.sol**: The UniV3Relayer contract in the Open Dollar protocol fetches and transforms price data from a UniswapV3Pool into a standard IBaseOracle feed. It's configured with specific base and quote tokens, along with a fee tier and quote period for obtaining reliable price data. The contract ensures that the quote result is expressed in an 18 decimal format, making it compatible with the Open Dollar ecosystem.

- **ODGovernor.sol**: The ODGovernor contract in the Open Dollar protocol is a comprehensive governance contract that incorporates various modules and extensions for decentralized decision-making. It manages voting and proposals, configures parameters like voting delay and quorum, and integrates with the Open Dollar token and a timelock contract to ensure secure and effective governance processes.

- **ODProxy.sol**: The ODProxy contract is a simple proxy contract designed for use in the Open Dollar protocol. It allows the owner to execute functions on another contract (the target) via delegatecall. The contract enforces that only the owner can invoke these functions. It includes error handling for cases when the target address is required, the target call fails, or the sender is not the owner.

- **Vault721.sol**: The Vault721 contract is used in the Open Dollar protocol (Version 1.5.5) to manage tradable Vaults as ERC-721 tokens. It involves governance, initialization, proxy creation, and management. Users can interact with their Vaults and trade ownership of these assets through this contract.

- **SAFEHandler.sol**: The SAFEHandler contract is used to create a unique safe handler address for each user's SAFE within the Open Dollar protocol. It is deployed when a new SAFE is created and grants permissions to the SAFE manager to modify the SAFE associated with this contract's address.

- **ODSafeManager.sol**: The ODSafeManager contract serves as an interface to the SAFEEngine, facilitating the management of SAFEs in the Open Dollar protocol. It provides functions to open, modify, and transfer SAFEs, as well as control access permissions. The contract also interacts with the Vault721 contract for NFT management.

- **BasicActions.sol**: The BasicActions contract in the Open Dollar protocol facilitates the management of SAFEs. It allows users to open new SAFEs, generate and repay debt, lock and free token collateral, and manage collateralization ratios. These actions are crucial for maintaining the stability and usability of the protocol's stablecoin system.

### Privileged Roles

Some privileged roles exercise powers over the Controller contract:

- **Owner**

```solidity
  modifier onlyOwner() {
    if (msg.sender != OWNER) revert OnlyOwner();
    _;
  }
```

- **Control Access For DAO Governor**

```solidity
  modifier onlyGovernor() {
    if (msg.sender != governor) revert NotGovernor();
    _;
  }
```

- **Owner Of The Safe**

```solidity
  modifier safeAllowed(uint256 _safe) {
    address _owner = _safeData[_safe].owner;
    if (msg.sender != _owner && safeCan[_owner][_safe][msg.sender] == 0) revert SafeNotAllowed();
    _;
  }
```

- **Safe Handler**

```solidity
  modifier handlerAllowed(address _handler) {
    if (msg.sender != _handler && handlerCan[_handler][msg.sender] == 0) revert HandlerNotAllowed();
    _;
  }
```

### Approach Taken-in Evaluating The Open Dollar Protocol

Accordingly, I analyzed and audited the subject in the following steps;

1.  **Core Protocol Contract Overview**:

    I focused on thoroughly understanding the codebase and providing recommendations to improve its functionality.
    The main goal was to take a close look at the important contracts and how they work together in the Open Dollar Protocol.

    **Main Contracts I Looked At**

                ODGovernor.sol
                Vault721.sol
                ODSafeManager.sol
                ODProxy.sol
                SAFEHandler.sol
                AccountingEngine.sol
                BasicActions.sol
                CamelotRelayer.sol

    I started my analysis by examining the ODGovernor.sol contract. Audit the governance contract first as it has a central role in modifying protocol parameters, enabling modifications to protocol parameters and security settings. Its functions for proposal creation, voting, and quorum verification must be meticulously audited to ensure secure and accurate governance operations. Particular attention should be paid to timelock control mechanisms, role-based access controls, and protection against vulnerabilities.

    Then, I turned our attention to the Vault721.sol contract second because it manages all safe ownership, transfers, and approvals via the ERC721 standard, and it interacts closely with the ODSafeManager.

    Then ODSafeManager.sol contract next as it's essential to make sure it interacts securely with Vault721 and controls the creation and movement of safes within the system.

    Then audit `ODProxy`, which is used to restrict ownership changes to ensure protocol integrity. Security of this contract is important and `SAFEHandler` as it grants permissions for ODSafeManager to make modifications to a safe. The security of this interaction is crucial but may depend on the ODSafeManager.

    And `AccountingEngine` contract because it deals with the financial operations of the protocol. Vulnerabilities in this contract could have significant financial implications and then `BasicActions` contract, which contains core actions related to managing safes. It's important to ensure the security of these actions.Then `CamelotRelayer` contract as it's responsible for fetching the market price. The reliability of price oracles is crucial for accurate protocol operation.

2.  **Documentation Review**:

    Then went to Review [their docs](https://docs.opendollar.com) for a more detailed and technical explanation of Open Dollar.

3.  **Manual Code Review** In this phase, I initially conducted a line-by-line analysis, following that, I engaged in a comparison mode.

    - **Line by Line Analysis**: Pay close attention to the contract's intended functionality and compare it with its actual behavior on a line-by-line basis.

    - **Comparison Mode**: Compare the implementation of each function with established standards or existing implementations, focusing on the function names to identify any deviations.

### Architecture Description and Diagram

Architecture of the key contracts that are part of the Open Dollar protocol:

[This diagram](https://www.figma.com/file/g7S9iJpEvWALcRN0uC9j08/Open-Dollar-Diagram-v1?type=design&mode=design) illustrates the interaction among the key components of the Open Dollar protocol.


### Accounting Engine

The **Accounting Engine** plays a central role in the protocol. It receives system surplus and debt, manages debt queues, and conducts surplus and debt auctions.

### Oracle Relayers

- **Camelot Relayer**: Used by the Oracle Relayer, it fetches the current market price of the system coin (OD) from a Camelot pool on the Arbitrum network. This price data is essential for various protocol operations.

- **UniV3 Relayer**: An alternative option for fetching the OD's market price, used by the Oracle Relayer. It retrieves price data from a Uniswap V3 pool on the Arbitrum network.

### Governor (ODGovernor)

The **Governor (ODGovernor)** is the DAO-managed contract that governs the protocol. It can modify protocol parameters, such as adding new collateral types and changing PID settings. The Governor interacts with other protocol components to propose and execute changes.

### Proxy Components

- **ODProxy**: A more restrictive version of the DSProxy used by the Maker Protocol. It ensures that only the Vault721 contract has the ability to transfer a safe. This restriction is crucial for the security of the protocol.

- **Vault721**: Serves as the Proxy Registry, Proxy Factory, and the ERC721 "Non-fungible Vault." It manages all safe ownership, transfers, and approvals via the ERC721 standard. It tracks proxy ownership and deploys new proxies when necessary.

### SAFE Components

- **SAFEHandler**: Grants permission to the ODSafeManager to make modifications to a safe. A new SAFEHandler is deployed for each safe, and its address serves as a unique identifier within the SAFEEngine.

- **ODSafeManager**: A more restrictive Safe Manager, allowing only the Vault721 contract to move a safe. It also calls Vault721 to mint a new safe when one is created.

### BasicActions

These actions are part of the protocol and interact with various components as needed. They play a crucial role in executing various operations within the protocol.

The interaction among these components forms the backbone of the Open Dollar protocol, allowing it to manage its surplus and debt, obtain reliable price data, and govern its parameters securely. These interactions are essential for maintaining the stability and functionality of the protocol in the decentralized financial ecosystem.

### Architecture Feedback

**Comments and Documentation**: While the codebase contains comments, improving the quality of comments and documentation can enhance code readability. Consider using more explanatory comments to clarify the purpose and functionality of each section of the code. This will make it easier for other developers and auditor to understand and maintain the code.

**Decimals Handling**: When working with decimals, ensure that all conversions are handled accurately.For-Example The multiplier calculation in CamelotRelayer.sol could potentially benefit from additional comments and explanation for clarity.

**Formal Verification**: Consider a professional formal verification of the contract to identify and mitigate any security risks.

**Consolidation of Related Functions**:For-Exampe: Some functions like `_generateDebt` and `_repayDebt` in BasicActions.sol share common functionality, and combining them into a single function with parameters to specify the operation could reduce code duplication.

### Codebase Quality

Overall, I consider the quality of the Open Dollar codebase to be excellent. The code appears to be very mature and well-developed. We have noticed the implementation of various standards adhere to appropriately. Details are explained below:

| Codebase Quality Categories              | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Code Maintainability and Reliability** | The codebase demonstrates moderate maintainability with well-structured functions and comments, promoting readability. It exhibits reliability through defensive programming practices, parameter validation, and handling external calls safely. The use of internal functions for related operations enhances code modularity, reducing duplication. Libraries improve reliability by minimizing arithmetic errors. Adherence to standard conventions and practices contributes to overall code quality. However, full reliability depends on external contract implementations like openzeppelin, uniswap.                                                                       |
| **Code Comments**                        | The contracts have comments that are used to explain the purpose and functionality of different parts of the contracts, making it easier for developers to understand and maintain the code. The comments provide descriptions of methods, variables, and the overall structure of the contract.For-Exmaple: The code comments in the "CamelotRelayerChild" contract provide essential information. The imported interfaces and contracts are declared, and the contract's title and purpose are described.                                                                                                                                                                         |
| **Documentation**                        | The documentation of the Open Dollar project is quite comprehensive and detailed, providing a solid overview of how Open Dollar is structured and how its various aspects function. However, we have noticed that there is room for additional details, such as diagrams, to gain a deeper understanding of how different contracts interact and the functions they implement. With considerable enthusiasm. We are confident that these diagrams will bring significant value to the protocol as they can be seamlessly integrated into the existing documentation, enriching it and providing a more comprehensive and detailed understanding for users, developers and auditors. |
| **Testing**                              | The audit scope of the contracts to be audited is 95% and it should be aimed to be 100%.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Code Structure and Formatting**        | The codebase contracts are well-structured and formatted. It inherits from multiple components, adhering for-example OpenZeppelin governance standards in some contracts. The constructors initializes the contract with parameters and inherited components. Override functions for each component are provided with accompanying comments explaining their purpose. The code is organized, making it readable and maintainable.                                                                                                                                                                                                                                                   |

### Systemic & Centralization Risks

The analysis provided highlights several significant systemic and centralization risks present in the Open Dollar protocol. These risks encompass concentration risk in ODGovernor, BasicActions, AccountingEngine risk and more, third-party dependency risk, and centralization risks arising from the existence of an “owner” role in specific contracts. However, the documentation lacks clarity on whether this address represents an externally owned account (EOA) or a contract, warranting the need for clarification. Additionally, the absence of fuzzing and invariant tests could also pose risks to the protocol’s security.

Here's an analysis of potential systemic and centralization risks in the provided contracts:

### Systemic Risks:

1. **No having, fuzzing and invariant tests could open the door to future vulnerabilities**.

2. The AccountingEngine contract manages debt and surplus within the system, which can be a systemic risk if not executed correctly. Mishandling debt or surplus could lead to imbalances and impact the stability of the entire protocol, and this contract allows parameters to be modified. Inappropriately setting these parameters can introduce systemic risks. For example, setting incorrect surplus transfer percentages or auction bid sizes may lead to issues.

3. The CamelotRelayer.sol retrieves price information from an external source, the CamelotPair. Systemic risk exists if this source provides inaccurate or manipulated data, leading to incorrect price feeds and potentially affecting other parts of the protocol relying on this data and this contract converts and scales price results by manipulating decimals, if there are errors or discrepancies in this conversion, leading to incorrect pricing information and potentially impacting the entire system.

4. The "build" function allows users in Vault721 contract to create ODProxy contracts, but it can be abused. It does not have any checks or permissions other than verifying if a user already has an ODProxy. There is potential for abuse if users create excessive proxies, which could lead to misuse or spam.

5. In BasicActions.sol contract systemic risks might arise from the exit mechanisms, especially if there are discrepancies between the rates used in calculations and those from the external SAFE engine.

### Centralization Risks:

1. In AccountingEngine.sol contract allows specific accounts to be authorized and to modify parameters. Centralization risks are present if a small number of entities or individuals control these authorizations, potentially leading to centralized control over the protocol.

2. CamelotRelayer contract's constructor sets critical parameters like baseToken, quoteToken, baseAmount, multiplier, and quotePeriod. Centralization risks are present because these parameters are not governed by a decentralized process and are hardcoded and The `getResultWithValidity` function returns false if the pool doesn't have enough history. The validity of the result depends on the pool's history and may be influenced by external factors, introducing centralization risks.

3. The contract UniV3Relayer.sol doesn't provide a way to modify the fee tier or other pool-related parameters after deployment, which could be problematic if adjustments are needed based on changing market conditions.

4. The ODGovernor.sol specifies a fixed quorum fraction (3) that is not governed or adjustable through governance processes. Centralization risks arise as this parameter cannot be modified in response to changing conditions.

5. The owner's address is set as immutable in ODProxy.sol contract, meaning it cannot be changed once the contract is deployed. This lack of flexibility to change ownership through a decentralized process can be a centralization risk, as there is no governance mechanism in place to handle ownership transitions and the contract does not have a mechanism for upgrades or modifications. If there are bugs or vulnerabilities discovered in the contract's code, there is no way to implement improvements without deploying an entirely new contract, which can be cumbersome and disruptive.

**Properly managing these risks and implementing best practices in security and decentralization will contribute to the sustainability and long-term success of the Open Dollar protocol.**

### Conclusion

In general, the Open Dollar project exhibits an interesting and well-developed architecture we believe the team has done a good job
regarding the code, but the identified risks need to be addressed, and measures should be implemented to protect the protocol from
potential malicious use cases. Additionally, it is recommended to improve the documentation and comments in the code to enhance
understanding and collaboration among developers. It is also highly recommended that the team continues to invest in security
measures such as mitigation reviews, audits, and bug bounty programs to maintain the security and reliability of the project.

### Time spent:
14 hours

**[pi0neerpat (Open Dollar) acknowledged and commented](https://github.com/code-423n4/2023-10-opendollar-findings/issues/330#issuecomment-1788042126):**
 > Excellent thank you for your thorough analysis!

***
# Disclosures

C4 is an open organization governed by participants in the community.

C4 Audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
