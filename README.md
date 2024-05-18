# DegenToken Smart Contract

## Overview

The DegenToken smart contract is an ERC20-based token deployed on the Ethereum blockchain. It allows the owner to mint new tokens and provides functionalities for transferring, redeeming, and burning tokens. The contract also includes a shop with items that can be redeemed using tokens.

## Features

1. **Minting New Tokens**: The owner can mint new tokens and distribute them to specified addresses.
2. **Transferring Tokens**: Users can transfer tokens to other addresses.
3. **Redeeming Tokens**: Users can redeem tokens for items listed in the shop.
4. **Burning Tokens**: Users can burn their tokens to remove them from circulation.
5. **Checking Token Balance**: Users can check their token balance.

## Contract Details

### Variables

- `mapping(uint256 => uint256) public productpricesList`: Stores the prices of items available for redemption in the shop.

### Constructor

```solidity
constructor(address initialOwner) ERC20("DegenToken", "DGN") Ownable(initialOwner) {
    productpricesList[1] = 50;
    productpricesList[2] = 100;
    productpricesList[3] = 200;
    productpricesList[4] = 10;
}
```

The constructor initializes the token with the name "DegenToken" and symbol "DGN". It also sets the initial prices for shop items.

### Functions

- `mintCoin(address account, uint256 amount) public onlyOwner`: Allows the owner to mint new tokens to a specified address.
  
- `transferCoin(address recipient, uint256 amount) public virtual returns (bool)`: Allows a user to transfer tokens to another address.

- `listShopItems() external pure returns (string memory)`: Returns a list of items available in the shop along with their prices.

- `redeemCoin(uint256 _item) public`: Allows a user to redeem tokens for an item. The function checks if the item is available and if the user has sufficient balance.

- `burnCoin(uint256 amount) public`: Allows a user to burn a specified amount of their tokens.

- `getBalance() external view returns (uint256)`: Returns the balance of the caller.

## Usage

### Deploying the Contract

To deploy the contract, you need to provide the address of the initial owner. This owner will have the ability to mint new tokens.

```solidity
constructor(address initialOwner)
```

### Minting Tokens

Only the owner can mint new tokens. The minted tokens can be distributed to any address.

```solidity
mintCoin(address account, uint256 amount)
```

### Transferring Tokens

Users can transfer tokens to other addresses using the `transferCoin` function.

```solidity
transferCoin(address recipient, uint256 amount)
```

### Redeeming Tokens

Users can redeem their tokens for items listed in the shop. The `redeemCoin` function checks if the item is available and if the user has enough balance to redeem it.

```solidity
redeemCoin(uint256 _item)
```

### Burning Tokens

Users can burn their tokens to remove them from circulation.

```solidity
burnCoin(uint256 amount)
```

### Checking Balance

Users can check their token balance using the `getBalance` function.

```solidity
getBalance()
```

## Shop Items

The shop contains the following items:

1. Rare Sword - 50 DGN
2. Legendary Armor - 100 DGN
3. Epic Potion - 200 DGN
4. Common Scroll - 10 DGN

## Contributor

Immanuel George V. Pollente
