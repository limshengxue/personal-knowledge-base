2025-06-29 20:48

# 5 - A Small Application
## Using Lateral (Nested Loop)
Use Case: Finding top N track appeared in playlist for each genre
```sql
select genre.name as genre, 
	case when length(ss.name) > 15 
		then substring(ss.name from 1 for 15) || '…' 
		else ss.name 
	end as track, artist.name as artist from genre left join lateral 
	(select track.name, track.albumid, count(playlistid) 
	from track left join playlisttrack using (trackid) 
	where track.genreid = genre.genreid 
	group by track.trackid order by count desc limit :n) 
ss(name, albumid, count) on true 
join album using(albumid) 
join artist using(artistid)
order by genre.name, ss.count desc;
```

- **Lateral join** trigger a nested loop which select all track appear more than 5 times in different playlist, for each genre
- The `ss()` assign alias on the subquery columns
- The `on true` indicate join clause is not required, as the joining happened with the `where track.genreid = genre.genreid`




# References
