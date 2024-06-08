// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleBank {
    mapping(address => uint256) private balances;

    // Event to log the deposit operation
    event Deposit(address indexed account, uint256 amount);

    // Event to log the withdrawal operation
    event Withdrawal(address indexed account, uint256 amount);

    // Function to deposit Ether into the contract
    function deposit() public payable {
        // Using require to ensure a positive deposit amount
        require(msg.value > 0, "Deposit amount must be greater than zero.");

        // Update the balance
        balances[msg.sender] += msg.value;

        // Emit the Deposit event
        emit Deposit(msg.sender, msg.value);
    }

    // Function to withdraw Ether from the contract
    function withdraw(uint256 _amount) public {
        // Using require to ensure the sender has enough balance
        require(balances[msg.sender] >= _amount, "Insufficient balance.");

        // Update the balance
        balances[msg.sender] -= _amount;

        // Transfer the Ether and check for successful transfer using assert
        (bool success, ) = msg.sender.call{value: _amount}("");
        assert(success);

        // Emit the Withdrawal event
        emit Withdrawal(msg.sender, _amount);
    }

    // Function to get the balance of the caller
    function getBalance() public view returns (uint256) {
        return balances[msg.sender];
    }

    // Function to demonstrate the use of revert
    function demoRevert(uint256 _value) public pure {
        // If the value is less than 10, revert the transaction
        if (_value < 10) {
            revert("Value must be at least 10.");
        }
    }
}
