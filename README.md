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

## Result

# 3) Years that video game critics loved

## Result

# 4) Was 1982 really that great?

## Result

# 5) Years that dropped off the critics' favorites list

## Result

# 6) Years video game players loved

## Result

# 7) Years that both players and critics loved

## Result

# 8) Sales in the best video game years

## Result
