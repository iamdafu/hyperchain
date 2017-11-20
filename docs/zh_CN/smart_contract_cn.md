# 智能合约


## 1. 智能合约简介

> 智能合约是指部署在区块链上的一段可以自动执行条款的计算机程序。智能合约能够根据外界输入信息自动执行预先定义好的协议并完成区块链内部相关状态的转移。

广泛意义上的智能合约还包括智能合约编程语言、编译器、虚拟机、事件、状态机、容错机制等。其中对智能合约影响较大的是智能合约编程语言以及其执行引擎。智能合约虚拟机一般为了安全起见是作为沙箱被分装起来，整个执行环境完全被隔离。虚拟机内部执行的智能合约不允许接触网络、文件系统、进程线程等系统资源。不同智能合约的安全性等级、表达的丰富性有所不同，Hyperchain系统自主研发智能合约的执行引擎HyperVM是一种通用智能合约引擎设计，允许多种不同智能合约引擎接入。目前的实现了兼容Ethereum的Solidity语言的HyperEVM和支持Java语言的智能合约引擎HyperJVM.


## 2. 智能合约引擎HyperVM

HyperVM 是Hyperchain自主研发的可插拔智能合约引擎通用框架，允许不同智能合约执行引擎接入。如下图所示是HyperVM的架构示意图，HyperVM的架构图提供了智能合约编译、执行等相关主要组件。其中，Compiler提供了智能合约编译相关功能，Interpreter和Executor则提供了智能合约解释以及执行相关功能，State组件赋予智能合约操作区块链账本的相关功能。Guard模块提供智能合约安全保障相关机制。

![hypervm-architecture](../../images/hypervm.png)

### 2.1 HyperEVM

为了最大程度利用开源社区在智能合约技术方面的研究和积累，提高智能合约的可重用性以及兼容性。HyperEVM的实现采用了完全兼容Ethereum的智能合约规范，使用Solidity作为智能合约开发语言，底层使用了优化了的Ethereum虚拟机EVM。
如下图所示为HyperEVM智能合约执行流程图:

![hyperevm-flow](../../images/hyperevm-flow.png)


HyperEVM执行一次交易之后会返回一个执行结果，系统将其保存在被称为交易回执的变量中，之后平台客户端可以根据本次的交易哈希进行交易结果的查询。HyperEVM的执行流程如下：





## 3. 智能合约使用

### 3.1 基于Solidity智能合约案例

### 编写合约

基于Solidity的智能合约同基于JS的程序类似，由一系列变量和相关函数组成。如下所示为模拟简单累加器功能的智能合约。我们以此为例简单介绍基于Solidity智能合约的基本组成部分。


```js
contract Accumulator{    
	uint32 sum = 0;   
	function increment(){
		sum = sum + 1;     
	}    

	function getSum() returns(uint32){
		return sum;    
	}   

	function add(uint32 num1,uint32 num2) {
		sum = sum+num1+num2;     
	}
}

```
Accumulator合约说明：

* 基于solidity的智能合约以关键字`contract`开头，类似Java等语言中的class关键字；
* 合约内部可以有变量和函数，上述sum 为uint32类型的简单变量，Solidity智能合约还支持map等集合类型；
* 合约允许定义执行函数以`function`关键字定义；

基于Solidity语言编写智能合约的详细规范参考[Solidiy官方网站](https://solidity.readthedocs.io/en/develop/)


### 编译合约

Hyperchain的智能合约的编译既可以采用Solidity官方编译器编译，也可以使用Hyperchain提供的智能合约部署JSON-RPC的接口进行编译（这种场景需要在安装Hyperchain的宿主机安装Solidity编译器sloc）。

调用Hyperchain编译Solidity智能合约命令如下：

```js
curl -X POST --data
'{
	"jsonrpc":"2.0",
	"namespace":"global",
	"method":"contract_compileContract",
	"params":["contract_code"],
	"id":1
}'
```

合约编译接口调用的返回如下：

```js
{

```
其中字段bin对应的内容为该合约的字节码表示，该bin内容供后续部署使用。

### 部署合约

Hyperchain部署Solidity命令如下：

```js
curl localhost:8081 --data '{"jsonrpc":"2.0", "namespace":"global",  "method":"contract_deployContract", "params":[{
```

部署合约返回如下，其中result字段内容为该合约在区块链中的地址，后期对该合约的调用需要指定该合约地址。

```js

{
```


### 调用合约

Hyperchain调用命令如下，其中payload为调用合约中函数以及其参数值的编码结果，to为所调用合约的地址。

```js
curl localhost:8081 --data

'{
	"jsonrpc":"2.0",
	"namespace":"global",
	"method": "contract_invokeContract",
	"params":[{
	"id":"1"
}'
```

合约调用会立即给客户端返回该交易的哈希值，后期可以根据该交易的哈希值查询具体交易的执行结果。

```js
{

```



合约操作其他方法以及上述方法中的参数详细说明参考：[Hyperchain API使用文档]()