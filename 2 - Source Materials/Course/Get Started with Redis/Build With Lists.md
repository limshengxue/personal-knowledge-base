2026-03-15 14:31

# Build With Lists
- An ordered group of string elements
- Head and tail (beginning and end of the list)

## Commands to Work With List
Add element from the head/tail
`LPUSH products:recent X`
`RPUSH products:recent X`

Check the length
`LLEN products:recent`

Remove element from tail/head
`RPOP products:recent`
`LPOP products:recent`

Get Element
`LRANGE products:recent 0 2` - a range of element
`LRANGE products:recent 0 -1` accept negative index
`LINDEX products:recent 1` - single element


## Use Cases
- When order matters
- As a message queue
- As a stack

# References
