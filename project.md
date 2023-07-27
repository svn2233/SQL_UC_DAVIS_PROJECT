# UC Davis SQL project "Profiling and Analyzing Yelp Dataset."  
###### *This is a SQL project that I did for the course "SQL for Data Science" offered by UC Davis via Coursera.*

#### *In this project I am tasked as a data scientist to answer the given questions and provide the SQL code to arrive to the answer in part 1 of this project. In part 2 of the project I'm asked to come up with my own inferences and analysis of the data for a particular research question I want to answer.*

## Profiling and Analyzing the Yelp Dataset Coursera

### Part 1: Yelp Dataset Profiling and Understanding
#### 1. Profile the data by finding the total number of records for each of the tables below:
```
i. Attribute table =10000
ii. Business table =10000
iii. Category table =10000
iv. Checkin table =10000
v. elite_years table =10000
vi. friend table = 10000
vii. hours table =10000
viii. photo table = 10000
ix. review table = 10000
x. tip table = 10000
xi. user table =10000
```
#### 2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key.

```
i. Business = 10000 (id)
ii. Hours = 1562 (business_id)
iii. Category = 2643 (business_id)
iv. Attribute = 1115(business_id)
v. Review = 10000
vi. Checkin = 493 (business_id)
vii. Photo = 10000
viii. Tip = 3979 (business_id), (537 (user_id)
ix. User =  10000 (id)
x. Friend = 11 (user_id)
xi. Elite_years = 2780 (user_id)
```
##### *Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.*	

#### 3. Are there any columns with null values in the Users table? Indicate "yes," or "no."

##### Answer: NO, there is no columns with null values in user table 
```
SQL code used to arrive at answer:

SELECT *
FROM user
WHERE id IS NULL OR
      name IS NULL OR
      review_count IS NULL OR
      yelping_since IS NULL OR
      useful IS NULL OR
      cool IS NULL OR
      fans IS NULL OR
      average_stars IS NULL OR
      compliment_hot IS NULL OR
      compliment_more IS NULL OR
      compliment_profile IS NULL OR
      compliment_cute IS NULL OR
      compliment_list IS NULL OR
      compliment_note IS NULL OR
      compliment_plain IS NULL OR
      compliment_cool IS NULL OR
      compliment_funny IS NULL OR
      compliment_writer IS NULL OR
      compliment_photos IS NULL
```
	
#### 4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:
```
 i. Table: Review, Column: Stars
	
		min:	1	max:	5	avg:3.7082
		
	ii. Table: Business, Column: Stars
	
		min:	1	max:	5	avg:3.6549
		
	
	iii. Table: Tip, Column: Likes
	
		min:	0	max:	2	avg:0.0144
		
	
	iv. Table: Checkin, Column: Count
	
		min:	1	max:	53	avg:1.9414
		
	
	v. Table: User, Column: Review_count
	
		min:	0	max:	2000	avg:24.2995
```
		
#### 5. List the cities with the most reviews in descending order:
```
SQL code used to arrive at answer:

SELECT sum(review_count) AS total_reviews, city
FROM business
GROUP BY city
ORDER BY total_reviews DESC;
```
	
##### *Copy and Paste the Result Below:*
```
+---------------+-----------------+
| total_reviews | city            |
+---------------+-----------------+
|         82854 | Las Vegas       |
|         34503 | Phoenix         |
|         24113 | Toronto         |
|         20614 | Scottsdale      |
|         12523 | Charlotte       |
|         10871 | Henderson       |
|         10504 | Tempe           |
|          9798 | Pittsburgh      |
|          9448 | Montréal        |
|          8112 | Chandler        |
|          6875 | Mesa            |
|          6380 | Gilbert         |
|          5593 | Cleveland       |
|          5265 | Madison         |
|          4406 | Glendale        |
|          3814 | Mississauga     |
|          2792 | Edinburgh       |
|          2624 | Peoria          |
|          2438 | North Las Vegas |
|          2352 | Markham         |
|          2029 | Champaign       |
|          1849 | Stuttgart       |
|          1520 | Surprise        |
|          1465 | Lakewood        |
|          1155 | Goodyear        |
+---------------+-----------------+
(Output limit exceeded, 25 of 362 total rows shown)
```
	
#### 6. Find the distribution of star ratings to the business in the following cities:

##### i. Avon
```
SQL code used to arrive at answer:

SELECT stars AS star_rating, COUNT(stars) AS count
FROM business
WHERE city='Avon'
GROUP BY stars;
```

##### *Copy and Paste the Resulting Table Below (2 columns – star rating and count):*
```
+-------------+-------+
| star_rating | count |
+-------------+-------+
|         1.5 |     1 |
|         2.5 |     2 |
|         3.5 |     3 |
|         4.0 |     2 |
|         4.5 |     1 |
|         5.0 |     1 |
+-------------+-------+
```

