
# proposal

```
{
  "propose": {
    "description": "",
    "msgs": "",
    "title": ""
  }
}
```

### with optional arguments
```
{
  "propose": {
    "description": "",
    "msgs": [CosmosMsg_for_Empty],
    "title": "",
    "latest": : Expiration (One Of) -> [
        {"at_height": BlockHeight -> 200},
        {"at_time":[] Timestamp -> UINT64: A point in time in nanosecond precision. },
        {"never": {}}},
    ]
  }
}
```


##### latest Schema
```
"latest": {
    "anyOf": [
        {
            "$ref": "#/definitions/Expiration"
        },
        {
            "type": "null"
        }
    ]
},
```

```
{
  "propose": {
    "description": "Testing",
    "msgs": [],
    "title": "Testing Proposal",
  }
}
```

junod tx wasm execute $CW3_FIXED_MULTISIG_CONTRACT_ADDRESS '{"propose":{"description":"Testing","msgs":[],"title":"Testing Proposal"}}' --from admin-a $BASE_OPTIONS













