Voting Process
==============

## Setup phase
**Actor**: Voting authority


1. A question to vote on has to be defined.
2. Public encrpytion parameters need to be specified.
3. ZK and Ballot contract are deployed using the information of step 1. & 2. 
4. The voting authority opens the vote.


## Voting phase 
**Actor**: Any person

1. Voter is authenticated and authorized upfront of accessing the voting application.
    * Authentication (& Authorization) is not linked to the voting process
    * Allows for integration of different authorization providers (OpenID connect, ERC725, ...)
2. The client receives the voting question and encryption parameters from the ballot contract (using the backend application as proxy for the contract address).
    * Alternatively, the client receives the contract address of the ballot only and fetches the question as well as the encryption parameters itself.
3. The voter makes an informed decision on the topic.
4. His cleartext vote is sent to the backend (0/1).
5. The cleartext vote is encrypted on the backend.
6. The backend generates a zero-knowledge proof for the encrypted message.
7. The backend sends the encrypted vote as well as the ZK proof to the client.
8. The voter can directly submit the encrypted vote & the ZK proof to the ballot contract on the client using Web3JS.
9. The ballot contract only accepts the vote if the zero-knowledge proof is valid.

## Closing phase
**Actor**: Voting authority

1. The vote is closed. No votes are accepted anymore after this point in time.
2. The voting results are collected from the ballot contract.
3. Votes are counted homomorphically and the result is decrypted.
4. The result is published in the ballot contract.

## Evaluation

**Advantages**
* The voter remains in possession of his private key, there is no need to have sent that information to any third party.
* The voter is authenticated and authorized from a 3rd party without restriction on how he is actually identified.
* The voter is not related in any way to his clear text vote. The server receives at most an IP-derived location of the client as well as public information of the browser. Using VPNs and other mechanisms, the voter may restrict any information provided.

**Disadvantage**
* The voter must be able to request ether from a faucet to his wallet, in order to be able to vote. An pessimistic calculation on the cost of a voting transaction must then be made beforehand by the authorities.
