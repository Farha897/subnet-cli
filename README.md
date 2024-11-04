# ERC20 and Vault Smart Contracts

## Overview

This project implements two Solidity smart contracts: an `ERC20` token contract and a `Vault` contract. The `ERC20` contract follows the ERC-20 token standard, providing a token with typical functions such as transfer, approve, and mint. The `Vault` contract acts as a staking vault for the ERC-20 tokens, allowing users to deposit tokens in exchange for shares and withdraw tokens by redeeming those shares.

### Contracts
1. **ERC20 Contract**
2. **Vault Contract**

## ERC20 Contract

The `ERC20` contract provides a basic implementation of an ERC-20 token.

### Features

- **Token Name**: Solidity by Example
- **Token Symbol**: SOLBYEX
- **Decimals**: 18

### Functions

- **`transfer(address recipient, uint amount) -> bool`**: Transfers tokens from the caller’s account to the `recipient`.
- **`approve(address spender, uint amount) -> bool`**: Approves a `spender` to transfer up to `amount` from the caller’s account.
- **`transferFrom(address sender, address recipient, uint amount) -> bool`**: Allows a `spender` to transfer tokens from a `sender` to a `recipient`.
- **`mint(uint amount)`**: Mints `amount` tokens to the caller's account, increasing `totalSupply`.
- **`burn(uint amount)`**: Burns `amount` tokens from the caller's account, decreasing `totalSupply`.

### Events

- **`Transfer(address indexed from, address indexed to, uint value)`**: Emitted on token transfers and minting/burning.
- **`Approval(address indexed owner, address indexed spender, uint value)`**: Emitted on approvals.

### Example

```solidity
ERC20 token = new ERC20();
token.mint(1000); // Mints 1000 tokens to caller's address
token.transfer(recipient, 500); // Transfers 500 tokens to recipient

# Vault Contract

The `Vault` contract is a Solidity smart contract designed to allow users to deposit an ERC-20 token in exchange for shares, which represent their proportional claim on the Vault's total assets. This contract is useful for implementing staking mechanisms or liquidity pools where users can deposit tokens and later withdraw them along with any rewards or interest accrued.

## Contract Summary

- **Token Type**: ERC-20
- **Functionality**: Deposit and withdraw ERC-20 tokens in exchange for Vault shares
- **License**: MIT
- **Solidity Version**: `^0.8.17`

## Constructor

- **`constructor(address _token)`**: Sets the ERC-20 token that will be accepted by the Vault. The `_token` address is stored as an immutable `IERC20` interface.

## State Variables

- **`IERC20 public immutable token`**: The ERC-20 token used in the Vault.
- **`uint public totalSupply`**: The total number of shares issued by the Vault.
- **`mapping(address => uint) public balanceOf`**: Maps each account to its balance of shares.

## Functions

### Public Functions

- **`deposit(uint _amount)`**: Allows a user to deposit `_amount` of the token into the Vault in exchange for shares. The amount of shares minted is proportional to the Vault’s balance before the deposit.

    **Formula**:
    ```
    shares = (amount * totalSupply) / balanceBeforeDeposit
    ```
    If `totalSupply` is zero, shares equal the deposited amount.

- **`withdraw(uint _shares)`**: Allows a user to withdraw tokens by redeeming `_shares`. The tokens withdrawn are proportional to the Vault’s balance before the withdrawal.

    **Formula**:
    ```
    amount = (shares * balanceBeforeWithdrawal) / totalSupply
    ```

### Private Functions

- **`_mint(address _to, uint _shares)`**: Mints `_shares` to `_to`, increasing the total supply and the user’s balance.
- **`_burn(address _from, uint _shares)`**: Burns `_shares` from `_from`, reducing the total supply and the user’s balance.

### Example Usage

```solidity
Vault vault = new Vault(address(token));
token.approve(address(vault), 1000);
vault.deposit(1000); // User deposits 1000 tokens and receives shares
vault.withdraw(500); // User withdraws tokens by redeeming 500 shares
