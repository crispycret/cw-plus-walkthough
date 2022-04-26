
# proposal

```
'{"propose": {"description": "Test Proposal Description", "msgs": "[]", "title": "Test Proposal"}}'
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





















