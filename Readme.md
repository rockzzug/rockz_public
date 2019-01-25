****#Rockz Token
##Requirements
Full ERC223 specification support. 

Must allow emission(mint) and reverse emission(burn) of tokens and fire corresponding events.

Will have following roles:
-	owner: can not be changed once set, assigned on smart contract deployment, is an address of account deployed the contract. Can set minter account. Can not mint tokens.
-	minter: can be multiple. Can mint tokens to his account. Can pause transfers.
-	user: ERC223, ERC20 methods only.

##Methods
NOTE: Callers MUST handle false from returns (bool success). Callers MUST NOT assume that false is never returned!
###name (ERC20)
Returns the name of the token - e.g. "Rockz".
```solidity
function name() view returns (string name)
```
###symbol (ERC20)
Returns the symbol of the token. E.g. “RKZ”
```solidity
function symbol() view returns (string symbol)
```
###decimals (ERC20)
Returns the number of decimals the token uses – e.g. 2. In our case as token represents fiat currency number of decimals should be 2.
```solidity
function decimals() view returns (uint8 decimals)
```
###totalSupply (ERC20)
Returns the total token supply.
```solidity
function totalSupply() view returns (uint256 totalSupply)
```
###balanceOf (ERC20)
Returns the account balance of another account with address _owner.
```solidity
function balanceOf(address _owner) view returns (uint256 balance)
```
###transfer (ERC20)
Transfers _value amount of tokens to address _to, and MUST fire the Transfer event. The function SHOULD throw if the _from account balance does not have enough tokens to spend. MUST check if _to is a contract address and throw if it is.
Note Transfers of 0 values MUST be treated as normal transfers and fire the Transfer event.
```solidity
function transfer(address _to, uint256 _value) returns (bool success)
```
###transfer (ERC223)
This function must transfer tokens and invoke the function tokenFallback (address, uint256, bytes) in _to, if _to is a contract. If the tokenFallback function is not implemented in _to (receiver contract), then the transaction must fail and the transfer of tokens should not occur.
If _to is an externally owned address, then the transaction must be sent without trying to execute tokenFallback in _to.
_data can be attached to this token transaction and it will stay in blockchain forever (requires more gas). _data can be empty.
NOTE: The recommended way to check whether the _to is a contract or an address is to assemble the code of _to. If there is no code in _to, then this is an externally owned address, otherwise it's a contract.
```solidity
function transfer(address _to, uint256 _value, bytes _data) returns (bool success)
```
###transferFrom (ERC20)
Transfers _value amount of tokens from address _from to address _to, and MUST fire the Transfer event.
The transferFrom method is used for a withdraw workflow, allowing contracts to transfer tokens on your behalf. This can be used for example to allow a contract to transfer tokens on your behalf and/or to charge fees in sub-currencies. The function SHOULD throw unless the _from account has deliberately authorized the sender of the message via some mechanism.
Note Transfers of 0 values MUST be treated as normal transfers and fire the Transfer event.
```solidity
function transferFrom(address _from, address _to, uint256 _value) returns (bool success)
```
###approve (ERC20)
Allows _spender to withdraw from your account multiple times, up to the _value amount. If this function is called again it overwrites the current allowance with _value.
NOTE: To prevent attack vectors like the one described here and discussed here, clients SHOULD make sure to create user interfaces in such a way that they set the allowance first to 0 before setting it to another value for the same spender. THOUGH The contract itself shouldn't enforce it, to allow backwards compatibility with contracts deployed before
```solidity
function approve(address _spender, uint256 _value) returns (bool success)
```
###allowance (ERC20)
Returns the amount which _spender is still allowed to withdraw from _owner.
```solidity
function allowance(address _owner, address _spender) view returns (uint256 remaining)
```
###burn
Burn specific amounts of tokens for _who address and fire Burn event.
function burn(uint256 _value, bytes _data) returns (bool success)
###mint
Mint _value of tokens and assign to minter’s balance, and fire Mint event
```solidity
function mint(uint256 _value, bytes _data) returns (bool success)
```

##Events
###Transfer (ERC20)
MUST trigger when tokens are transferred, including zero value transfers.
A token contract which creates new tokens SHOULD trigger a Transfer event with the _from address set to 0x0 when tokens are created.
```solidity
event Transfer(address indexed _from, address indexed _to, uint256 _value)
```
###Approval (ERC20)
MUST trigger on any successful call to approve(address _spender, uint256 _value).
```solidity
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```
###Burn
MUST trigger on any successful call to burn(address _who, uint256 _value).
```solidity
event Burn(address indexed _who, uint256 _value, bytes _data)
```
###Mint
MUST trigger on any successful call to mint(uint256 _value).
```solidity
event Mint(address indexed _minter, address indexed _to, uint256 _value, bytes _data)
```
