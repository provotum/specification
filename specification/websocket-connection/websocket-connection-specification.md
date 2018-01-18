Websocket Connection
====================

Base path for websocket connections is: `websocket`.
The following paths resp. topics are assumed to be relative to the base path specified above.

# Ballot Contract

The path for the ballot contract is `/contracts/ballot`.
It provides the following endpoints:

## Message Endpoints
Send messages to the listed endpoints in order to interact with a ballot contract.

* `/deploy`: Deploy the contract. You need to provide a message following the structure below:
  ```json
   {
      "addresses": {
        "zero-knowledge": "<smart contract address>"
      },
      "election": {
        "question": "<a question the participants want to vote on>",
        "public-key": {
          "p": 0,
          "g": 0
        }
      }
   }
  ```
  
## Topic Endpoints
You can subscribe to the listed endpoints in order to get notified about changes on the ballot contract:

* `/subscription/deployment`: Get notified about the ballot contract address, once it is deployed.
  ```json
    {
      "address": "<smart contract address>",
      "error": {
        "message": "<an error message, if present>"
      }
    }
  ```


## Zero-Knowledge Verification Contract

 The path for the zero-knowledge verification contract is `/contracts/zero-knowledge`.
 It provides the following endpoints:

## Message Endpoints
Send messages to the listed endpoints in order to interact with a ballot contract.

* `/deploy`: Deploy the contract. You need to provide a message following the structure below:
  ```json
   {
      "public-key": {
          "p": 0,
          "g": 0
        }
   }
  ```
  
## Topic Endpoints
You can subscribe to the listed endpoints in order to get notified about changes on the ballot contract:

* `/subscription/deployment`: Get notified about the zero-knowledge verification contract address, once it is deployed.
  ```json
    {
      "address": "<smart contract address>",
      "error": {
        "message": "<an error message, if present>"
      }
    }
  ```
