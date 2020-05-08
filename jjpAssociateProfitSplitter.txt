pragma solidity ^0.6.0;

import "github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/math/SafeMath.sol";

contract AssociateProfitSplitter {

    using SafeMath for uint;

    address payable public employee_one;
    address payable public employee_two;
    address payable public employee_three;
    
    mapping(address => uint) balances;

    constructor(address payable _one, address payable _two, address payable _three) public {
        employee_one = _one;
        employee_two = _two;
        employee_three = _three;
    }

    function balance() public view returns(uint) {
        return msg.sender.balance;
    }
   
   function deposit(uint amount) public payable {
       amount = msg.value.div(3);
       balances[msg.sender] = balances[msg.sender].sub(amount * 3);
       balances[employee_one] = balances[employee_one].add(amount);
       balances[employee_two] = balances[employee_two].add(amount);
       balances[employee_three] = balances[employee_three].add(amount);
       
       return msg.sender.transfer(msg.value - amount * 3); 
    }
    
    fallback() external payable {}

}
