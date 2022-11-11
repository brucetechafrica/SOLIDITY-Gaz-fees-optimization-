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



