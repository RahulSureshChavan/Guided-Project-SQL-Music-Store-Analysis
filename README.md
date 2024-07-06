# Guided-Project-SQL-Music-Store-Analysis
The project is guided by Rishabh Mishra on YouTube https://www.youtube.com/@RishabhMishraOfficial.

Aim: To the answer the following business questions using SQL on Bigquery.

1. Who is the senior most employee based on job title?

SELECT
  *
FROM
  premium-strata-428618-p6.music_store_data.employee
ORDER BY levels DESC
LIMIT 1

2. Which countries have the most Invoices?

SELECT
  billing_country,
  COUNT(*) as Number_of_invoices
FROM
  premium-strata-428618-p6.music_store_data.invoice
GROUP BY
  billing_country
ORDER BY
  Number_of_invoices DESC

3. What are top 3 values of total invoice?

SELECT
  total
FROM 
  premium-strata-428618-p6.music_store_data.invoice
ORDER BY total DESC
LIMIT 3

4. Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals. 
Return both the city name & sum of all invoice totals.

SELECT
  billing_city,
  SUM(total) as total_invoices
FROM
  premium-strata-428618-p6.music_store_data.invoice
GROUP BY
  billing_city
ORDER BY 
  total_invoices DESC
LIMIT 1

5. Who is the best customer? The customer who has spent the most money will be declared the best customer. 
Write a query that returns the person who has spent the most money.

SELECT
  c.customer_id,
  c.first_name,
  c.last_name,
  SUM(i.total) as total
FROM
  premium-strata-428618-p6.music_store_data.customer as c
JOIN premium-strata-428618-p6.music_store_data.invoice as i
  ON c.customer_id = i.customer_id
GROUP BY 
  c.customer_id,
  c.first_name,
  c.last_name
ORDER BY total DESC
LIMIT 1

6. Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
Return your list ordered alphabetically by email starting with A.

SELECT DISTINCT
  email,
  first_name,
  last_name
FROM
  premium-strata-428618-p6.music_store_data.customer as c
JOIN premium-strata-428618-p6.music_store_data.invoice as i
  ON c.customer_id = i.customer_id
JOIN premium-strata-428618-p6.music_store_data.invoice_line as il
 ON i.invoice_id = il.invoice_id
WHERE 
  track_id IN(
    SELECT 
      track_id
    FROM
      premium-strata-428618-p6.music_store_data.track as t
    JOIN premium-strata-428618-p6.music_store_data.genre as g
    ON t.genre_id = g.genre_id
    WHERE
      g.name = 'Rock'
  )
  ORDER BY c.email

7. Let's invite the artists who have written the most rock music in our dataset. 
Write a query that returns the Artist name and total track count of the top 10 rock bands.

SELECT
  a.artist_id,
  a.name,
  COUNT(a.artist_id) as number_of_songs
FROM premium-strata-428618-p6.music_store_data.artist as a
JOIN premium-strata-428618-p6.music_store_data.album as al
ON a.artist_id = al.artist_id
JOIN premium-strata-428618-p6.music_store_data.track as t
ON al.album_id = t.album_id
JOIN premium-strata-428618-p6.music_store_data.genre as g
ON t.genre_id = g.genre_id
WHERE
  g.name = 'Rock'
GROUP BY a.artist_id, a.name
ORDER BY number_of_songs DESC

8. Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.

SELECT
  name,
  milliseconds
FROM
  premium-strata-428618-p6.music_store_data.track
 WHERE milliseconds > (
    SELECT AVG(milliseconds)
    FROM premium-strata-428618-p6.music_store_data.track
 )
 ORDER BY milliseconds DESC

13. Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent.

14. We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre 
with the highest amount of purchases. Write a query that returns each country along with the top Genre. For countries where 
the maximum number of purchases is shared return all Genres.

15. Write a query that determines the customer that has spent the most on music for each country. 
Write a query that returns the country along with the top customer and how much they spent. 
For countries where the top amount spent is shared, provide all customers who spent this amount.

Snippets of query results are uploaded to the repository as well.

Results were saved in various tables on BigQuery under newly created "Results" dataset.
