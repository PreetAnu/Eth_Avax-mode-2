# Eth_Avax-mode-2
The contract simulates a decentralized ATM service on Ethereum, allowing users to manage their digital currency transactions and ownership securely⚒️
## Decentralized ATM Service
A Decentralized ATM Service for managing ETH deposits, withdrawals, and ownership transfers.
## Description
This project simulates a decentralized Automated Teller Machine (ATM) service on Ethereum, allowing users to manage their digital currency transactions and ownership securely through a smart contract and a web interface.
## Getting Started
### Installing
* Clone the repository from GitHub.
* Navigate to the project directory.
* Install the necessary dependencies: npm i.
### Executing program
* Open three terminals in your project directory.
* In the first terminal, start the Hardhat node:npx hardhat node.
* In the second terminal, deploy the smart contract: npx hardhat run --network localhost scripts/deploy.js.
* In the third terminal, start the front-end server: npm run dev.
Open your browser and navigate to http://localhost:3000/ to interact with the application.
### Contract Details 🔗
The smart contract used in this project is named Assessment and is located in the contracts/Assessment.sol file.
```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;

//import "hardhat/console.sol";

contract Assessment {
    address public owner;
    uint256 public balance;

    event Deposit(uint256 amount);
    event Withdraw(uint256 amount);
    event OwnershipTransferred(address indexed oldOwner, address indexed newOwner);
    event OwnershipRenounced(address indexed oldOwner);

    constructor(uint initBalance) payable {
        owner = payable(msg.sender);
        balance = initBalance;
    }

    // make sure this is the owner
    modifier onlyOwner() {
        require(msg.sender == owner, "You are not the owner of this account");
        _;
    }

    function getBalance() public view returns(uint256){
        return balance;
    }

    function deposit(uint256 _amount) public payable {
        uint _previousBalance = balance;

        // perform transaction
        balance += _amount;
        // assert transaction completed successfully
        assert(balance == _previousBalance + _amount);
        // emit the event
        emit Deposit(_amount);
    }

    // custom error
    error InsufficientBalance(uint256 balance, uint256 withdrawAmount);

    function withdraw(uint256 _withdrawAmount) public  payable {

        uint _previousBalance = balance;
        if (balance < _withdrawAmount) {
            revert InsufficientBalance({
                balance: balance,
                withdrawAmount: _withdrawAmount
            });
        }

        // withdraw the given amount
        balance -= _withdrawAmount;

        // assert the balance is correct
        assert(balance == (_previousBalance - _withdrawAmount));

        // emit the event
        emit Withdraw(_withdrawAmount);
    }
    
    function transferOwnership(address newOwner) public virtual onlyOwner {
    require(newOwner != address(0), "Ownable: new owner is the zero address");
    address oldOwner = owner;
    owner = payable(newOwner);
    emit OwnershipTransferred(oldOwner, newOwner);
    }


    function renounceOwnership() public onlyOwner {
        address oldOwner = owner;
        owner = payable(address(0));
        emit OwnershipRenounced(oldOwner);
    }
}

```
It allows users to deposit and withdraw funds, as well as transfer and renounce ownership. The contract emits events for deposits, withdrawals, and ownership changes.
### Scripts 
The deploy.js script, located in the scripts folder, is used to deploy the Assessment contract. It uses Hardhat's ethers library to interact with the blockchain. This script is executed with the npx hardhat run command.
## Help
If you encounter any issues, ensure that:
* MetaMask is installed and configured correctly.
* You have enough funds in your MetaMask wallet on the local network.
## Authors
Name    : Anupreet Kaur
email-id: anupreetk159@gmail.com
## License
This project is licensed under the MIT License - see the LICENSE.md file for details.

