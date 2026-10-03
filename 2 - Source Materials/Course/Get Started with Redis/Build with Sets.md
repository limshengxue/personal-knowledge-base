2026-03-15 14:43

# Build with Sets
- Unordered collection of unique string elements

## Commands
`SADD product:views alice`
`SMEMBERS product:views` - view the members
`SCARD product:views` - view count of a set
`SREM product:views alice` - remove a member
`SUNION` - union
`SINTER` - intersect
`SDIFF` - different between sets (order matters for this operation)


## Use Cases
- Counting and tracking unique things
- Deduplicate things


# References
