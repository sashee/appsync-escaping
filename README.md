# Demonstration code for AppSync escaping

## Deploy

* ```terraform init```
* ```terraform apply```

## Use

```graphql
query MyQuery {
  basic_u: unescaped(input: "test")  
  basic_e: escaped(input: "test")
  missing_u: unescaped
  missing_e: escaped
  quote_u: unescaped(input: "\"")
  quote_e: escaped(input: "\"")
}
```

Result:

```json
{
  "data": {
    "basic_u": "test",
    "basic_e": "test",
    "missing_u": "$ctx.args.input",
    "missing_e": null,
    "quote_u": null,
    "quote_e": "\""
  },
  "errors": [
    {
      "path": [
        "quote_u"
      ],
      "data": null,
      "errorType": "MappingTemplate",
      "errorInfo": null,
      "locations": [
        {
          "line": 6,
          "column": 3,
          "sourceName": null
        }
      ],
      "message": "Unexpected character ('\"' (code 34)): was expecting comma to separate Object entries\n at [Source: (String)\"{\n\t\"version\": \"2018-05-29\",\n\t\"payload\": \"\"\"\n}\n\"; line: 3, column: 16]"
    }
  ]
}
```

## Cleanup

* ```terraform destroy```
