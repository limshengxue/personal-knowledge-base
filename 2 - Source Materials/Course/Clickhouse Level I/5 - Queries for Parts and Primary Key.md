2025-11-09 10:55

# 5 - Queries for Parts and Primary Key
View the number of active parts
```SQL
SELECT * 
FROM system.parts
WHERE table = 'uk_prices_1'
AND active = 1;
```

View compressed and uncompressed size taken by the active parts
```SQL
SELECT
    formatReadableSize(sum(data_compressed_bytes)) AS compressed_size,
formatReadableSize(sum(data_uncompressed_bytes)) AS uncompressed_size
FROM system.parts
WHERE table = 'uk_prices_1' AND active = 1;
```



# References
