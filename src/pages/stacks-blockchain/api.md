---
title: Stacks Blockchain API
description: Overview of the Stacks 2.0 Blockchain API
---

## Introduction

The Stacks 2.0 Blockchain API allows you to query the Stacks 2.0 blockchain and interact with smart contracts.

## Using the API

-> Todo: Postman collection

## OpenAPI spec

-> Todo: Generation of client libraries for other languages

## JS client library

A generated JS Client is available for consumption of this API. The client library enables typesafe REST and WebSocket communication. [Please review the client documentation for more details](https://blockstack.github.io/stacks-blockchain-api/client/index.html).

-> Todo: Client library overview (including WebSockets, and TS type generation)

## Rate limiting

Rate limiting is only applied to [faucet requests](https://blockstack.github.io/stacks-blockchain-api/#tag/Faucets) and based on the address that tokens are requested for.

### BTC Faucet

The bitcoin faucet is limited to **5 requests per 5 minutes**.

### STX Faucet

The Stacks faucet rate limits depend on the type of request. For stacking requests, a limitation of **1 request per 2 days**. In case of regular Stacks faucet requests, the limits are set to **5 requests per 5 minutes**.

## Pagination

To make API responses more compact, lists returned by the API are paginated. For lists, the response body includes:

- `limit`: the number of list items return per response
- `offset`: the number of elements to skip (starting from 0)
- `total`: the number of all available list items
- `results`: the array of list items (length of array equals the set limit)

Here is a sample response:

```json
{
  "limit": 10,
  "offset": 0,
  "total": 101922,
  "results": [
    {
      "tx_id": "0x5e9f3933e358df6a73fec0d47ce3e1062c20812c129f5294e6f37a8d27c051d9",
      "tx_status": "success",
      "tx_type": "coinbase",
      "fee_rate": "0",
      "sender_address": "ST3WCQ6S0DFT7YHF53M8JPKGDS1N1GSSR91677XF1",
      "sponsored": false,
      "post_condition_mode": "deny",
      "block_hash": "0x58412b50266debd0c35b1a20348ad9c0f17e5525fb155a97033256c83c9e2491",
      "block_height": 3231,
      "burn_block_time": 1594230455,
      "canonical": true,
      "tx_index": 0,
      "coinbase_payload": {
        "data": "0x0000000000000000000000000000000000000000000000000000000000000000"
      }
    },
    { ... }
  ]
}
```

Using the `limit` and `offset` properties, you can paginate through the entire list by increasing the offset by the limit until you reach the total.

## Requesting proofs

## Searching for transactions and/or blocks

## Response types

- Clarity value in JSON format

## Error handling

## Proxied Stacks Node RPC endpoints

## Rosetta support

## Running an API server
