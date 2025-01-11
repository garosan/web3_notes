# ERC20 Tokens

ERC20 tokens typically have a name and a symbol. For example, ApeCoin has the name "ApeCoin" but the symbol "APE". The name of the token generally doesn't change, so we'll set it in the constructor and not provide any functions to change it later. We'll make these variables public so that anyone can check the name and symbol of the contract.

We also need to store everyone's balances.

Code so far:

```solidity
contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
    }
}
```

We say `balanceOf` because that is part of the ERC20 [specification](https://eips.ethereum.org/EIPS/eip-20). ERC20 as a specification means that people can call the function `balanceOf` on your contract, supply an address, and get how many tokens that address owns.

Everyone's balance is zero right now, so we need a way to bring tokens into existence. We'll allow a special address, the person who deployed the contract, to create tokens at will:

```solidity
contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        balanceOf[owner] += amount;
    }
}
```

It is general practice that the function mint() takes to and amount as the parameter argument. It allows for the contract deployer to mint tokens to other accounts. **For the sake of simplicity in this article, the function `mint()` only allows the deployer of the mint tokens into his account.**

To keep track of how many tokens there are in existence, the ERC20 specification requires a public function or variable called totalSupply that tells us how many tokens have been created:

```solidity
contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;

    uint256 public totalSupply;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;

    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }
}
```

### Decimals

Remember this is the largest number a `uint256` can represent:

`115792089237316195423570985008687907853269984665640564039457584007913129639935`

To be able to describe "decimals", we say the 18 zeros to the right are the fractional part of the coin:

`10000000000000000000000000000000000000000000000000000000000.000000000000000000`

Thus, if our ERC20 has 18 decimals, we can have at most `10000000000000000000000000000000000000000000000000000000000` full coins.

That's 10 octodecillion coins or 1 quadrillion x 1 quadrillion x 1 quadrillion x 1 trillion.

We shouldn't need more than this for our applications.

18 decimal places is pretty standard, but some coins use 6 decimal places.

As a sidenote:

- Bitcoin (BTC) has 8 decimal places. The smallest unit, called a _Satoshi_, is 0.00000001 BTC.
- Ethereum (ETH) has 18 decimal places. The smallest unit, called a _Wei_, is 0.000000000000000001 ETH.
- Solana (SOL) has 9 decimal places. The smallest unit, called a _Lamport_, is 0.000000001 SOL.

So with these additions, our token should look like this now:

```solidity
contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    constructor(string memory _name, string memory _symbol, uint8 decimals) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }
}
```

If you are paying attention, I did throw a curve ball at you here. The number type for the decimals variable is `uint8`, not `uint256`. uint8 can only represent numbers up to 255. However, a `uint256` has 77 zeros. Thus, it isn't possible to have more than 77 decimal places if you want to have one whole coin. So the standard dictates we use a `uint8` since the number of decimals can never be very large.

### Transfer

Let's go back to our transfer function to explain one thing:

```solidity
function transfer(address to, uint256 amount) public {
    require(balanceOf[msg.sender] >= amount, "you aint rich enough");
    require(to != address(0), "cannot send to address(0)");
    balanceOf[msg.sender] -= amount;
    balanceOf[to] += amount;
}
```

Take a look at this line of code:

`require(to != address(0), “cannot send to address(0))`

What is this doing? Well, nobody "owns" the zero address, so tokens sent there are un-spendable. By convention, sending a token to the zero address should reduce the totalSupply so we want to have a separate function for that.

### Allowance

Allowance enables an address to spend someone else's tokens, up to a limit that they specify.

Why would you allow someone to spend tokens for you? This is a very long story, but to summarize, think about how you would “know” someone transferred you ERC20 tokens. All that happens is a function gets executed and a mapping changed values. You didn't “receive” the tokens, they just became associated with your address.

The important thing here is that the established pattern for smart contracts to be recipients of transfers is to allow the smart contract to have a certain allowance, then tell that smart contract to withdraw the balance from your account.

When you want to transfer tokens to a smart contract, the typical method is to first approve the smart contract to withdraw a certain amount of tokens from your account. Then, you instruct the smart contract to withdraw the approved amount of tokens from your account. This is a common pattern used in smart contracts to enable token transfers to the contract.

Let's add the tracker for allowance, and a way to give allowance to another user.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256)) public allowance;
    // just added	address public owner;


    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }

    // just added
    function approve(address spender, uint256 amount) public {
        allowance[msg.sender][spender] = amount;
    }
}
```

In the line `allowance[msg.sender][spender] = amount;`, spender refers to the address of the account that is being granted the allowance by the `msg.sender`. The `msg.sender` is giving permission to the spender to spend a certain amount of tokens from their account.

Therefore, msg.sender is the owner of the tokens and spender is someone who has been approved by the owner to spend a certain amount of tokens on their behalf.

Ah, but we don't have a way to actually use the allowance given, it just sits there! That's what transferFrom is for.

### `transferFrom`

Newest code:

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;
    uint256 public totalSupply;

    // owner -> spender -> allowance
        // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256)) public allowance;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }

    function approve(address spender, uint256 amount) public {
        allowance[msg.sender][spender] = amount;
    }

    // just added
    function transferFrom(address from, address to, uint256 amount) public {
        require(balanceOf[from] >= amount, "not enough money");

        if (msg.sender != from) {
            require(allowance[from][msg.sender] >= amount, "not enough allowance");

            allowance[from][msg.sender] -= amount;
        }

        balanceOf[from] -= amount;
        balanceOf[to] += amount;
    }
}
```

