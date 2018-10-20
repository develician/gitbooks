> ### interface

1. 상속: is
2. 전에 생성했던 token 의 주소로 재생성

`SampleToken`

```solidity
pragma solidity ^0.4.24;

contract ERC20TokenComplete {

    string public constant name = "test";
    string public constant symbol = "t";
    uint8 public constant decimals = 0;

    uint256 public totalSupply ;

    mapping(address => uint256) public balanceOf;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Burn(address indexed from, uint256 value);

    address owner; // 토큰 발행자

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    constructor (
        uint256 _totalSupply
    ) public {
        owner = msg.sender;
        totalSupply = _totalSupply * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        Transfer(address(this), msg.sender, totalSupply);
        assert(true);
    }

    function transfer(address to, uint amount)  public returns(bool) {
        require(balanceOf[msg.sender] >= amount); // 보내는 사람의 보유량 확인
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        Transfer(msg.sender, to, amount);
    }

    function burn(uint amount) onlyOwner public {
        require(totalSupply >= amount); // 전체 보유량 확인
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Burn(msg.sender, amount);
    }

    function addPublish(uint amount) onlyOwner public{
        totalSupply += amount * 10 ** uint(decimals); // totalSupply = totalSupply + amount * 10 ** uint(decimals);
        balanceOf[msg.sender] +=  amount * 10 ** uint(decimals);
    }
}
```

ICO contract

`SampleICOContract`

```solidity
interface token {
    function transfer(address to, uint amount) public returns(bool);
    function balanceOf(address addr) returns (uint);
}

contract ICO {
    address beneficiary;
    uint fundingGoal;
    uint decimals = 0;
    uint price;
    token tokenReward;
    address tokenAddress;
    address owner;

    constructor(
        address ifSuccessfulSendTo,
        uint fundingGoalInEthers,
        uint etherCostOfEachToken,
        address addressOfTokenUsedAsReward
    ) {
        beneficiary = ifSuccessfulSendTo;
        fundingGoal = fundingGoalInEthers * 10 ** uint256(decimals);
        price = (1 ether / etherCostOfEachToken);
        tokenReward = token(addressOfTokenUsedAsReward);
        tokenAddress = addressOfTokenUsedAsReward;
        owner = msg.sender;
    }

    function () payable {
        tokenReward.transfer(msg.sender, msg.value / price * 10 ** uint256(decimals));

    }

    function getBalanceOf(address addr) public view returns (uint) {
        return tokenReward.balanceOf(addr);
    }
}
```

1. Token Contract 생성
2. ICO Contract 생성
3. ICO Contract 에게 판매할 토큰 전송
4. ICO Contract 에게 사람들이 이더를 보내면 토큰을 준다.
5. 세일즈 종료
6. 이더 환급
   1. 먹튀 => scam
7. 토큰 환급

1-3 준비
4 진행
5-7 끝

ICO : 상점
Token: 은행
