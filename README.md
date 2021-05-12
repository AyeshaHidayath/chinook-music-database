# Chinook database

SELECT al.title AS album_name, t.name AS track_name, t.Milliseconds AS milliseconds
FROM track t
JOIN Album al
ON al.albumid = t.albumid
GROUP BY 1,2
HAVING t.Milliseconds > (SELECT AVG(t.milliseconds) AS avg_time
FROM track t)
ORDER BY 3
LIMIT 10;
