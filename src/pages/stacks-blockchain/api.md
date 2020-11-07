---
title: Stacks Blockchain API
description: Overview of the Stacks 2.0 Blockchain API
---

## Introduction

The Stacks 2.0 Blockchain API allows you to query the Stacks 2.0 blockchain and interact with smart contracts.

The API can be used without any authorization. The basepath for the API is:

```bash
https://stacks-node-api.blockstack.org/
```

The API is comprised of two parts: the Stacks Blockchain API and the Stacks Node RPC API.

### Stacks Node RPC API

The [stacks-node implementation](https://github.com/blockstack/stacks-blockchain/) exposes JSON RPC endpoints.

All `/v2/` routes a proxied to a Blockstack PBC-hosted Stacks Node. For a trustless architecture, you should make these requests to a self-hosted node.

### Stacks Blockchain API

All `/extended/` routes are provided by the Stacks 2.0 Blockchain API directly. They extend the Stacks Node API capabilities to make it easier to integrate with.

## Using the API

Depending on your programming environment, you might need to access the API differently.
The easiest way to start interacting with the API might be through the [Postman Collection](https://app.getpostman.com/run-collection/614feab5c108d292bffa#?env%5BStacks%20Blockchain%20API%5D=W3sia2V5Ijoic3R4X2FkZHJlc3MiLCJ2YWx1ZSI6IlNUMlRKUkhESE1ZQlE0MTdIRkIwQkRYNDMwVFFBNVBYUlg2NDk1RzFWIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJibG9ja19pZCIsInZhbHVlIjoiMHgiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im9mZnNldCIsInZhbHVlIjoiMCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoibGltaXRfdHgiLCJ2YWx1ZSI6IjIwMCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoibGltaXRfYmxvY2siLCJ2YWx1ZSI6IjMwIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ0eF9pZCIsInZhbHVlIjoiMHg1NDA5MGMxNmE3MDJiNzUzYjQzMTE0ZTg4NGJjMTlhODBhNzk2MzhmZDQ0OWE0MGY4MDY4Y2RmMDAzY2RlNmUwIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJjb250cmFjdF9pZCIsInZhbHVlIjoiU1RKVFhFSlBKUFBWRE5BOUIwNTJOU1JSQkdRQ0ZOS1ZTMTc4VkdIMS5oZWxsb193b3JsZFxuIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJidGNfYWRkcmVzcyIsInZhbHVlIjoiYWJjIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJjb250cmFjdF9hZGRyZXNzIiwidmFsdWUiOiJTVEpUWEVKUEpQUFZETkE5QjA1Mk5TUlJCR1FDRk5LVlMxNzhWR0gxIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJjb250cmFjdF9uYW1lIiwidmFsdWUiOiJoZWxsb193b3JsZCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY29udHJhY3RfbWFwIiwidmFsdWUiOiJzdG9yZSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY29udHJhY3RfbWV0aG9kIiwidmFsdWUiOiJnZXQtdmFsdWUiLCJlbmFibGVkIjp0cnVlfV0=) or [cURL](https://curl.haxx.se/).

-> Postman allows you to [generate sample code](https://learning.postman.com/docs/sending-requests/generate-code-snippets/) for API requests for various languages and libraries

## OpenAPI spec

The API was design using the [OpenAPI specification](https://swagger.io/specification/), making it compatible with a variety of developer tools.

Thanks to this design choice, we were able to generate parts of our JS/TS client library purely from the specification file. The client generation is done using the [openapi-generator](https://github.com/OpenAPITools/openapi-generator).

-> The client generator supports a variety of languages and might be helpful if you are looking to integrate the API using a different lanugage than Javascript

## JS client library

A generated JS Client is available for consumption of this API. The client library enables typesafe REST and WebSocket communication. [Please review the client documentation for more details](https://blockstack.github.io/stacks-blockchain-api/client/index.html).

The client is made up of three components:

1. Generate API client
2. TypeScript definitions for Clarity values
3. WebSocket client

The following chapters will demonstrate the usage of each component.

### API client sample

It is importanrt to note that the JS client requires setting the request library that will handle HTTP communication. The example below uses the universal fetch API [`cross-fetch`](https://github.com/lquixada/cross-fetch):

```js
import fetch from 'cross-fetch';
import { Configuration AccountsApi } from '@stacks/blockchain-api-client';

(async () => {
  const apiConfig = new Configuration({
    fetchApi: fetch,
    basePath: 'https://stacks-node-api.blockstack.org', // defaults to http://localhost:3999
  });

  // initiate the /accounts API with the basepath and fetch library
  const accountsApi = new AccountsApi(apiConfig);

  // get transactions for a specific account
  const txs = await accountsApi.getAccountTransactions({
    principal: 'ST000000000000000000002AMW42H',
  });

  console.log(txs);

})().catch(console.error);
```

### TypeScript sample

-> Todo: add sample

### WebSocket sample

The WebSocket components enabled you to subscribe to specific updates, enabling a near real-time display of updates on transactions and accounts.

```js
import { connectWebSocketClient } from '@stacks/blockchain-api-client';

const client = await connectWebSocketClient('ws://stacks-node-api.blockstack.org/');

const sub = await client.subscribeAddressTransactions(contractCall.txId, event => {
  console.log(event);
});

await sub.unsubscribe();
```

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
