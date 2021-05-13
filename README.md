# Chinook database

/*QUERY 1 - Return the first 10 track names (in ascending order) along with the Album title 
that have a song length longer than the average song length*/

SELECT al.title AS album_name, t.name AS track_name, t.Milliseconds AS milliseconds
FROM track t
JOIN Album al
ON al.albumid = t.albumid
GROUP BY 1,2
HAVING t.Milliseconds > (SELECT AVG(t.milliseconds) AS avg_time
FROM track t)
ORDER BY 3
LIMIT 10;
