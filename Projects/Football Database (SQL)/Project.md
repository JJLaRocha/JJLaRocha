# Football Database Project

## Introduction and Context

Hello! Since a kid I always loved football and until this day I still have a great passion for it! Eventhough I watch a lot of games, I’m not really the person that knows a lot about statistics and general data about the game. What football leagues move more money with football players? Were they always the same? What football manager has the best results in theory? These are only examples, I could list a lot of all sort of questions that I am interested in knowing the answers. So in this project I will use the data I have available to answer some questions that I am quite curious about!

I gathered information about players, clubs, competitions and even transfers and, in summary, the data is spread throughout the following tables, that are presented below with the respective table schema.

### Tables Schema

Players Table
"player_id" – Primary Key
"first_name"
"last_name"
"name"
"last_season"
"current_club_id"
"country_of_birth"
"city_of_birth"
"country_of_citizenship"
"date_of_birth"
"sub_position"
"position"
"foot"
"height_in_cm"
"current_club_name"

Clubs Table
"club_id" – Primary Key
"name"
"domestic_competition_id"
"total_market_value"
"squad_size"
"average_age"
"foreigners_number"
"foreigners_percentage"
"national_team_players"
"stadium_name"
"stadium_seats"
"coach_name"
"indic"

Transfers Table
"transfer_id" – Primary Key
"player_id"
"tranfer_date"
"transfer_season"
"from_club_id"
"to_club_id"
"from_club_name"
"to_club_name"
"transfer_fee"
"market_value_in_eur"
"player_name"
"season"

Club Games Table
"game_id" – Primary Key
"club_id"
"own_goals"
"own_position"
"own_manager_name"
"opponent_id"
"opponent_goals"
"opponent_position"
"opponent_manager_name"
"hosting"

Competitions Table
"competition_id" – Primary Key
"competition_code"
"name"
"sub_type"
"type"
"country_id"
"country_name"
"domestic_league_code"
"confederation"

### Database Schema

As you can see, there are common columns between all the tables, and those common columns is what we are going to use to relate the tables with one and other (Primary Key of a certain table always find the same column as foreign key at least one other table). Here is the general schema for the database with the relationships between the tables:

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/1.png]

And here is the code I used to create the database structure inside PgAdmin:

```
transfers
-
transfer_id int pk
player_id int FK >- players.player_id
tranfer_date datetime
transfer_season varhcar(20)
from_club_id int
to_club_id int
from_club_name varchar(20)
to_club_name varchar(20)
transfer_fee int
market_value_in_eur int
player_name varchar(20)

players
-
player_id int pk
first_name varchar(20)
last_name varchar(20)
name varhcar(20)
last_season int
current_club_id int FK >- clubs.club_id
country_of_birth varchar(20)
city_of_birth varchar(20)
country_of_citizenship varchar(20)
date_of_birth datetime
sub_position varchar(20)
position varchar(20)
foot varchar(10)
height_in_cm int
current_club_name varchar(20)

clubs
-
club_id int pk FK >- club_games.club_id
name varchar(20)
domestic_competition_id int FK >- competitions.competition_id
total_market_value int
squad_size int
average_age float64
foreigners_number int
foreigners_percentage float64
national_team_players int
stadium_name varchar(30)
stadium_seats int
net_transfer_record varchar(20)
coach_name varchar(20)

club_games
-
game_id int pk
club_id int fk
own_goals int
own_position int
own_manager_name varchar(20)
opponent_id int
opponent_goals int
opponent_position int
opponent_manager_name varchar(20)
hosting varchar(10)
is_win int

competitions
-
competition_id int pk
competition_code varchar(20)
name varchar(20)
sub_type varchar(20)
type varchar(20)
country_id int
country_name varchar(20)
domestic_league_code varchar(10)
confederation varchar(20)
 ```

Now that we have the database structure, we only need to load the tables with the values we already have in excel files and the assemblance of the database is complete and we can start having fun! 

## Questions

### 1 - What was the most expensive player transfer each season?

```
SELECT
	max(market_value_in_eur),
	transfer_season
FROM
	transfers
GROUP BY	
	transfer_season
ORDER BY	
	Transfer_season;
```

Return Table:

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/2.png]

As you can see, there is a bit of an issue with the Transfer Season column, since it is not actually returning values in the right order. Let's fix that:

