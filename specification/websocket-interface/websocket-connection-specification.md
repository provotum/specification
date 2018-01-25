Websocket Interface
====================

Base path for websocket connections is: `websocket`.
The following topics are assumed to be all relative to the base path specified above.
  
# Topic Endpoints aka Subscriptions aka responses to RESTful initialized requests
The following sections list events which are published as a response to a RESTful request.

## Deployments
The path for receving notifications about a successful resp. erroneous deployment is `/topic/deployments`.
A message published on this topic follows the structure below:

```json
{
  "id": "<UUID>",
  "responseType": "<ballot-deployed|zero-knowledge-deployed>",
  "status": "<success|error>",
  "contract": {
    "type": "<ballot|zero-knowledge>",
    "address": "<contract address>"
  },
  "message": "optional message, may be an empty string"
}
```

__Note__ that the element `address` may be null, if status equals to `error`.

## Removals
The path for receving notifications about a removal of a contract is `/topic/removals`.
A message published on this topic follows the structure:

```json
  {
    "id": "<UUID>",
    "responseType": "<ballot-removed|zero-knowledge-removed>",
    "status": "<success|error>",
    "transaction": "0x1234567890",
    "message": "optional message, may be an empty string"
  }

```

## Votes
The topic to listen for events corresponding to votes is `/topic/votes`.
They include the following events:

* `Vote`: The event indicating that a transaction was submitted in order to vote

```json
{
  "id": "<UUID>",
  "responseType": "<vote|vote-event>",
  "status": "<success|error>",
  "transaction": "<sender addresss>",
  "message": "optional message, may be an empty string",
}
```

## Opening resp. Closing Voting
The topic to listen for events opening resp. closing the vote: `/topic/state`
A message will follow the format below:

```json
{
  "id": "<UUID>",
  "responseType": "<open-vote|close-vote>",
  "status": "<success|error>",
  "transaction": "<sender addresss>",
  "message": "optional message, may be an empty string",
}
```


## Meta
A meta message usually includes information about the current state of a contract.
Currently, the following events are emitted:

### GetQuestionEvent
Contains the question of a ballot.

```json
{
  "id": "<UUID>",
  "responseType": "get-question-event",
  "status": "<success|error>",
  "message": "optional message, may be an empty string",
  "question": "<question>"
}
```

### GetResultsEvent
Contains the current votes of a ballot.

```json
{
  "id": "<UUID>",
  "responseType": "get-results-event",
  "status": "<success|error>",
  "message": "optional message, may be an empty string",
  "votes": {
    "<senderAddress1>": 0,
    "<senderAddress2>": 1,
    " ... ": 0
  }
}

```


# Blockchain events
The following sections describe events which happened on the blockchain and are relied to a topic.

## Events
The path for receving notifications about events is `/topic/events`.
The following events may occur:

* `ChangeEvent`: A event changing the current status of the Ballot contract. This event is usually emitted for:
  * Voting has been opened on the contract
  * Voting has been closed on the contract
  * The ballot contract has been removed
* `ProofEvent`: A event indicating that a proof validation has taken place on the Zero-Knowledge Contract.
* `VoteEvent`: The event on the blockchain, indicating that a vote has been processed.

All these events are of the form:
```json
  {
    "id": "<UUID>",
    "responseType": "<vote-event|change-event|proof-event>",
    "status": "<success|error>",
    "senderAddress": "0x1234567890",
    "message": "optional message, may be an empty string"
  }
```
