2026-03-15 15:13

# Build with sorted sets
- A set where each member is associated with a score
- The score is then used to sort the members

## Commands
`ZADD product:rank 4.5 apple`
`ZSCORE product:rank apple` - return the score
`ZRANK product:rank BOWTIE` - return a rank, ascending order, 0-based
`ZRANGE product:rank 0 -1 WITHSCORES` - return a range
`ZRANGE product:rank 4 5 BYSCORE WITHSCORES` - return item with score between 4 and 5 (inclusive)

## Use Cases
- Leaderboards
- Recommendation Engines


# References