```
SELECT
	transfer_season,
	season
FROM (	SELECT
		transfer_season,
		CASE
			WHEN LEFT(transfer_season,1) = '9' AND RIGHT(LEFT(transfer_season,2),1) <> '9' THEN CONCAT('19',LEFT(transfer_season,2),'/',CONCAT('19',RIGHT(transfer_season,2)))
			WHEN LEFT(transfer_season,1) = '9' AND RIGHT(LEFT(transfer_season,2),1) = '9' THEN CONCAT('19',LEFT(transfer_season,2),'/',CONCAT('20',RIGHT(transfer_season,2)))
			else CONCAT('20',LEFT(transfer_season,2),'/',CONCAT('20',RIGHT(transfer_season,2))) end AS season
		FROM
			transfers
	) ;
```

Basically what we did was create a rule that says the following: whenever the first character (not actually integer since the column in varchar) is a ‘1’ or a ‘0’, you add a ‘20’ to both numbers before and after the delimiter ‘/’. When the first character is a ‘9’, the 1990’s, you add a ‘19’. The exception is for when first part of the season is 99, to which you add a ‘20’ after the delimiter, since it will be 1999/2000.

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/3.png]

This was the result, so let’s actually make it a real column of our table transfers:

```
ALTER TABLE transfers
ADD COLUMN season VARCHAR(9);

UPDATE transfers
SET season = CASE
			WHEN LEFT(transfer_season,1) = '9' AND RIGHT(LEFT(transfer_season,2),1) <> '9' THEN CONCAT('19',LEFT(transfer_season,2),'/',CONCAT('19',RIGHT(transfer_season,2)))
			WHEN LEFT(transfer_season,1) = '9' AND RIGHT(LEFT(transfer_season,2),1) = '9' THEN CONCAT('19',LEFT(transfer_season,2),'/',CONCAT('20',RIGHT(transfer_season,2)))
			else CONCAT('20',LEFT(transfer_season,2),'/',CONCAT('20',RIGHT(transfer_season,2))) end;

SELECT * FROM transfers;
```

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/4.png]

And there we go, it’s actually a part of our table now! Let’s get a bit more data of each player whose transfer was the highest in value. In the return table for the first query we saw that the column market_value_in_eur had some null values so let’s keep that in mind. Also, we will want to filter our results by a window function column so we will use a CTE:

```			
WITH CTE AS (
SELECT 
	dense_rank() OVER(PARTITION BY season ORDER BY market_value_in_eur DESC) AS rankk,
	season,
	market_value_in_eur,
	player_name,
	player_id,
	from_club_name,
	to_club_name
FROM
	transfers
WHERE
	market_value_in_eur is not null)

SELECT
	a.*,
	b.country_of_birth
FROM
	CTE AS a
INNER JOIN
	players AS b ON a.player_id=b.player_id
WHERE
	rankk=1;
```

Result table below (in excel because I copied for all rows to be visible this time), and we have answered our first question!

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/5.png]

### 2 - Are there any country/geographical or club related tendency for these most expensive players?

Let’s keep moving. I am curious to know if there are any tendencies registered in this data, in terms of country and clubs. I won’t present the following code because it will be too extense, I’ll just explain it: I’ll use this last query as a select to work as our source (from). So now, if I want to know the number of players from that list that were born in each of the countries from the list I can do something like this:


```
SELECT
	count(*),
	country_of_birth
FROM (last query)
GROUP BY
	country_of_birth
ORDER BY
	COUNT(*) DESC;
```

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/6.png]

With no surprise that most players are from Europe, with the rest of the share being of South America. Now let’s do the same for clubs from and clubs to, so basically in the last query just replace the column country from club from and club into, respectively, and we get:

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/7.png]                 

As you can see, Real Madrid really dominates when you look for the club that paid for the most expensive player of each season, whereas the clubs that sold these players have a much more balanced distribution!

### 3 - What football leagues move more money with players?
	
Now i want to know what leagues that generated the most money with transfers every season. So we need to join the tables. Let’s see top 3 leagues by season. Since we can’t use window functions like rank in the WHERE clause, we’ll create a CTE to get around it:


```
WITH CTE AS (
SELECT
	rank() OVER(PARTITION BY season ORDER BY SUM(transfer_fee) DESC) AS rankk,
	season,
	league_name,
	SUM(transfer_fee)
FROM (
	SELECT 
		DISTINCT(transfer_id),
		player_id,
		season,
		from_club_id,
		from_club_name,
		to_club_name,
		to_club_id,
		transfer_fee,
		cc.name AS league_name,
		cc.type AS league_type,
		country_name
	FROM
		transfers AS t
	LEFT JOIN
		clubs AS c ON t.from_club_id = c.club_id
	LEFT JOIN
		competitions AS cc ON c.domestic_competition_id = cc.domestic_league_code
	WHERE
		club_id IS NOT NULL AND transfer_id IS NOT NULL AND cc.type='domestic_league'
	)
GROUP BY
	season,
	league_name
HAVING
	SUM(transfer_fee) IS NOT NULL AND SUM(transfer_fee) <> 0
	)
SELECT * FROM CTE WHERE rankk <= 3 ORDER BY season,rankk;
```

