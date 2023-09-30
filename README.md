# Exploring postgresql
## The CSV Files
1) game_reviews <br>
2) game_sales<br>
3) game_sales_data<br>
4) top_critic_scores<br>
5) top_critic_scores_more_than_four_games<br>
6) top_user_scores_more_than_four_games<br>


# 1) The ten best-selling video games
![1a](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/312fc574-4625-433b-ba5c-43fe3e1f7e04)
## SQL Code
```python
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10
```

## Result
![1b](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/2b95e3b1-c476-45f9-a743-d7f7ac08f09c)

# 2) Missing review scores
## SQL Code
```python
SELECT COUNT(*)
FROM game_sales AS g
LEFT JOIN reviews AS r
ON g.game = r.game
WHERE r.critic_score IS NULL 
AND r.user_score IS NULL
```

## Result
![2](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/56f89b8f-b7af-4642-812c-489e7c403ba8)


# 3) Years that video game critics loved
## SQL Code
```python
SELECT g.year,
        ROUND(AVG(r.critic_score),2) AS avg_critic_score
FROM game_sales AS g
INNER JOIN reviews AS r
ON g.game = r.game
GROUP BY g.year
ORDER BY avg_critic_score DESC
LIMIT 10
```
## Result
![3](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/2ac73707-51d3-4302-bb59-a0a003b7e09c)


# 4) Was 1982 really that great?
## SQL Code
```python
SELECT g.year,
        ROUND(AVG(r.critic_score),2) AS avg_critic_score,
        COUNT(*) AS num_games
FROM game_sales AS g
INNER JOIN reviews AS r
ON g.game = r.game
GROUP BY g.year
HAVING COUNT(*) > 4
ORDER BY avg_critic_score DESC
LIMIT 10
```
## Result
![4](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/7cf11834-0434-4c0b-a21a-188082ea8889)

# 5) Years that dropped off the critics' favorites list
![5a](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/b57bf6c8-8165-4def-8971-e021cb3c5aa7)

## SQL Code
```python
SELECT year, 
        avg_critic_score
FROM top_critic_years
EXCEPT 
SELECT year, 
        avg_critic_score
FROM top_critic_years_more_than_four_games
ORDER BY avg_critic_score DESC
```
## Result
![5b](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/f0acaf73-2a20-49f1-84f1-af7a9357923b)

# 6) Years video game players loved
## SQL Code
```python
SELECT g.year,
        ROUND(AVG(r.user_score),2) AS avg_user_score,
        COUNT(*) AS num_games
FROM game_sales AS g
INNER JOIN reviews AS r
ON g.game = r.game
GROUP BY g.year
HAVING COUNT(*) > 4
ORDER BY avg_user_score DESC
LIMIT 10
```
## Result
![6](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/cbb3e0d8-f141-43bc-88f2-333749898622)

# 7) Years that both players and critics loved
![7a](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/c934d411-336f-4bdf-8b6a-18b7c25f3af9)

## SQL Code
```python
SELECT year
FROM top_critic_years_more_than_four_games
INNER JOIN top_user_years_more_than_four_games
USING (year)
```

## Result
![7b](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/0a5022d0-31fc-4d0a-a772-d4bd22de8af1)

# 8) Sales in the best video game years
## SQL Code
```python
SELECT year,
    SUM(games_sold) AS total_games_sold
FROM game_sales
WHERE year IN
    (SELECT year
    FROM top_critic_years_more_than_four_games
    INNER JOIN top_user_years_more_than_four_games
    USING (year))
GROUP BY year
ORDER BY total_games_sold DESC
```

## Result
![8](https://github.com/UKVeteran/Exploring-postgresql/assets/39216339/faf526ff-cc49-42c6-8411-3a4698569ff9)

