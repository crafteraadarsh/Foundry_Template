# [Foundry](https://github.com/z0r0z/zenplate)  [![License: AGPL-3.0-only](https://img.shields.io/badge/License-AGPL-black.svg)](https://opensource.org/license/agpl-v3/) [![solidity](https://img.shields.io/badge/solidity-%5E0.8.19-black)](https://docs.soliditylang.org/en/v0.8.19/) [![Foundry](https://img.shields.io/badge/Built%20with-Foundry-000000.svg)](https://getfoundry.sh/) ![tests](https://github.com/z0r0z/zenplate/actions/workflows/ci.yml/badge.svg)

This Repo is an example of Fuzzing Testing using Foundry.
Forge supports property based testing.
Property-based testing is a way of testing general behaviors as opposed to isolated scenarios.
Let’s examine what that means by writing a unit test, finding the general property we are testing for, and converting it to a property-based test instead

## Getting Started

Run: `curl -L https://foundry.paradigm.xyz | bash && source ~/.bashrc && foundryup`

Build the foundry project with `forge build`. Run tests with `forge test`. Measure gas with `forge snapshot`. Format with `forge fmt`.

## Fuzzer.sol

This file defines a simple Solidity contract named `Fuzzer`. The contract has two public functions:

### `setNumber(uint256 newNumber)`

This function takes a `uint256` value as input and assigns it to the contract's `number` state variable.

### `increment()`

This function increments the value of the `number` state variable by 1.

## Test Contract: FuzzerTest

### Setup:

- `setUp()`: Creates an instance of the `Fuzzer` contract before each test.

### Test Cases:

#### `testFuzzIncrement()`

- Retrieves the initial value of `number`.
- Calculates the expected value after incrementing.
- Uses `vm.assume` to ensure the expected value is different from the initial value.
- Calls `increment()` to increment `number`.
- Asserts that the final value of `number` matches the expected value using `assertEq`.

#### `testFuzzSetNumber()`

- Generates a random number using a hash of block data.
- Uses `vm.assume` to ensure the generated number is not zero.
- Calls `setNumber()` to set the value of `number`.
- Asserts that `number` has been set correctly using `assertEq`.

#### Theory
### [`vm.assume`](https://book.getfoundry.sh/cheatcodes/assume)

* `vm.assume` is a function used for testing. It allows developers to specify assumptions about the state of the Ethereum Virtual Machine (EVM) during testing. These assumptions help in defining the preconditions or expected conditions before executing certain parts of the code. For instance, you might use `vm.assume` to ensure that a certain variable is within a specific range or has a particular value before executing a function.

### [`assertEq`](https://book.getfoundry.sh/reference/forge-std/assertEq)

* `assertEq` is an assertion function used in smart contract testing to verify that two values are equal. It compares the actual value of a variable or expression with an expected value and raises an error if they are not equal. This assertion is crucial for ensuring that contract functions behave as expected and produce the correct outcomes. Developers typically use `assertEq` extensively in unit tests to validate the behavior of smart contracts under various conditions.
### [`block.timestamp` & `block.prevrandao`](https://docs.soliditylang.org/en/latest/cheatsheet.html)
* `block.prevrandao` `(uint)`: random number provided by the beacon chain (EVM >= Paris) (see EIP-4399 )
* `block.timestamp` `(uint)`: current block timestamp in seconds since Unix epoch


