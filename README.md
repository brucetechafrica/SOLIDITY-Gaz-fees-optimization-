# SOLIDITY-Gaz-fees-optimization-
Gaz fee optimization on solidity 

Coding in solidity is a little different than other languages as every action you do cost real gas.
Every time a transaction get’s sent to the blockchain, gas fees must be paid. The amount of gas is related to the amount of computation the transaction requires, in other words, the amount of computation the EVM will have to perform to execute the transaction (in case the transaction does not involve the EVM, a simple Ether transfer for example, the amount of gas is fixed).

Here are some tips and tricks to help you keep your gas cost low.

# Gaz efficient💡

There are two “types” of gas that I will be talking about in this repository :
Transaction Gas : The amount of gas your users will have to pay every time they interact with your smart contract. The idea here is to implement gas efficient functions that consume as little gas as possible.
Deployment Gas : The amount of gas that you will have to pay every time you deploy your smart contracts. Deploying smart contract is something that usually only happens once, but still saving gas can be interesting for you.

Sometimes, techniques to reduce one type of gas can cause the other type of gas to increase, this is a tradeoff you will have to deal with…

# SAVING Gaz general tips 

🌟Minimize on-chain data (events, IPFS, stateless contracts, merkle proofs)

🌟 Minimize on-chain operations (strings, return storage value, looping, local storage, batching)

🌟 Variables ordering

🌟 Libraries (embedded, deploy)

🌟 Contract size (messages, modifiers, functions)

🌟 Solidity compiler optimizer

🌟 Minimal Proxy

# 🔴MINIMIZE on-chain data

Saving data on storage memory is expensive, if you manage to reduce the amount of information that you need to store on the blockchain to a minimum, you will be saving a lot of transaction Gas.

🔺 Events : You can consider using events to “store” data on the blockchain. An event is a piece of information that will actually be stored on the blockchain, only that it will not be part of your contract’s storage, in fact, it will not be possible for smart contracts to read or use events in any way. Events are only available to off-chain applications reading the blockchain. This is why events are not to be used if your smart contract requires that information, but if you just need the data to be persisted on the blockchain only for reading purposes.

🔺 IPFS : In case you need to save files (documents, videos, …) in a decentralized way, you should consider IPFS (a distributed, cheap file storage). Each file stored on IPFS will have a unique ID that you can store on the blockchain for reference, but the actual file will be stored in IPFS.

🔺 Merkle Proofs : If you need to use the blockchain to verify if some information is valid or not, you can use merkle proofs. A merkle proof uses a single chunk of data in order to prove the validity of a much larger amount of data. The idea is that you will only need tot store the Merkle tree root on the blockchain (Hash12345678) in order to be able to validate multiple transactions (Tx1 …. Tx8). If for example someone wants to prove “Tx4” validity, he will need to provide Tx4, Hash3, Hash12 and Hash5678, then your contract will be able to recalculate the merkle root (Hash12345678) and check if it corresponds to the one stored on the blockchain. You will not need to store the hashes of all the transactions.


# 🔴Minimize on-chain operations

Only add to your smart contracts the functionality that for security, legal, or any other very good reason, needs to be performed on the blockchain. Keep all the remaining tasks off-chain, in a dedicated backend or even your front end, that way you will save transaction gas.

🔺 Strings : strings are just bytes to ethereum. Even if both data types exist, the EVM will process strings as bytes, which requires some overhead, meaning that if you can use bytes instead of strings do it. If you still need to use strings, then try to keep string operations (concatenation, etc…) outside of your smart contracts.

🔺 Return storage values : if you need to return storage values after executing some functionality. Return it as it is, without transforming it, let the off-chain application retrieving the data do the work (extract certain values from an array etc…).

🔺 Looping : avoid looping through long arrays, not only it will cost a lot a gas but it can even make your contract impossible to be executed if gas costs increase to much (beyond the Block gas limit). Use mappings instead which are hash tables that will let you access any value in a single operation using its key, instead of looping through an array until you find the key you are looking for.

🔺 Local storage : local storage variables are method local variables that point to an actual state variable (stored in storage). Instead of copy/pasting storage arrays in memory in order to manipulate them, then copying them back to storage, simply use local storage variables and work on the storage directly.

# 🔴Variables Ordering

Solidity storage slots are 32 bytes long, but not all data types take that amount of space : bool, int8 … int128, bytes1 … bytes31 and addresses take less than 32 bytes.
The solidity compiler will try to pack together variables in a single slot, but these variables need to be defined next to each other.
For example, if you define 2 int128 next to each other, they will both be packed into the same storage slot since they take 16 bytes each. However if you define an int128, followed by a unit256, then another int128, you will be using 3 storage slots since the unit256 in between the 2 int128 need a full storage slot.

![This is an image](https://miro.medium.com/max/828/1*ZUWhvnvonDWhJv_dEJL_4Q.png)

![This is an image](https://miro.medium.com/max/828/1*mKFJ9UE85mJ1uA0iQv8F4g.png)

You will be able to save storage space and transaction gas doing this.


# 🔴Libraries (embedded, deploy)

If you are going to re-use code among your smart contracts then you better pack all that code into a library, deploy it and make your contracts point to it by importing it.
Libraries can be of two types.

Libraries can be of two types.
🔺Embedded Libraries: libraries that contain internal functionality. These libraries do not get deployed but embedded into your contract, meaning that you will deploy their code along your smart contract code… You will not be re-using anything nor saving any Gas with these type of libraries….

🔺 Deployed Libraries: libraries that contain public or external functionality. These libraries get deployed once, then all smart contracts importing them will be actually delegating calls to them. This means that the library code gets deployed only once then used by all smart contracts. You will be saving deployment Gas if you use this type of library.

# 🔴Minimal Proxies (ERC 1167)

If you are going to need to deploy multiple contracts with exactly the same functionality you should consider using “Minimal Proxies” (defined in the ERC 1167).
A minimal proxy is just a contract that will delegate all its calls to a pre-defined implementation contract, nothing else. There is already a well defined byte-code that represents the Minimal proxy contract compiled code, you will simply need to insert your implementation contract address into it and you are ready to deploy as many copies of your minimal proxy as you need.
Since that byte-code is so minimal, the cost of deploying it is as low as it can get, you will be saving a bunch of deployment Gas.
There is a caveat with minimal proxies that you should keep in mind: Minimal proxy implementation contract address cannot be changed, meaning that you will not be able to migrate their code.

