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
