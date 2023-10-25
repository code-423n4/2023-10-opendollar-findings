## Gas Optimizations

|       | Issue                                                                                                                                                                |
| ----- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GAS-1 | Be careful using the `values` method in OpenZeppelin's EnumerableSet data structure, where gas costs can grow quickly and can result in a DoS of a method calling it |

### [GAS-1] Be careful using the `values` method in OpenZeppelin's EnumerableSet data structure, where gas costs can grow quickly and can result in a DoS of a method calling it

#### Description:

The `values` operation will copy the entire storage to memory, which can be quite expensive. This is designed to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that this function has an unbounded cost, and using it as part of a state-changing function may render the function uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.

Reference: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/structs/EnumerableSet.sol

#### **Proof Of Concept**

```solidity
File: src/contracts/factories/CamelotRelayerFactory.sol

35:     return _camelotRelayers.values();

```

```solidity
File: src/contracts/proxies/ODSafeManager.sol

74:     _safes = _usrSafes[_usr].values();

79:     _safes = _usrSafesPerCollat[_usr][_cType].values();

88:     _safes = _usrSafes[_usr].values();

```

#### Recommended Mitigation Steps:

Be careful with the "values" method, where gas costs can grow quickly (based on the set size) and can result in a DoS of a method calling it.