##### ii. Beachwood
```
SQL code used to arrive at answer:
SELECT stars AS star_rating, COUNT(stars) AS count
FROM business
WHERE city='Beachwood'
GROUP BY stars;
```
##### *Copy and Paste the Resulting Table Below (2 columns – star rating and count):*
```
+-------------+-------+
| star_rating | count |
+-------------+-------+
|         2.0 |     1 |
|         2.5 |     1 |
|         3.0 |     2 |
|         3.5 |     2 |
|         4.0 |     1 |
|         4.5 |     2 |
|         5.0 |     5 |
+-------------+-------+
```

#### 7. Find the top 3 users based on their total number of reviews:
		
```
SQL code used to arrive at answer:

SELECT name, SUM(review_count) AS total_count
FROM user
GROUP BY id
ORDER BY total_count DESC
LIMIT 3;
```	
##### *Copy and Paste the Result Below:*
```
+--------+-------------+
| name   | total_count |
+--------+-------------+
| Gerald |        2000 |
| Sara   |        1629 |
| Yuri   |        1339 |
+--------+-------------+
```

#### 8. Does posing more reviews correlate with more fans?

##### Please explain your findings and interpretation of the results:
##### *Posing more reviews does correlate with more fans. As the more reviews users get they tend to have more fans.*
```
SELECT range AS fans_range, 
       COUNT(*) AS num_user, 
       AVG(review_count) AS avg_num_review,     
       AVG(fans) AS avg_num_fans
FROM (SELECT CASE WHEN fans BETWEEN 0 AND 9 THEN '0 - 9'
                  WHEN fans BETWEEN 10 AND 99 THEN '10 - 99'
                  ELSE '100-1000' END AS range,
             review_count, 
             fans
      FROM user) AS subtable
GROUP BY subtable.range;	
```
```
+------------+----------+----------------+----------------+
| fans_range | num_user | avg_num_review |   avg_num_fans |
+------------+----------+----------------+----------------+
| 0 - 9      |     9690 |  15.0085655315 | 0.447265221878 |
| 10 - 99    |      294 |  283.326530612 |  25.5986394558 |
| 100-1000   |       16 |          891.5 |         189.75 |
+------------+----------+----------------+----------------+
```
	
#### 9. Are there more reviews with the word "love" or with the word "hate" in them?

##### *Answer: There is more reviews with the word "love"*
```
SQL code used to arrive at answer:

SELECT reaction, count(*) AS count
FROM (SELECT CASE WHEN LOWER(text) LIKE '%love%' THEN 'love'
                  WHEN LOWER(text) LIKE '%hate%' THEN 'hate' 
                  ELSE NULL END AS reaction FROM review)
GROUP BY reaction
ORDER BY count DESC;
```
```
+----------+-------+
| reaction | count |
+----------+-------+
|     None |  8042 |
|     love |  1780 |
|     hate |   178 |
+----------+-------+
```
	
#### 10. Find the top 10 users with the most fans:
```
SQL code used to arrive at answer:

SELECT name, fans
FROM user
ORDER BY fans DESC
LIMIT 10;	
```
##### *Copy and Paste the Result Below:*
```
+-----------+------+
| name      | fans |
+-----------+------+
| Amy       |  503 |
| Mimi      |  497 |
| Harald    |  311 |
| Gerald    |  253 |
| Christine |  173 |
| Lisa      |  159 |
| Cat       |  133 |
| William   |  126 |
| Fran      |  124 |
| Lissa     |  120 |
+-----------+------+
```
	
## Part 2: Inferences and Analysis

#### 1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.
```
The city I chose is Las Vegas and the category I chose is Shopping
	
i. Do the two groups you chose to analyze have a different distribution of hours?

SQL Code:
SELECT CASE WHEN stars >= 4.0 THEN '4-5 stars'
            WHEN stars >= 2.0 THEN '2-3 stars'
            ELSE 'below 2' END AS 'STARS',               
       COUNT(DISTINCT b.id) AS id_count,            
       COUNT(h.hours) AS open_days_total AS number of opening_days        
       COUNT(h.hours) / COUNT(DISTINCT b.id)  AS open_days_avg
FROM business b
     INNER JOIN hours h ON b.id = h.business_id
     INNER JOIN category c ON b.id = c.business_id
WHERE city = 'Las Vegas' AND category ='Shopping'
GROUP BY STARS;
```
```
Result:
+-----------+----------+-----------------+---------------+
| STARS     | id_count | open_days_total | open_days_avg |
+-----------+----------+-----------------+---------------+
| 2-3 stars |        2 |              13 |           6.5 |
| 4-5 stars |        2 |              12 |           6.0 |
+-----------+----------+-----------------+---------------+
```
According to the data there seems to be no big difference in opening days between the 2 groups

