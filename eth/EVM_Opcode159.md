原文链接：https://medium.com/@danielyamagata/understand-evm-opcodes-write-better-smart-contracts-e64f017b619



# Understand EVM Opcodes, Write Better Smart Contracts



Your good developer habits are leading you to write inefficient smart contracts. For typical programming languages, the only costs associated with state changes and computation are time and the electricity used by the hardware. However, for EVM-compatible languages, such as Solidity and Vyper, these actions explicitly cost *money*. This cost is in the form of the blockchain’s native currency (ETH for Etheruem, AVAX for Avalanche, etc.), which can be thought of as a commodity used to pay for these actions.

The cost for computation, state transitions, and storage is called *gas*. Gas is used to prioritize transactions, as a [Sybil resistance](https://en.wikipedia.org/wiki/Sybil_attack) mechanism, and to prevent attacks stemming from the [halting problem](https://en.wikipedia.org/wiki/Halting_problem).

*Feel free to read my article on* [*Solidity basics* ](https://medium.com/@danielyamagata/solidity-basics-your-first-smart-contract-f11f4f7853d0)*to learn more about gas*

These atypical costs lead to software design patterns that would seem both inefficient and strange in typical programming languages. To be able to recognize these patterns and grasp why they lead to cost efficiencies, you must first have a basic understanding of the Ethereum Virtual Machine, i.e. the EVM.

**What is the EVM?**

*If you are already familiar with the EVM, feel free to skip to the section,* ***What are EVM Opcodes?\***

A blockchain is a transaction-based [*state machine*](https://en.wikipedia.org/wiki/Finite-state_machine). Blockchains incrementally execute transactions, which morph into some new state. Therefore, each transaction on a blockchain is a *transition of state.*

Simple blockchains, like Bitcoin, natively only support simple transfers. In contrast, smart-contract compatible chains, like Ethereum, implement two types of accounts, externally owned accounts and contract accounts, in order to support complex logic.

Externally owned accounts are controlled by users via private keys and have no code associated with them, while contract accounts are solely controlled by their associated code. EVM code is stored as [bytecode](https://en.wikipedia.org/wiki/Bytecode) in a virtual [ROM](https://en.wikipedia.org/wiki/Read-only_memory).

The EVM handles the execution and processing of all transactions on the underlying blockchain. It is a stack machine in which each item on the stack is 256-bits or 32 bytes. The EVM is embedded within each Ethereum node and is responsible for executing the contract’s bytecode.

The EVM stores data in both storage and memory. *Storage* is used to store data permanently while *memory* is used to store data during function calls. You can also pass in function arguments as *calldata,* which act similar to allocating to memory except the data is non-modifiable.

*Learn more about Ethereum and the EVM in Preethi Kasireddy’s* [*article, “How does Ethereum work, anyway?”*](https://www.preethikasireddy.com/post/how-does-ethereum-work-anyway#:~:text=The Ethereum blockchain is essentially,transition to a new state.)

Smart contracts are written in higher-level languages, such as Solidity, Vyper, or Yul, and subsequently broken down into EVM bytecode via a compiler. However, there are times when it is more gas efficient to use bytecode directly in your code.

![1.png](https://img.learnblockchain.cn/attachments/2022/09/3yW6IizY6316af8248ebd.png)

[LooksRare’s TransferSelectorNFT smart contract](https://github.com/LooksRare/contracts-exchange-v1/blob/master/contracts/TransferSelectorNFT.sol)

EVM bytecode is written in hexadecimal. It is the language that the virtual machine is able to interpret. This is somewhat analogous to how CPUs can only interpret machine code.

![2.png](https://img.learnblockchain.cn/attachments/2022/09/gUZSV7fM6316af85c8841.png)

Example of Solidity Bytecode

**What are EVM Opcodes?**

All Ethereum bytecode can be broken down into a series of operands and opcodes. Opcodes are predefined instructions that the EVM interprets and is subsequently able to execute. For example, the ADD opcode is represented as 0x01 in EVM bytecode. It removes two elements from the stack and pushes the result.

The number of elements removed from and pushed onto the stack depends on the opcode. For example, there are thirty-two PUSH opcodes: PUSH1 through PUSH32. PUSH* adds a * byte item on the stack that ranges from 0 to 32 bytes in size. It does not remove any values from the stack and adds a single value. In contrast, the ADDMOD opcode represents the [modulo addition operation](https://libraryguides.centennialcollege.ca/c.php?g=717548&p=5121840#:~:text=Properties of addition in modular,%2B d ( mod N ) .) and removes three items from the stack and subsequently pushes the result. Notably, the PUSH opcodes are the only ones that come with operands.

![3.png](https://img.learnblockchain.cn/attachments/2022/09/cuWOtV4M6316af89b9b6d.png)

The Opcodes of the Prior Bytecode Example

Each opcode is one byte and has a differing cost. Depending on the opcode, these costs are either fixed or determined by a formula. For example, the ADD opcode costs 3 gas. In contrast, SSTORE, the opcode which saves data in storage, costs 20,000 gas when a storage value is set to a non-zero value from zero and costs 5000 gas when a storage variable’s value is set to zero or remains unchanged from zero.

*SSTORE’s cost actually varies further depending on if a value has been accessed or not. Full details of SSTORE’s and SLOAD’s costs can be found* [*here*](https://hackmd.io/@fvictorio/gas-costs-after-berlin)

**Why is understanding EVM Opcodes important?**

Understanding EVM opcodes is extremely important for minimizing gas consumption, and, in turn, reducing costs for your end user. Since the cost associated with EVM opcodes is arbitrary, different coding patterns that achieve the same result might lead to greatly higher costs. Knowing which opcodes are the most expensive will help you minimize and avoid their usage when unnecessary. View the [Ethereum documentation](https://ethereum.org/en/developers/docs/evm/opcodes/) for a full list of EVM opcodes and their associated gas costs.

![4.png](https://img.learnblockchain.cn/attachments/2022/09/p1uzciT06316af8d1666d.png)

Below are concrete examples of unintuitive design patterns stemming from the cost of EVM opcodes:

**Using Multiplication over Exponetentiation: MUL vs EXP**

The MUL opcode costs 5 gas and is used to perform multiplication. For example, the arithmetic behind 10 * 10 would cost 5 gas.

The EXP opcode is used to perform exponentiation, and its gas cost is determined by a formula: if the exponent is zero, the opcode costs 10 gas. However, if the exponent is greater than zero, it costs 10 gas plus 50 times the number of bytes in the exponent.

Since a byte is 8 bits, a single byte is used to represent values between 0 and 2⁸-1, two bytes would be used to represent values between 2⁸ and 2¹⁶-1, etc. For example, 10¹⁸ would cost 10 + 50 * 1 = 60 gas, while 10³⁰⁰ would cost 10 + 50 * 2 = 160 gas, since it takes one byte to represent 18 and two bytes to represent 300.

From the above, it is clear that there are certain times in which you should use multiplication over exponentiation. Here is a concrete example:

```
contract squareExample {
uint256 x;
constructor (uint256 _x) {
   x = _x;
 }
function inefficcientSquare() external {
   x = x**2;
 }
function efficcientSquare() external {
     x = x * x;
 }
}
```

Both *inefficcientSquare* and *efficcientSquare* set the state variable, *x*, to the square of itself. However, the arithmetic of *inefficcientSquare* costs 10 + 1 * 50 = 60 gas while the arithmetic of *efficcientSquare* costs 5 gas.

For reasons in addition to the above cost of arithmetic, *inefficcientSquare* costs ~200 more gas than *efficcientSquare* on average*.*

![5.png](https://img.learnblockchain.cn/attachments/2022/09/257Lojs26316af9026e6d.png)

**Caching data: SLOAD & MLOAD**

It is well known that caching data leads to far better performance at scale. However, caching data on the EVM is *extremely important* and will lead to dramatic gas savings even for a small number of operations.

The SLOAD and MLOAD opcodes are used to load data from storage and memory. MLOAD always cost 3 gas, while SLOAD’s cost is determined by a formula: SLOAD costs 2100 gas to initially access a value during a transaction and costs 100 gas for each subsequent access. This means that it is ≥97% cheaper to load data from memory than from storage.

Below is some sample code and the potential gas savings:

```
contract storageExample {
uint256 sumOfArray;
function inefficcientSum(uint256 [] memory _array) public {
        for(uint256 i; i < _array.length; i++) {
            sumOfArray += _array[i];
        }
} 
function efficcientSum(uint256 [] memory _array) public {
   
   uint256 tempVar;
   for(uint256 i; i < _array.length; i++) {
            tempVar += _array[i];
        }
   sumOfArray = tempVar;
} 
} // end of storageExample
```

The contract, storageExample, has two functions: **inefficcientSum** and **efficcientSum**

Both functions take *_array*, which is an array of unsigned integers, as an argument. They both set the contract’s state variable, *sumOfArray*, to the sum of the values in *_array*.

**inefficcientSum** uses the state variable, itself, for its calculations. Remember that state variables, such as *sumOfArray*, are kept in storage*.*

**efficcientSum** creates a temporary variable in memory, *tempVar*, that is used to calculate the sum of the values in *_array*. *sumOfArray* is then subsequently assigned to the value of *tempVar*.

*efficcientSum* is >50% gas efficient than *inefficcientSum* when passing in array of **only 10 unsigned integers.**

![6.png](https://img.learnblockchain.cn/attachments/2022/09/htSLDxPR6316af932c3d9.png)

These efficiencies scale with the number of computations: *efficcientSum* is >300% more gas efficient than *inefficcientSum* when passing in an array of 100 unsigned integers.

![7.png](https://img.learnblockchain.cn/attachments/2022/09/3cNHPYNU6316af975451d.png)

**Avoid Object Oriented Programming: the CREATE Opcode**

The CREATE opcode is used when creating a new account with associated code (i.e. a smart contract). It costs *at least* 32,000 gas and is the most expensive opcode on the EVM.

It is best to minimize the number of smart contracts used when possible. This is unlike typical object-oriented programming in which the separation of classes is encouraged for reusability and clarity.

**Here is a concrete example:**

Below is some code to create a “vault” using an object-oriented approach. Each vault contains a uint256, which is set in its constructor.

```
contract Vault {
    uint256 private x; 
    constructor(uint256 _x) { x = _x;}
    function getValue() external view returns (uint256) {return x;}
} // end of Vault
interface IVault {
    function getValue() external view returns (uint256);
} // end of IVault
contract InefficcientVaults {
    address[] public factory;
    constructor() {}
    function createVault(uint256 _x) external {
        address _vaultAddress = address(new Vault(_x)); 
        factory.push(_vaultAddress);
}
    function getVaultValue(uint256 vaultId) external view returns (uint256) {
        address _vaultAddress = factory[vaultId];
        IVault _vault = IVault(_vaultAddress);
        return _vault.getValue();
    }
} // end of InefficcientVaults
```

Each time that *createVault()* is called, a new *Vault* smart contract is created. The value stored in the *Vault* is determined by the argument passed into *createVault().* The address of the new *Vault* contract is then stored in an array, *factory.*

Now here is some code that accomplishes the same goal but uses a mapping in place of creating a new smart contract:

```
contract EfficcientVaults {
// vaultId => vaultValue
mapping (uint256 => uint256) public vaultIdToVaultValue;
// the next vault's id
uint256 nextVaultId;
function createVault(uint256 _x) external {
    vaultIdToVaultValue[nextVaultId] = _x;
    nextVaultId++;
}
function getVaultValue(uint256 vaultId) external view returns (uint256) {
    return vaultIdToVaultValue[vaultId];
}
} // end of EfficcientVaults
```

Each time that *createVault()* is called, its argument is stored in a mapping, and its ID is determined by the state variable, *nextVaultId,* which is incremented each time that *createVault()* is called.

This difference in implementation leads to a dramatic reduction in gas costs.

![8.png](https://img.learnblockchain.cn/attachments/2022/09/1NyGnqci6316af9a31bba.png)

EfficcientVaults’ *createVault()* is 61% more efficient and costs ~76,300 less gas than that of InefficcientVaults on average.

It should be noted that there are certain times when creating a new contract from within a contract is desirable and is typically done for immutability and efficiency. *The transaction cost for all interactions with a contract will increase with the size of a contract*. Therefore, if you expect to store massive amounts of data on-chain, it’s likely better to separate this data via separate contracts. However, if this is not the case, creating new contracts should be avoided.

**Storing Data: SSTORE**

SSTORE is the EVM opcode to save data to storage. As a generalization, SSTORE costs 20,000 gas when setting a storage value to non-zero from zero and 5000 gas when a storage value is set to zero.

Due to this cost, storing data on-chain is highly inefficient and costly. It should be avoided whenever possible.

This practice is most common with NFTs. Developers will store an NFT’s metadata (its image, attributes, etc.) on a decentralized storage network, like Arweave or IPFS, in place of storing it on-chain. The only data that is kept on-chain is a link to the metadata on the respective decentralized storage network. This link is queryable by the *tokenURI()* function found in all ERC721s that contain metadata.

![9.png](https://img.learnblockchain.cn/attachments/2022/09/mAjvW1Ku6316af9e93ae0.png)

A standard implementation of a tokenURI( ) function. (Source: [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/ERC721.sol))

For example, take the [Bored Ape Yacht Club smart contract](https://etherscan.io/token/0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d#readContract). Calling the *tokenURI( )* function with the tokenId, 0, returns the following link: *ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/0*

![10.png](https://img.learnblockchain.cn/attachments/2022/09/WmjqnrXF6316afa3478a4.png)

If you go to this link, you will find the JSON file that contains the BAYC #0’s metadata:

![11.png](https://img.learnblockchain.cn/attachments/2022/09/eBv6mhxk6316afa76f064.png)

These attributes are easily verifiable on [OpenSea](https://opensea.io/assets/ethereum/0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d/0):

![12.png](https://img.learnblockchain.cn/attachments/2022/09/PWDKlpqu6316afabdf409.png)

It should also be noted that certain data structures are simply unfeasible in the EVM due to the cost of storage. For example, representing [a graph using an adjacency matrix ](https://www.geeksforgeeks.org/graph-and-its-representations/)would be completely unfeasible due to its O(V²) space complexity.

*All of the above code can be found on my* [*Github*](https://github.com/tokyoDan67/evmOpcodeExamples)

Thank you for reading, and I hope you enjoyed this article!

There are so many more gas optimizations and nuances that I did not have a chance to cover. To learn more, I suggest the following resources:

- [*Tight Variable Packing*](https://fravoll.github.io/solidity-patterns/tight_variable_packing.html) and [*Memory Array Building*](https://fravoll.github.io/solidity-patterns/memory_array_building.html) by Franz Volland
- [*Solidity gas optimization tips*](https://mudit.blog/solidity-gas-optimization-tips/) and [*Solidity tips and tricks to save gas and reduce bytecode size*](https://blog.polymath.network/solidity-tips-and-tricks-to-save-gas-and-reduce-bytecode-size-c44580b218e6) by Mudit Gupta
- [*EVM: From Solidity to byte code, memory and storage*](https://www.youtube.com/watch?v=RxL_1AfV7N4&ab_channel=EthereumEngineeringGroup) by the Ethereum Engineering Group
- [*The Ethereum Yellowpaper*](https://ethereum.github.io/yellowpaper/paper.pdf)

*Please reach out to me and my team at* [*Bloccelerate VC*](https://www.bloccelerate.vc/) *if you are building in Web3. We are always looking to back great founders.*

[Website](https://bloccelerate.vc/)

[LinkedIn](https://www.linkedin.com/in/daniel-yamagata/)

[Twitter](https://twitter.com/daniel_yamagata)

*Feel free to also drop me a note if you have any suggestions for any toolings or topics that I should cover in the future*
