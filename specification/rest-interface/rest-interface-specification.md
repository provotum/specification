RESTful interface specification
============================

For requesting operations on the Blockchain, there is a REST interface. 
The following subsections describe the available endpoints.

**Note**: Generally, there is no direct response to any request you make to the REST API since operations on a blockchain may take an undefined amount of time. Therefore, the backend will process the incoming requests asynchronously and notify about results on a corresponding websocket topic.

**Note**: On success, all endpoints will return a HTTP status code of `202 Accepted` and an empty body.
**Note**: The endpoints may return a HTTP status of `400 Bad Request` and an empty body if fields are missing or the request is malformed.

# Zero-Knowledge Contract

## Deploy
Deploy a new zero-knowledge contract.
Pass as request body the following content:

**POST** `/zero-knowledge/deploy`
```
  {
    "public-key": {
      "p": 0,
      "g": 0
    }
  } 
```

The corresponding response will be published to the [Deployment Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#deployments).

## Remove
Remove a zero-knowledge contract at a specific address.

**DELETE**: `/zero-knowledge/{contractAddress}/remove`
No body has to be provided.

The corresponding response will be published to the [Removal Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#removals).

# Ballot Contract

## Deploy
Deploy a ballot contract. Voters are not allowed to vote yet.
Pass as request body the following content:

**POST** `/ballot/deploy`
```
{
  "election": {
    "question": "A question to vote on",
    "public-key": {
      "p": 0,
      "g": 0
    }
  },
  "addresses": {
    "zero-knowledge": "<zk contract address>"
  }
}
```

The corresponding response will be published to the [Deployment Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#deployments).

## Open Vote
Allow voters to vote on a specific contract.

**POST** `/ballot/{contractAddress}/open-vote`
No body has to be provided.

The corresponding response will be published to the [State Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#opening-resp-closing-voting).

## Vote
Vote on a specific contract.

**POST** `/ballot/{contractAddress}/vote`
```
  {
    "credentials": {
      "public-key": "",
      "private-key": ""
    },
    "vote": 1
  }
```

The corresponding response will be published to the [Vote Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#votes).

## Close Vote
Close the ability of voters to vote on a specific contract.

**POST** `/ballot/{contractAddress}/close-vote`
No body has to be provided.

The corresponding response will be published to the [State Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#opening-resp-closing-voting).


## Get Question
Request the question from the contract.

**POST** `/ballot/{contractAddress}/question`
No body has to be provided.

The corresponding response will be published to the [Meta Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#meta).

## Get Results
Request the current results of the vote from the contract specified.

**POST** `/ballot/{contractAddress}/results`
No body has to be provided.

The corresponding response will be published to the [Meta Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#meta).

## Remove
Remove the contract at the given address.

**DELETE** `/ballot/{contractAddress}/remove`

The corresponding response will be published to the [Removal Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#removals).