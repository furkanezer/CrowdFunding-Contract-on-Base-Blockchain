# CrowdFunding-Contract-on-Base-Blockchain


<img src="https://i.imgur.com/60tdulP_d.webp?maxwidth=760&fidelity=grand)">


[CrowdFunding Smart Contract is here.](https://basescan.org/address/0x499994f51915532ce9de28ccd0ea55130a881449)

Contract : 0x499994F51915532ce9dE28ccd0ea55130A881449

Creator  : 0xdeD7d6c87C287808FAafE98B54027AA194E8A2eb

```shell


// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Crowdfunding {

    // Campaign Information
    string public name;
    string public description;
    uint256 public targetAmount;
    uint256 public deadline;

    // Contributors
    mapping(address => uint256) public contributions;
    address[] public contributorsList;

    // Distribution Information
    bool public fundingSuccessful;
    uint256 public totalRaisedAmount;

    // Events
    event Contributed(address contributor, uint256 amount);
    event FundingSuccessful(uint256 raisedAmount);
    event FundingUnsuccessful(uint256 raisedAmount);
    event FundsWithdrawn(address contributor, uint256 amount);

    constructor(
        string memory _name,
        string memory _description,
        uint256 _targetAmount,
        uint256 _deadline
    ) {
        name = _name;
        description = _description;
        targetAmount = _targetAmount;
        deadline = _deadline;
    }

    function contribute() external payable {
        require(block.timestamp < deadline, "Campaign deadline has passed!");

        contributions[msg.sender] += msg.value;
        contributorsList.push(msg.sender);
        totalRaisedAmount += msg.value;

        emit Contributed(msg.sender, msg.value);
    }

    function finalizeFunding() external {
        require(block.timestamp >= deadline, "Campaign deadline has not been reached!");

        if (totalRaisedAmount >= targetAmount) {
            fundingSuccessful = true;
            emit FundingSuccessful(totalRaisedAmount);

            // Perform distribution logic.
        } else {
            fundingSuccessful = false;
            emit FundingUnsuccessful(totalRaisedAmount);

            // Perform refund logic.
        }
    }

    // Withdraw funds
    function withdrawFunds() external {
        require(fundingSuccessful == false, "Funds cannot be withdrawn as funding was successful!");
        require(contributions[msg.sender] > 0, "You cannot withdraw funds as you have not contributed!");

        uint256 amountToWithdraw = contributions[msg.sender];
        contributions[msg.sender] = 0;

        // Perform refund logic.

        emit FundsWithdrawn(msg.sender, amountToWithdraw);
    }

    // Helper functions
    function timeRemaining() public view returns (uint256) {
        return deadline - block.timestamp;
    }

    function fundingPercentage() public view returns (uint256) {
        return (totalRaisedAmount * 100) / targetAmount;
    }
}

```
