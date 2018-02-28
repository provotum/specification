RESTful interface specification
============================

For requesting operations on the Blockchain, there is a REST interface. 
The following subsections describe the available endpoints.

**Note**: Generally, there is no direct response to any request you make to the REST API since operations on a blockchain may take an undefined amount of time. Therefore, the backend will process the incoming requests asynchronously and notify about results on a corresponding websocket topic.

**Note**: On success, all endpoints will return a HTTP status code of `202 Accepted` and an empty body, if not noted differently.
**Note**: The endpoints may return a HTTP status of `400 Bad Request` and an empty body if fields are missing or the request is malformed.

# Table of contents
1. [Zero-Knowledge Contract Endpoint](#zero-knowledge-contract)
2. [Ballot Contract Endpoint](#ballot-contract)
3. [Encryption Endpoint](#encryption)

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
**Deprecated**: Use the endpoint of [Encryption](#encryption) instead.

~~Vote on a specific contract.~~

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

~~The corresponding response will be published to the~~ [Vote Topic](https://github.com/provotum/specification/blob/master/specification/websocket-interface/websocket-connection-specification.md#votes).

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

# Encryption

## Encrypt and generate proof
Encrypt the given vote and generate a corresponding proof, that the vote is within the boundary of a valid vote, i.e. \[0,1\].

**POST** `/encryption/generate`
```
  {
    "vote": 1
  } 
```

The response will have a status code of `201` and contain the ciphertext as well as the corresponding proof in the body:

```
{
  "ciphertext": "<string>",
  "proof": "<string>",
  "random": "<string>"
}
```


## Verify a ciphertext
The backend allows to verify a proof of a particular encryption, i.e. that the vote is within the boundary of a valid vote, i.e. \[0,1\].

**POST** `/encryption/verify`
```
{
  "ciphertext": "<string>",
  "proof": "<string>"
}
```

If the proof is valid for the given ciphertext, then a status code of `200` is returned, `409` otherwise.
The response body will be empty in all cases.
