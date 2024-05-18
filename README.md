# KTokens Smart Contract

## Overview

KTokens is a custom ERC20 token with additional functionalities including staking, item redemption, and administrative controls for minting, pausing, and emergency withdrawal. This contract is built using Solidity and leverages OpenZeppelin libraries for secure and standardized implementations.

## Features

- **Minting**: The owner can mint new tokens.
- **Transferring**: Standard ERC20 transfer functionality with additional checks.
- **Redeeming Items**: Users can redeem tokens for in-game items.
- **Burning**: Users can burn their tokens.
- **Staking**: Users can stake their tokens to earn rewards.
- **Pausing**: The contract can be paused and unpaused by the owner.
- **Emergency Withdrawal**: The owner can withdraw all tokens from the contract in case of emergency.
- **Item Management**: The owner can add, update, and remove items from the shop.

## Contract Details

### Constructor

The constructor initializes the contract by minting an initial supply of tokens to the deployer and setting up the reward rate and initial shop items.

```solidity
constructor(address initialOwner) ERC20("Degen", "DGN") Ownable(initialOwner) {
    _mint(msg.sender, 10 * 10 ** decimals());
    rewardRate = 100; // Example reward rate
    nextItemId = 1;
    addItem("Rare Sword", 50);
    addItem("Legendary Armor", 100);
    addItem("Epic Potion", 200);
    addItem("Common Scroll", 10);
}
```

### Minting

Allows the owner to mint new tokens.

```solidity
function mint(address account, uint256 amount) public onlyOwner whenNotPaused {
    _mint(account, amount);
}
```

### Transferring

Standard ERC20 transfer with an additional check for paused state.

```solidity
function transfer(address recipient, uint256 amount) public virtual override onlyWhenNotPaused returns (bool) {
    require(amount <= balanceOf(msg.sender), "Insufficient balance");
    _transfer(msg.sender, recipient, amount);
    return true;
}
```

### Redeeming Items

Users can redeem their tokens for items listed in the shop.

```solidity
function redeem(uint256 itemId) public nonReentrant whenNotPaused {
    Item memory item = shopItems[itemId];
    require(item.price > 0, "Item does not exist");
    require(balanceOf(msg.sender) >= item.price, "Insufficient balance to redeem item");
    _burn(msg.sender, item.price);
    // Logic to transfer item to user can be added here
}
```

### Burning

Users can burn their tokens.

```solidity
function burn(uint256 amount) public whenNotPaused {
    require(amount <= balanceOf(msg.sender), "Insufficient balance");
    _burn(msg.sender, amount);
}
```

### Staking

Users can stake their tokens to earn rewards.

```solidity
function stake(uint256 amount) public whenNotPaused {
    require(amount <= balanceOf(msg.sender), "Insufficient balance to stake");
    _transfer(msg.sender, address(this), amount);
    stakedBalances[msg.sender] = stakedBalances[msg.sender].add(amount);
    stakingRewards[msg.sender] = stakingRewards[msg.sender].add(amount.mul(rewardRate).div(10000));
}
```

### Unstaking

Users can unstake their tokens.

```solidity
function unstake(uint256 amount) public nonReentrant whenNotPaused {
    require(stakedBalances[msg.sender] >= amount, "Insufficient staked balance");
    stakedBalances[msg.sender] = stakedBalances[msg.sender].sub(amount);
    _transfer(address(this), msg.sender, amount);
}
```

### Claiming Rewards

Users can claim their staking rewards.

```solidity
function claimRewards() public nonReentrant whenNotPaused {
    uint256 reward = stakingRewards[msg.sender];
    require(reward > 0, "No rewards to claim");
    stakingRewards[msg.sender] = 0;
    _mint(msg.sender, reward);
}
```

### Pausing

The owner can pause and unpause the contract.

```solidity
function pause() public onlyOwner {
    _pause();
}

function unpause() public onlyOwner {
    _unpause();
}
```

### Emergency Withdrawal

The owner can withdraw all tokens from the contract in case of an emergency.

```solidity
function emergencyWithdraw() public onlyOwner {
    uint256 contractBalance = balanceOf(address(this));
    _transfer(address(this), owner(), contractBalance);
}
```

### Item Management

The owner can add, update, and remove items from the shop.

```solidity
function addItem(string memory name, uint256 price) public onlyOwner {
    shopItems[nextItemId] = Item(name, price);
    nextItemId++;
}

function updateItem(uint256 itemId, string memory name, uint256 price) public onlyOwner {
    require(shopItems[itemId].price > 0, "Item does not exist");
    shopItems[itemId] = Item(name, price);
}

function removeItem(uint256 itemId) public onlyOwner {
    require(shopItems[itemId].price > 0, "Item does not exist");
    delete shopItems[itemId];
}
```

### Get Balance

Allows users to get their token balance.

```solidity
function getBalance() external view returns (uint256) {
    return balanceOf(msg.sender);
}
```

## How to Use

1. **Deploy the Contract**: Deploy the contract to a blockchain network with the initial owner address.
2. **Minting Tokens**: The owner can mint new tokens using the `mint` function.
3. **Transferring Tokens**: Users can transfer tokens to other addresses using the `transfer` function.
4. **Staking and Unstaking**: Users can stake their tokens to earn rewards and unstake them when needed.
5. **Claiming Rewards**: Users can claim their staking rewards.
6. **Redeeming Items**: Users can redeem their tokens for items listed in the shop.
7. **Pausing the Contract**: The owner can pause and unpause the contract.
8. **Emergency Withdrawal**: The owner can perform an emergency withdrawal of all tokens from the contract.

## Security Considerations

- The contract includes reentrancy guards to prevent reentrancy attacks.
- SafeMath is used to prevent overflow and underflow issues.
- The contract can be paused in case of emergencies.
