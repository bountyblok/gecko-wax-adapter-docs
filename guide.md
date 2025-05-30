# Gecko WAX Adapter Quick Test Guide

Use the following commands to quickly verify each of your four endpoints against the test instance at **gecko-api.wax.io**.

---

## Base URL

```
https://gecko-api.wax.io
```

---

## 1. GET /latest-block

Fetch the latest WAX block number and its Unix timestamp.

```bash
curl -s "https://gecko-api.wax.io/latest-block" | jq .
```

**Expected response format:**

```json
{
  "block": {
    "blockNumber": 373909916,
    "blockTimestamp": 1748433738
  }
}
```

---

## 2. GET /asset

Retrieve metadata for an on-chain token. Replace `<contract>:<SYMBOL>` as needed. Example for the main WAX token:

```bash
curl -s "https://gecko-api.wax.io/asset?id=eosio.token:WAX" | jq .
```

**Expected response format:**

```json
{
  "asset": {
    "id": "eosio.token",
    "name": "WAX",
    "symbol": "WAX",
    "decimals": 8,
    "totalSupply": 4395032909.800781
  }
}
```

---

## 3. GET /pair

Fetch details for an Alcor pool. Use one of the `pairId`s you see in your /events output. Example with `6559`:

```bash
curl -s "https://gecko-api.wax.io/pair?id=swap.alcor:6559" | jq .
```

**Expected response format:**

```json
{
  "pair": {
    "id": "swap.alcor:6559",
    "dexKey": "swap.alcor",
    "asset0Id": "alpha.waxfun",
    "asset1Id": "eosio.token",
    "feeBps": 3000
  }
}
```

---

## 4. GET /events

List all swap/join/exit events between two block numbers. Example:

```bash
curl -s "https://gecko-api.wax.io/events?fromBlock=373901885&toBlock=373901885" | jq .
```

**Expected response format:**

```json
{
  "events": [
    {
      "eventType": "swap",
      "block": {
        "blockNumber": 373901885,
        "blockTimestamp": 1748429723
      },
      "txnId": "4e40366b9e6bd4593...",
      "txnIndex": 0,
      "eventIndex": 0,
      "maker": "waxapple1123",
      "pairId": "swap.alcor:6559",
      "asset0In": 222168.94625682,
      "asset1Out": 80.09999999,
      "priceNative": 0.0003605364356250177,
      "reserves": { "asset0": 320961222.8018034, "asset1": 115623.03428906 }
    }
  ]
}
```

---

## Error responses

All endpoints return **HTTP 502** if an upstream call (WAX RPC or Hyperion) fails:

```json
{ "error": "<detailed error message>" }
```

---
