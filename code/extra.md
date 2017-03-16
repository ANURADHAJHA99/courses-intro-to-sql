Count of movies not rated.
```sql
SELECT COUNT(*)
FROM films
WHERE certification = 'Not Rated' OR certification IS NULL;
```

Count of movies not in English.
```sql
SELECT COUNT(*)
FROM films
WHERE language <> 'English';

```

Number of movies in black and white.
```sql
SELECT COUNT(*)
FROM films
WHERE color = 'Black and White';
```

Highest grossing per certification.
```sql
SELECT certification, MAX(gross)
FROM films
GROUP BY certification
ORDER BY max DESC;
```

Count of films in each certification bracket.
```sql
SELECT certification, COUNT(title)
FROM films
GROUP BY certification
ORDER BY count DESC;
```

Country with most R-Rated films.
```sql
SELECT country, COUNT(certification)
FROM films
WHERE certification = 'R'
GROUP BY country
ORDER BY count DESC;
```

Longest duration per year.
```sql
SELECT release_year, MAX(duration) AS max_duration
FROM films
GROUP BY release_year
ORDER BY max_duration DESC;
```

Count of films made per country.
```sql
SELECT country, COUNT(title)
FROM films
GROUP BY country
ORDER BY count DESC;
```

Count of user reviews vs critic reviews.
```sql
SELECT COUNT(num_user) AS count_users, COUNT(num_critic) AS count_critics
FROM reviews;
```

Count of actors.
```sql
SELECT COUNT(*)
FROM roles
WHERE role = 'actor';
```

Count of directors.
```sql
SELECT COUNT(*)
FROM roles
WHERE role = 'director';
```

###### HERE BE SUBQUERIES
select title, duration, release_year from films where duration = (select min(duration) from films);

select title from films where gross = (select max(gross) from films);

Get the name and duration of the longest movie made in the USA.


**Note: not really sure how to do some of these correctly, need to use review_id from films to get results from the reviews table.**

Film with most reviews.
```sql
SELECT res.* FROM (
  SELECT num_votes FROM reviews ORDER BY num_votes DESC
) res;
```

Film with least reviews.
```sql
```

Film with most votes.
```sql
```

Film with least votes.
```sql
```

Film with most facebook likes.
```sql
```

Number of days worth of film time.
```sql
```

The cost of a film per minute per film.
```sql
```

The average cost of a film per minute.
```sql
```