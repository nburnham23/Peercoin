# Peercoin
## Noah Burnham
## CS 3120 Final Project

For my final project, I have implemented Peercoin: a cryptocurrency that relies on Proof of Stake for mining. Proof of Stake mining is where a miner puts up a "stake" to prove they are a real person. The stake determines the set mining difficulty level. This scheme is an improvement over Bitcoin because it does not waste compute (and energy) because as time goes on, it gets easier for the miner to successfully mine a block. 

### Proof of Stake Mining
The standard mining difficulty is $2^{256} / 10000$. When a miner is mining a block, they try different nonce values and hash the block to see if the mining difficulty is satisfied (ie. the hash of the block is less than the difficulty). The miner must first put up a stake in order to mine, and the mining difficulty is increased (to make mining easier) based on the stake's coin-age. A stake is just a list of outputs from previous transactions. To compute the coin-age of any output, multiply the value of the output by the number of blocks since the coin was produced as an output. To compute the "total stake" -- the factor by which the mining difficulty will be interested -- add up all of the coin-ages of all the outputs making up the stake. Then, multiply the standard mining difficulty by $(1 + total stake)/100$. This will increase the mining difficulty number and make it *easier* to get a hash value for a block that satisfies the modified difficulty level. For a larger-scale system, the mining difficulty should increase slower, but for example purposes I used a smaller denominator.

Once a miner stakes coins (outputs from previous transactions), the coin-age should be reset. When a block is successfully mined, the current height of the blockchain is calculated by traversing the blockchain, and the output's `block_height` field is marked as the current height of the blockchain.

### Validating the blockchain
Validating the blockchain is very similar to validating the Bitcoin blockchain, but with one small tweak. To validate the blockchain, you need to check the hash to ensure that things haven't been tampered with, and check each transaction within the block to ensure that whoever was sending the Peercoin was authorized to do so (using signatures). The difference is that the verifier also has to check to see whether the block meets the difficulty requirement. Since the mining difficulty level is dynamic, the difficulty used to mine a block is stored within each block. 

Since each output gets hashed when validating the blockchain, and the stakes (which are outputs themselves) are mutated to reset their coin age, the `block_height` field of the Output class is omitted from the `__repr__` function.

