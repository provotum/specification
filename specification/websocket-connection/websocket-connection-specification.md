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
  
 * `/vote`: Submit a vote to the contract. You'll need to provide a message of the following structure:
   ```json
   {
      "contract-address": "<smart contract address>",
      "vote": 1,
      "credentials": {
        "public-key": 1234567890,
        "private-key": 0987654321
      }
   }
   ```

# Zero-Knowledge Verification Contract

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
  
# Topic Endpoints

## Deployment
The path for receving notifications about a successful resp. erroneous deployment is `/topic/deployments`.

```json
{
  "id": "<UUID>",
  "status": "<success|error>",
  "contract": {
    "type": "<ballot|zero-knowledge>",
    "address": "<contract address>"
  },
  "message": "optional message, may be an empty string"
}
```

__Note__ that the element `address` may be null, if status equals to `error`.

## Ballot: VoteEvent
An event tracing the state of a particular vote. Its topic is `/topic/ballot/vote-event`.

```json
{
  "id": "<UUID>",
  "status": "<success|error>",
  "message": "the event message",
  "senderAddress": "<sender addresss>"
}
```

## Ballot: ChangeEvent
An event tracing the state of the ballot contract. Its topic is `/topic/ballot/change-event`.

```json
{
  "id": "<UUID>",
  "status": "<success|error>",
  "message": "the event message",
  "senderAddress": "<sender addresss>"
}
```

## Zero Knowledge: ProofEvent
An event tracing the state of the proof. Its topic is `/topic/zero-knowledge/proof-event`.

```json
{
  "id": "<UUID>",
  "status": "<success|error>",
  "message": "the event message",
  "senderAddress": "<sender addresss>"
}
```
