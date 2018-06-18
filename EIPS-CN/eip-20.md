---
eip: 20
title: ERC-20 代币标准
author: Fabian Vogelsteller <fabian@ethereum.org>, Vitalik Buterin <vitalik.buterin@ethereum.org>
translator: BoB Jiang <bob@hiblock.net>
type: Standards Track
category: ERC
status: Final
created: 2015-11-19
---

[英文链接](../EIPS/eip-20.md)

## 简单总结

代币的标准接口。


## 摘要

下面的标准允许在智能合约中代币的标准API的实现。
该标准提供了转账代币的基本功能，并允许批准代币，以便其他链上第三方可以使用这些代币。


## 动机

标准接口允许以太坊上的任一代币可以被其他应用程序重用：从钱包转到去中心化的交易所。


## 规范

## 代币
### 方法

**注意**: 调用者必须处理 `returns (bool success)` 返回的 `false` 。调用者一定不能假设从不返回 `false` ！


#### name

返回代币的名字 - 比如 `"MyToken"` 。Returns the name of the token - e.g. `"MyToken"`.

可选的 - 该方法可以用来改善可用性，
但接口及其他合约一定不能期望这些值存在。（译者注：即不能假设 name 一定可以返回代币名字）

``` js
function name() view returns (string name)
```


#### symbol

返回代币的标识符。 如 “HIX”。

可选的 - 该方法可以用来改善可用性，
但接口及其他合约一定不能期望这些值存在。（译者注：即不能假设 name 一定可以返回代币名字）


``` js
function symbol() view returns (string symbol)
```



#### decimals

返回代币使用的小数点位数 - 如 `8` ，意思是代币数量除以 `100000000` 以得到代表用户的最小单位。

可选的 - 该方法可以用来改善可用性，
但接口及其他合约一定不能期望这些值存在。（译者注：即不能假设 name 一定可以返回代币名字）

``` js
function decimals() view returns (uint8 decimals)
```


#### totalSupply

返回全部的代币供应量。Returns the total token supply.

``` js
function totalSupply() view returns (uint256 totalSupply)
```



#### balanceOf

返回 `_owner` 地址的账户余额。

``` js
function balanceOf(address _owner) view returns (uint256 balance)
```



#### transfer

转账 `_value` 数量的代币给地址 `_to` ， 且一定会触发 `Transfer` 事件。
如果 `_from` 账户余额不足，则该方法应该 `throw` 。

*注意* 值为0的转账必须当做正常转账处理且触发 `Transfer` 事件。

``` js
function transfer(address _to, uint256 _value) returns (bool success)
```



#### transferFrom

从 `_from` 地址转账 `_value` 给地址 `_to` ，且必须触发 `Transfer` 事件。

`transferFrom` 方法用于取款工作流，允许合约代表你来转账代币。
比如这可以用于允许合约代币你来转账代币，或以子货币来收取费用。
如果 `_from` 账户没有有意的通过某种机制授权消息的发送者，则该方法应该 `throw` 。

*注意* 值为0的转账必须当做正常转账处理且触发 `Transfer` 事件。

``` js
function transferFrom(address _from, address _to, uint256 _value) returns (bool success)
```



#### approve

允许 `_spender` 从你的账户多次取款，最大额度为 `_value` 。如果该方法再次调用，会用 `_value` 重新当前的额度。

**注意**：为了防止攻击向量，如这个[这里所述](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/) 以及 [这里讨论](https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729)，客户应该确保创建用户接口，为相同花费者设置其他值的最大额度前，首先设置当前额度为 `0` 。
尽管合约本身不应该强制如此，然而这是为了允许向后兼容之前部署的合同。

``` js
function approve(address _spender, uint256 _value) returns (bool success)
```


#### allowance

返回 `_spender` 还被允许从 `_owner` 提款的额度。

``` js
function allowance(address _owner, address _spender) view returns (uint256 remaining)
```



### Events


#### Transfer

代币转账时必须触发，包括价值为0的转账。

创建新代币的合约，在代币创建时应该触发 Transfer 事件，并将 `_from` 地址设为 `0x0` 。

``` js
event Transfer(address indexed _from, address indexed _to, uint256 _value)
```



#### Approval

任何成功的调用 `approve(address _spender, uint256 _value)` 都必须触发该事件。

``` js
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```



## 实现

已经有许多 ERC20 兼容的代币部署在以太坊网络上。
不同的团队编写了不同的实现，这些团队有不同的权衡：从节约 gas 到提高安全性。

#### 可用的示例实现如下：
- https://github.com/OpenZeppelin/zeppelin-solidity/blob/master/contracts/token/ERC20/StandardToken.sol
- https://github.com/ConsenSys/Tokens/blob/master/contracts/eip20/EIP20.sol

#### 再次调用"approve"前增加强制设为0的实现：
- https://github.com/Giveth/minime/blob/master/contracts/MiniMeToken.sol



## 历史

与该标准有关的历史链接：

- Vitalik Buterin的原始提议: https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs/499c882f3ec123537fc2fccd57eaa29e6032fe4a
- Reddit discussion: https://www.reddit.com/r/ethereum/comments/3n8fkn/lets_talk_about_the_coin_standard/
- Original Issue #20: https://github.com/ethereum/EIPs/issues/20



## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