Result table:

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/8.png]

We could easily see also the top 3 spending leagues by just changing the join between club and transfers tables, joining club_id from table clubs with table to_club_id from transfers table.

### 4 - What manager won more games?

Now let’s head over to the club_games table to see what interesting intel we could gather from it. Looking at its schema, one thing that I don’t like about it is the column “is_win”, which should let us know what team won but it’s very confusing and actually has a lot of incorrect values (does not have a specific value for ties, for starters.). So let’s drop that column, add a new one and fill it in a proper way:

```
ALTER TABLE club_games
DROP COLUMN is_win,
ADD COLUMN winner varchar;   

UPDATE club_games
SET winner = CASE
			WHEN own_goals > opposition_goals THEN ‘Home’
			WHEN own_goals < opposition_goals THEN ‘Away’
			WHEN own_goals = opposition_goals THEN ‘Tie’
			ELSE null
		END;
```

Now that we have added this column, let’s see what are the coaches that have more wins. Keep in mind that the same coach can be both column own_manager_name (home team) and opponent_manager_name (away team), so we have to merge the two tables. Here’s the entire query:

```
WITH CTE AS (
select 
	own_manager_name AS manager_name,
	COUNT(*) as wins
FROM
	club_games
WHERE 
	own_manager_name is not null
GROUP BY 
	own_manager_name,
	winner
HAVING
	winner = 'Home' 

UNION ALL

select 
	opponent_manager_name AS manager_name,
	COUNT(*) as wins
FROM
	club_games
WHERE 
	opponent_manager_name is not null
GROUP BY 
	opponent_manager_name,
	winner
HAVING
	winner = 'Away' 
)
SELECT
	manager_name,
	SUM(wins),
	rank() OVER (ORDER BY SUM(wins) DESC)
FROM
	CTE
group by manager_name
```

This merging of the two tables with Union clause is because the same manager name can appear in both own_manager_name and opponent_manager_name columns. So, what we want to do is consider them both, since they can win games playing home and playing away.

Result table clip:

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/9.png]

### 5 - What manager has the highest winning ratio?

The number of wins of each manager does not actually paint the whole picture since they might, and probably have, different number of games. Let’s then calculate the ratio of wins for each coach and see who is performing better. To do that, just divide the number of wins by total amount of games, which we will calculate with  a new query, that will then be used as a sub query in the join clause:

```
-- First, the query from back ago, that calculates all wins by manager
WITH CTE AS ( 
select 
	own_manager_name AS manager_name,
	COUNT(*) as wins
FROM
	club_games
WHERE 
	own_manager_name is not null
GROUP BY 
	own_manager_name,
	winner
HAVING
	winner = 'Home' 

UNION ALL

select 
	opponent_manager_name AS manager_name,
	COUNT(*) as wins
FROM
	club_games
WHERE 
	opponent_manager_name is not null
GROUP BY 
	opponent_manager_name,
	winner
HAVING
	winner = 'Away' 
), 
-- Next query is to aggregate the data, since wuth the union all you don't really accomplish that
CTE2 AS (
SELECT
	manager_name,
	SUM(wins) AS wins,
	rank() OVER (ORDER BY SUM(wins) DESC)
FROM
	CTE
group by manager_name
),
-- Same as CTE but for total games instead of wins
CTE3 AS (
SELECT
	own_manager_name AS manager,
	COUNT(*) AS games
FROM
	club_games
WHERE
	own_manager_name is not null
GROUP BY
	own_manager_name

UNION ALL

SELECT
	opponent_manager_name AS manager,
	COUNT(*) AS games
FROM
	club_games
WHERE
	opponent_manager_name is not null
GROUP BY
	opponent_manager_name
), 
-- Same as CTE2 but for total games instead of wins
CTE4 AS (
SELECT
	manager,
	SUM(games) AS games
FROM 
	CTE3
GROUP BY
	manager
ORDER BY 
	SUM(games) DESC
)
SELECT
	a.manager_name,
	a.wins,
	b.games,
	a.wins/b.games AS ratio,
	rank() OVER (ORDER BY (a.wins/b.games) DESC)
FROM	
	CTE2 AS a
JOIN
	CTE4 AS b 
ON a.manager_name = b.manager
WHERE b.games > 100
```
Result table clip:

!(image)[https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Football%20Database%20(SQL)/Imagens/10.png]