First, it is possible for the owner of the coin to call `transferFrom`. In that case, allowance is meaningless, so we don't bother checking the allowance mapping, and update the balances accordingly.

Otherwise, we check to see the spender has been given enough allowance, then subtract the amount they are spending. If we didn't subtract their spending, we would have unlimited spending power.

There is one more cleanup to do. If we read the original specification for EIP 20 it says that `approve`, `transfer`, and `transferFrom` must return true after they succeed. So let's add that.

Newest code:

```solidity
contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256))
        public allowance;

    constructor(
        string memory _name,
        string memory _symbol
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(
        address to,
        uint256 amount
    )
        public {
            require(msg.sender == owner,
                "only owner can create tokens");
            totalSupply += amount;
            balanceOf[owner] += amount;
    }

    function transfer(
        address to,
        uint256 amount
    )
        public
        returns (bool) {
            require(balanceOf[msg.sender] >= amount,
                "you aint rich enough");
            require(to != address(0),
                "cannot send to address(0)");
            balanceOf[msg.sender] -= amount;
            balanceOf[to] += amount;

            return true;
    }

    function approve(
        address spender,
        uint256 amount
    )
        public
        returns (bool) {
            allowance[msg.sender][spender] = amount;

            return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    )
        public
        returns (bool) {
            require(balanceOf[from] >= amount,
                "not enough money");
            require(to != address(0),
                "cannot send to address(0)");

            if (msg.sender != from) {
                require(allowance[from][msg.sender] >= amount,
                    "not enough allowance");

                allowance[from][msg.sender] -= amount;
            }

            balanceOf[from] -= amount;
            balanceOf[to] += amount;

            return true;
    }
}
```

At the risk of throwing too much information at you, there is a cleanup to this code we can do. Note that transferFrom and transfer have duplicate code in them. What can we do about that? We could factor out the balance update code into a separate function, but we need to make sure that function isn't public or someone can steal coins!

```

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256))
        public allowance;

    constructor(
        string memory _name,
        string memory _symbol
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(
        address to,
        uint256 amount
    )
        public {
            require(msg.sender == owner,
                "only owner can create tokens");
            totalSupply += amount;
            balanceOf[owner] += amount;
    }

    function transfer(
        address to,
        uint256 amount
    )
        public
        returns (bool) {
            return helperTransfer(msg.sender, to, amount);
    }

    function approve(
        address spender,
        uint256 amount
    )
        public
        returns (bool) {
            allowance[msg.sender][spender] = amount;

            return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    )
        public
        returns (bool) {
            if (msg.sender != from) {
                require(allowance[from][msg.sender] >= amount,
                    "not enough allowance");

                allowance[from][msg.sender] -= amount;
            }

            return helperTransfer(from, to, amount);
    }

    function helperTransfer(
        address from,
        address to,
        uint256 amount
    )
        internal
        returns (bool) {
            require(balanceOf[from] >= amount,
                "not enough money");
            require(to != address(0),
                "cannot send to address(0)");
            balanceOf[from] -= amount;
            balanceOf[to] += amount;

            return true;
    }
}
```

[Original Article](https://www.rareskills.io/learn-solidity/erc20-tuotrial)

### Further reading

- [EIPS Ethereum](https://eips.ethereum.org/)
- [EIPS ERC-20](https://eips.ethereum.org/EIPS/eip-20)
- [EIPS vs ERC](https://www.coinbase.com/learn/your-crypto/what-is-an-ethereum-improvement-proposal-eip)
