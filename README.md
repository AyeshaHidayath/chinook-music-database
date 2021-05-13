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




/*QUERY 2 - Top 10 Customers who spent the most on the highest earned artist*/

WITH t1 AS ( SELECT a.Name AS artist_name , COUNT(il.Quantity)*(il.UnitPrice) AS total_spent
			FROM invoice i
			JOIN InvoiceLine il
			ON i.InvoiceId = il.InvoiceId
			JOIN Track t
			ON t.TrackId = il.TrackId
			JOIN Album al
			ON al.AlbumId = t.AlbumId
			JOIN Artist a
			ON a.ArtistId = al.ArtistId
			GROUP BY 1
			ORDER BY 2 DESC
			LIMIT 1 ) 
			
SELECT  a.name AS artist_name, c.CustomerId, c.FirstName, c.LastName, 
		c.FirstName||' '||c.LastName AS full_name,
		COUNT(il.Quantity)*(il.UnitPrice) AS total_spent
        
FROM Customer c
JOIN invoice i
ON c.CustomerId = i.CustomerId
JOIN InvoiceLine il
ON i.InvoiceId = il.InvoiceId
JOIN Track t
ON t.TrackId = il.TrackId
JOIN Album al
ON al.AlbumId = t.AlbumId
JOIN Artist a
ON a.ArtistId = al.ArtistId
GROUP BY 1,2,3,4
HAVING a.Name = ( SELECT artist_name 
		  FROM t1)
ORDER BY 6 DESC
LIMIT 10; 



/*QUERY 3 - Finding the total purchases for each Genre and  
  grouping them based on their demands using CASE statement.*/
  
  SELECT  g.name AS GenreName, g.GenreId AS genreId,  COUNT(i.total) AS Purchases,
   	CASE	WHEN COUNT(i.total) BETWEEN 0 AND 10 THEN '5 (Very Low Demand)' 
		WHEN COUNT(i.total) BETWEEN 11 AND 100 THEN '4 (Low Demand)'
		WHEN COUNT(i.total) BETWEEN 101 AND 300 THEN '3 (Moderate Demand)'
		WHEN COUNT(i.total) BETWEEN 301 AND 500 THEN '2 (High Demand)'
	ELSE '1 (Very high demand)' END AS Ranking_based_on_demand
FROM GENRE g
JOIN Track t
ON g.Genreid = t.genreid
JOIN invoiceline il
ON il.trackid = t.trackid
JOIN invoice i
ON i.invoiceid = il.invoiceid
GROUP BY 1,2
ORDER BY 2; 



