2026-03-15 15:34

# Build with JSON
- We can store serialized JSON or JSON document
- JSON document is easier to query specific field and allow typing

## Commands
`JSON.SET product:apple $ '{"name":"Fuji", "color":"red"}'`
`JSON.GET product:apple`
`JSON.GET product:apple $.name`
`JSON.SET product:apple $.quantity 10`
`JSON.DEL product:apple $.quantity`


## Use Cases
- Nested data (not supported by hash)
- To store & cache records/session data
- Allow index and search



# References
