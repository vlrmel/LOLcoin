# LOLcoin
pragma solidity ^0.8.0;

contract LOLcoin {
    mapping (address => uint) public balanceOf;
    mapping (address => mapping (address => uint)) public allowance;
    
    uint public totalSupply;
    string public name = "LOLcoin";
    string public symbol = "LOL";
    uint8 public decimals = 18;
    
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event CreateMeme(address indexed creator, string memeText);
    event SpreadMeme(address indexed spreader, address indexed creator, string memeText);

    constructor(uint initialSupply) {
        totalSupply = initialSupply * 10 ** decimals;
        balanceOf[msg.sender] = totalSupply;
    }
    
    function transfer(address to, uint value) public returns (bool success) {
        require(balanceOf[msg.sender] >= value, "Not enough balance");
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }
    
    function approve(address spender, uint value) public returns (bool success) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }
    
    function transferFrom(address from, address to, uint value) public returns (bool success) {
        require(balanceOf[from] >= value, "Not enough balance");
        require(allowance[from][msg.sender] >= value, "Not enough allowance");
        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }
    
    function createMeme(string memory memeText) public {
        balanceOf[msg.sender] += 1000;
        emit CreateMeme(msg.sender, memeText);
    }
    
    function spreadMeme(address creator, string memory memeText) public {
        balanceOf[msg.sender] += 500;
        balanceOf[creator] += 500;
        emit SpreadMeme(msg.sender, creator, memeText);
    }
}