```
ii. Do the two groups you chose to analyze have a different number of reviews?

SQL code:
SELECT CASE WHEN stars >= 4.0 THEN '4-5 stars'
            WHEN stars >= 2.0 THEN '2-3 stars'
            ELSE 'below 2' END AS 'STARS',               
       COUNT(DISTINCT b.id) AS id_count,            
       SUM(b.review_count) AS review_count_total,
       SUM(b.review_count)/COUNT(DISTINCT b.id)  AS review_count_avg
FROM business b
INNER JOIN category c ON b.id = c.business_id
WHERE city = 'Las Vegas' AND category ='Shopping'
GROUP BY STARS;
```
```
Results:
+-----------+----------+--------------------+------------------+
| STARS     | id_count | review_count_total | review_count_avg |
+-----------+----------+--------------------+------------------+
| 2-3 stars |        2 |                 17 |              8.5 |
| 4-5 stars |        2 |                 36 |             18.0 |
+-----------+----------+--------------------+------------------+      
```
According to the data the two groups have different number of reviews with 2-3 stars having 17 reviews and 4-5 stars having 36 reviews. This shows that 4-5 stars businesses has basically double the reviews of 2-3 star businesses.  
```
iii. Are you able to infer anything from the location data provided between these two groups? Explain.

SQL code used for analysis:

SELECT CASE WHEN stars >= 4.0 THEN '4-5 stars'
            WHEN stars >= 2.0 THEN '2-3 stars'
            ELSE 'below 2' END AS 'STARS',
       b.neighborhood,
       b.address,
       b.postal_code
FROM business b 
INNER JOIN category c ON b.id = c.business_id
WHERE city = 'Las Vegas' AND category ='Shopping'
ORDER BY STARS;
```
```
Result:

+-----------+--------------+------------------------+-------------+
| STARS     | neighborhood | address                | postal_code |
+-----------+--------------+------------------------+-------------+
| 2-3 stars | Eastside     | 3808 E Tropicana Ave   | 89121       |
| 4-5 stars |              | 3555 W Reno Ave, Ste F | 89118       |
+-----------+--------------+------------------------+-------------+
According to the data, businesses that score 4-5 stars are located away from E Tropicana Ave and businesses that score 2-3 stars are in the Eastside neighborhood on E tropicana Ave.
```

### 2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer.
		
```
i. Difference 1: Their is a difference number of business open vs closed. According to the data the number of open business is bigger than closed ones
     
ii. Difference 2: Their is a difference of number of review and average stars when it comes to open and closed business. It shows that the open businesses have more reviews and better average stars.

SQL code used for analysis:

SELECT is_open, 
       COUNT(distinct business.id) AS num_business, 
       COUNT(distinct review.id) AS num_review,
       ROUND(AVG(review.stars),2) AS avg_stars
FROM business b
JOIN review r ON b.id = r.business_id
GROUP BY is_open;
```
```
Result:

+---------+--------------+------------+---------------+
| is_open | num_business | num_review |     avg_stars |
+---------+--------------+------------+---------------+
|       0 |           61 |         71 |     3.65      |
|       1 |          446 |        565 |     3.76      |
+---------+--------------+------------+---------------+
```
	
### 3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.

Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on.These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. Provide answers, in-line, to all of the following:

#### i. Indicate the type of analysis you chose to do:
* The analysis I chose is finding the most successful category of business.
      
#### ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:
* To conduct my analysis, I compared each category based on their average star rating and average opening rate. To ensure greater accuracy, I focused on categories with more than 10 businesses.
* From the data presented below, it is evident that the most successful category is "Local Services," boasting an average star rating of 4.21 and an average opening rate of 0.83. A noticeable trend emerges, indicating that higher star ratings are associated with higher opening rates, while lower-rated categories tend to experience closures more frequently, resulting in lower average openings.

```
iii. Output of your finished dataset:

+------------------------+--------------+-----------+------------+
| category               | num_business | avg_stars | avg_is_open |
+------------------------+--------------+-----------+------------+
| Local Services         |           12 |      4.21 |       0.83 |
| Health & Medical       |           17 |      4.09 |       0.94 |
| Home Services          |           16 |       4.0 |       0.94 |
| Shopping               |           30 |      3.98 |       0.83 |
| Beauty & Spas          |           13 |      3.88 |       0.92 |
| American (Traditional) |           11 |      3.82 |       0.73 |
| Food                   |           23 |      3.78 |       0.87 |
| Bars                   |           17 |       3.5 |       0.65 |
| Nightlife              |           20 |      3.48 |        0.6 |
| Restaurants            |           71 |      3.46 |       0.75 |
+------------------------+--------------+-----------+------------+
```   
         
##### iv. Provide the SQL code you used to create your final dataset:
```
SELECT c.category, 
       COUNT(b.id) AS num_business, 
       ROUND(AVG(b.stars),2) AS avg_stars, 
       ROUND(AVG(b.is_open),2) AS avg_is_open
FROM business 
INNER JOIN category ON b.id = c.business_id
GROUP BY category
HAVING num_business > 10
ORDER BY avg_stars DESC, avg_is_open DESC;
```
