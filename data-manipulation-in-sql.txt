## [Data Manipulation in SQL](https://app.datacamp.com/learn/courses/data-manipulation-in-sql)


Data Manipulation in SQL
------------------------------
CASE statement:
- Contains a WHEN, THEN, and ELSE statement, finished with an END

CASE WHEN x = 1 THEN 'a'
    WHEN x = 2 THEN 'b'
    ELSE 'c'
    END AS new_column_name

SELECT id, home_goal, away_goal, 
        CASE WHEN home_goal > away_goal THEN 'Home Team Win'
                WHEN home_goal < away_goal THEN 'Away Team Win'
                ELSE 'Draw'
                END AS match_result
FROM match
WHERE season = '2013/2014';

SELECT
	-- Select the team long name and team API id
	team_long_name,
	team_api_id
FROM teams_germany
-- Only include FC Schalke 04 and FC Bayern Munich
WHERE team_long_name IN ('FC Schalke 04', 'FC Bayern Munich');

SELECT 
	-- Select the date of the match
	date,
	-- Identify home wins, losses, or ties
	CASE WHEN home_goal > away_goal THEN 'Home win!'
        WHEN home_goal < away_goal THEN 'Home loss :(' 
        ELSE 'Tie' END AS outcome
FROM matches_spain;

SELECT 
	m.date,
	t.team_long_name AS opponent,
    -- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Barcelona win!'
        WHEN m.home_goal < m.away_goal THEN 'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
LEFT JOIN teams_spain AS t 
ON m.awayteam_id = t.team_api_id
-- Filter for Barcelona as the home team
WHERE m.hometeam_id = 8634; 

-- Select matches where Barcelona was the away team
SELECT  
	m.date,
	t.team_long_name AS opponent,
	CASE WHEN m.away_goal > m.home_goal THEN 'Barcelona win!'
        WHEN m.away_goal < m.home_goal THEN 'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
-- Join teams_spain to matches_spain
LEFT JOIN teams_spain AS t 
ON m.hometeam_id = t.team_api_id
WHERE m.awayteam_id = 8634;

SELECT 
	date,
	CASE WHEN hometeam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END as home,
	CASE WHEN awayteam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END as away,
	-- Identify all possible match outcomes
	CASE WHEN home_goal > away_goal AND hometeam_id = 8634 THEN 'Barcelona win!'
        WHEN home_goal > away_goal AND hometeam_id = 8633 THEN 'Real Madrid win!'
        WHEN home_goal < away_goal AND awayteam_id = 8634 THEN 'Barcelona win!'
        WHEN home_goal < away_goal AND awayteam_id = 8633 THEN 'Real Madrid win!'
        ELSE 'Tie!' END AS outcome
FROM matches_spain
WHERE (awayteam_id = 8634 OR hometeam_id = 8634)
      AND (awayteam_id = 8633 OR hometeam_id = 8633);

-- Select the season and date columns
SELECT 
	season,
	date,
    -- Identify when Bologna won a match
	CASE WHEN hometeam_id = 9857 
        AND home_goal > away_goal 
        THEN 'Bologna Win'
		WHEN awayteam_id = 9857 
        AND away_goal > home_goal 
        THEN 'Bologna Win' 
		END AS outcome
FROM matches_italy;

-- Select the season, date, home_goal, and away_goal columns
SELECT 
	season,
    date,
	home_goal,
	away_goal
FROM matches_italy
WHERE 
-- Exclude games not won by Bologna
	CASE WHEN hometeam_id = 9857 AND home_goal > away_goal THEN 'Bologna Win'
		WHEN awayteam_id = 9857 AND away_goal > home_goal THEN 'Bologna Win' 
		END IS NOT NULL;

SELECT 
	c.name AS country,
    --Count games from the 2012/2013 season
	COUNT(CASE WHEN m.season = '2012/2013' 
        	THEN m.id ELSE NULL END) AS matches_2012_2013
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY c.name;

SELECT 
	c.name AS country,
    -- Count matches in each of the 3 seasons
	COUNT(CASE WHEN m.season = '2012/2013' THEN m.id END) AS matches_2012_2013,
	COUNT(CASE WHEN m.season = '2013/2014' THEN m.id END) AS matches_2013_2014,
	COUNT(CASE WHEN m.season = '2014/2015' THEN m.id END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY c.name;

SELECT 
	c.name AS country,
    -- Sum the total records in each season where the home team won
	SUM(CASE WHEN m.season = '2012/2013' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2012_2013,
 	SUM(CASE WHEN m.season = '2013/2014' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2013_2014,
        SUM(CASE WHEN m.season = '2014/2015' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY c.name;

SELECT 
	c.name AS country,
    -- Calculate the percentage of tied games in each season
	AVG(CASE WHEN m.season='2013/2014' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2013/2014' AND m.home_goal != m.away_goal THEN 0
			END) AS ties_2013_2014,
	AVG(CASE WHEN m.season='2014/2015' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2014/2015' AND m.home_goal != m.away_goal THEN 0
			END) AS ties_2013_2014
FROM country AS c
LEFT JOIN matches AS m
ON c.id = m.country_id
GROUP BY country;

SELECT 
	c.name AS country,
    -- Calculate the percentage of tied games in each season
	ROUND(AVG(CASE WHEN m.season='2013/2014' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2013/2014' AND m.home_goal != m.away_goal THEN 0
			END), 2) AS pct_ties_2013_2014,
	ROUND(AVG(CASE WHEN m.season='2014/2015' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2014/2015' AND m.home_goal != m.away_goal THEN 0
			END), 2) AS pct_ties_2014_2015
FROM country AS c
LEFT JOIN matches AS m
ON c.id = m.country_id
GROUP BY country;

Short and Simple Subqueries
----------------------------
Subquery -> A query nested inside another query

SELECT column
FROM (SELECT column FROM table) AS subquery;

- Can be in any part of a query (SELECT, FROM, WHERE, GROUP BY)
- Can return a variety of information 

Why subqueries:
- Comparing groups to summarize values (How did Liverpool compare to English Premier League average performance that year?)
- Reshaping data (What is the highest average of goals scored in the Bundesliga?)
- Combining data that cannot be joined (How do you get both the hoe and away names into a table of match results?)

- Subquery with WHERE
SELECT home_goal
FROM match
WHERE home_goal > (SELECT AVG(home_goal) FROM match);

SELECT date, hometeam_id, awayteam_id, home_goal, away_goal
FROM match
WHERE season = '2012/2013'
    AND home_goal > (SELECT AVG(home_goal) FROM match);

- Subquery with IN
SELECT team_long_name, team_short_name AS abbr
FROM team
WHERE team_api_id IN (SELECT hometeam_id FROM match WHERE country_id = 15722);

-- Select the average of home + away goals, multiplied by 3
SELECT 
	3 * AVG(home_goal + away_goal)
FROM matches_2013_2014;

SELECT 
	-- Select the date, home goals, and away goals scored
    date,
	home_goal,
	away_goal
FROM  matches_2013_2014
-- Filter for matches where total goals exceeds 3x the average
WHERE (home_goal + away_goal) >
       (SELECT 3 * AVG(home_goal + away_goal)
        FROM matches_2013_2014); 

SELECT 
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team 
-- Exclude all values from the subquery
WHERE team_api_id NOT IN 
     (SELECT DISTINCT hometeam_id  FROM match);

SELECT
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team
-- Filter for teams with 8 or more home goals
WHERE team_api_id IN
	  (SELECT hometeam_id 
       FROM match
       WHERE home_goal >= 8);

- Subquery in FROM
SELECT t.team_long_name AS team, AVG(n.home_goal) AS home_avg
FROM match AS m
LEFT JOIN team AS t
ON n.hometeam_id = t.team_api_id
WHERE season = '2011/2012'
GROUP BY team;

SELECT team, home_avg
FROM (SELECT t.team_long_name AS team, AVG(m.home_goal) AS home_avg
        FROM match AS m)
        LEFT JOIN team AS t
        ON n.hometeam_id = t.team_api_id
        WHERE season = '2011/2012'
        GROUP BY team) AS subquery
ORDER BY home_avg DESC
LIMIT 3;

- You can create multiple subqueries in one FROm statement (alias and join them)
- You can join a subquery to a table in FROM (include a joining columns in both tables)

SELECT
	-- Select country name and the count match IDs
    c.name AS country_name,
    COUNT(sub.id) AS matches
FROM country AS c
-- Inner join the subquery onto country
-- Select the country id and match id columns
INNER JOIN (SELECT country_id, id 
           FROM match
           -- Filter the subquery by matches with 10+ goals
           WHERE (home_goal + away_goal) >= 10) AS sub
ON c.id = sub.country_id
GROUP BY country_name;

SELECT
	-- Select country, date, home, and away goals from the subquery
    country,
    date,
    home_goal,
    away_goal
FROM 
	-- Select country name, date, home_goal, away_goal, and total goals in the subquery
	(SELECT c.name AS country, 
     	    m.date, 
     		m.home_goal, 
     		m.away_goal,
           (m.home_goal + m.away_goal) AS total_goals
    FROM match AS m
    LEFT JOIN country AS c
    ON m.country_id = c.id) AS subq
-- Filter by total goals scored in the main query
WHERE total_goals >= 10;

- SUbqueries in SELECT
SELECT COUNT(id) FROM match;

SELECT season, COUNT(id) AS matches, 12837 AS total_matches
FROM match
GROUP BY season;

- Subquery
SELECT season, COUNT(id) AS matches, (SELECT COUNT(id) FROM match) AS total_matches
FROM match
GROUP BY season;

Things to remember SELECT subqueries:
- Need to return a SINGLE value -> will generate an error otherwise

SELECT 
	l.name AS league,
    -- Select and round the league's total goals
    ROUND(AVG(home_goal + m.away_goal), 2) AS avg_goals,
    -- Select & round the average total goals for the season
    (SELECT ROUND(AVG(home_goal + away_goal), 2) 
     FROM match
     WHERE season = '2013/2014') AS overall_avg
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
-- Filter for the 2013/2014 season
WHERE m.season = '2013/2014'
GROUP BY league;

SELECT
	-- Select the league name and average goals scored
	l.name AS league,
	ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Subtract the overall average from the league average
	ROUND(AVG(m.home_goal + m.away_goal) -
		(SELECT AVG(home_goal + away_goal)
		 FROM match 
         WHERE season = '2013/2014'),2) AS diff
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
-- Only include 2013/2014 results
WHERE season = '2013/2014'
GROUP BY l.name;

Best practices:
- Format your queries
- Annotate your queries
- Indent your queries
- Properly filter each subquery

SELECT 
	-- Select the stage and average goals for each stage
	m.stage,
    ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Select the average overall goals for the 2012/2013 season
    ROUND((SELECT AVG(home_goal + away_goal) 
           FROM match 
           WHERE season = '2012/2013'),2) AS overall
FROM match AS m
-- Filter for the 2012/2013 season
WHERE season = '2012/2013'
-- Group by stage
GROUP BY stage;

SELECT 
	-- Select the stage and average goals from the subquery
	s.stage,
	ROUND(s.avg_goals,2) AS avg_goals
FROM 
	-- Select the stage and average goals in 2012/2013
	(SELECT
		 stage,
         AVG(home_goal + away_goal) AS avg_goals
	 FROM match
	 WHERE season = '2012/2013'
	 GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                    FROM match WHERE season = '2012/2013');

SELECT 
	-- Select the stage and average goals from s
	stage,
    ROUND(s.avg_goals,2) AS avg_goal,
    -- Select the overall average for 2012/2013
    (SELECT AVG(home_goal + away_goal) FROM match WHERE season = '2012/2013') AS overall_avg
FROM 
	-- Select the stage and average goals in 2012/2013 from match
	(SELECT
		 stage,
         AVG(home_goal + away_goal) AS avg_goals
	 FROM match
	 WHERE season = '2012/2013'
	 GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                    FROM match WHERE season = '2012/2013');


Correlated Queries, Nested Queries, and Common Table Expressions
-------------------------------------------------------------------
Correlated subquery:
- Uses values from the outer query to generate a result
- Re-run for every row generated in the final data set
- Uses for advanced joining, filtering, and evaluating data

SELECT 
	-- Select the stage and average goals from s
	stage,
    ROUND(s.avg_goals,2) AS avg_goal,
    -- Select the overall average for 2012/2013
    (SELECT AVG(home_goal + away_goal) FROM match WHERE season = '2012/2013') AS overall_avg
FROM 
	-- Select the stage and average goals in 2012/2013 from match
	(SELECT
		 stage,
         AVG(home_goal + away_goal) AS avg_goals
	 FROM match
	 WHERE season = '2012/2013'
	 GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                    FROM match AS m
                    WHERE s.stage > m.stage);

Simple subquery                                 | Correlated subquery
- Can be run independently from the main query  | Dependent on the main query to EXECUTE
- Evaluated once in the whole query             | Evaluated in loops (running time longer)

SELECT 
	-- Select country ID, date, home, and away goals from match
	main.country_id,
    date,
    main.home_goal, 
    main.away_goal
FROM match AS main
WHERE 
	-- Filter the main query by the subquery
	(home_goal + away_goal) > 
        (SELECT AVG((sub.home_goal + sub.away_goal) * 3)
         FROM match AS sub
         -- Join the main query to the subquery in WHERE
         WHERE main.country_id = sub.country_id);

SELECT 
	-- Select country ID, date, home, and away goals from match
	main.country_id,
    main.date,
    main.home_goal,
    main.away_goal
FROM match AS main
WHERE 
	-- Filter for matches with the highest number of goals scored
	(home_goal + away_goal) =
        (SELECT MAX(sub.home_goal + sub.away_goal)
         FROM match AS sub
         WHERE main.country_id = sub.country_id
               AND main.season = sub.season);

Nested subqueries:
- Subquery inside another subquery

SELECT 
    EXTRACT(MONTH FROM date) AS month, 
    SUM(m.home_goal + m.away_goal) AS total_goals, 
    SUM(m.home_goal + m.away_goal) - 
        (SELECT AVG(goals) // Outer query
         FROM (SELECT EXTRACT(MONTH FROM date) AS month,
                SUM(home_goal + away_goal) AS goals
                FROM match
                GROUP BY month)) AS avg_diff // Inner query
FROM match AS m
GROUP BY month;

SELECT 
    c.name AS country,
    (SELECT AVG(home_goal + away_goal)
    FROM match AS m
    WHERE m.country_id = c.id -- Correlates with main query
    AND id IN (SELECT id -- Begin inner subquery
                FROM match
                WHERE season = '2011/2012')) AS avg_goals
FROM country AS c
GROUP BY country;

SELECT
	-- Select the season and max goals scored in a match
	season,
    MAX(home_goal + away_goal) AS max_goals,
    -- Select the overall max goals scored in a match
   (SELECT MAX(home_goal + away_goal) FROM match) AS overall_max_goals,
   -- Select the max number of goals scored in any match in July
   (SELECT MAX(home_goal + away_goal) 
    FROM match
    WHERE id IN (
          SELECT id FROM match WHERE EXTRACT(MONTH FROM date) = 07)) AS july_max_goals
FROM match
GROUP BY season;

-- Select matches where a team scored 5+ goals
SELECT
	country_id,
    season,
	id
FROM match
WHERE home_goal >= 5 OR away_goal >= 5;

-- Count match ids
SELECT
    country_id,
    season,
    COUNT(subquery) AS matches
-- Set up and alias the subquery
FROM (
	SELECT
    	country_id,
    	season,
    	id
	FROM match
	WHERE home_goal >= 5 OR away_goal >= 5 )
    AS subquery
-- Group by country_id and season
GROUP BY country_id, season;

#Gabungan
SELECT
	c.name AS country,
    -- Calculate the average matches per season
	AVG(outer_s.matches) AS avg_seasonal_high_scores
FROM country AS c
-- Left join outer_s to country
LEFT JOIN (
  SELECT country_id, season,
         COUNT(id) AS matches
  FROM (
    SELECT country_id, season, id
	FROM match
	WHERE home_goal >= 5 OR away_goal >= 5) AS inner_s
  -- Close parentheses and alias the subquery
  GROUP BY country_id, season) AS outer_s
ON c.id = outer_s.country_id
GROUP BY country;

Common Table Expressions (CTEs)
-------------------------------
- Table declared before the main query

WITH cte AS (
    SELECT col1, col2
    FROM table
)

SELECT 
    AVG(col1) AS avg_col1
FROM cte;

SELECT 
    c.name AS country,
    COUNT(s.id) AS matches
FROM country AS c
INNER JOIN (
    SELECT country_id , id
    FROM match 
    WHERE (home_goal + away_goal) >= 10) AS s
ON c.id = s.country_id
GROUP BY country;

WITH s AS (
    SELECT country_id, id
    FROM match
    WHERE (home_goal + away_goal) >= 10
)

SELECT 
    c.name AS country,
    COUNT(s.id) AS matches
FROM country AS c
INNER JOIN s
ON c.id = s.country_id
GROUP BY country;

-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id, 
  		id
    FROM match
    WHERE (home_goal + away_goal) >= 10)
-- Select league and count of matches from the CTE
SELECT
    l.name AS league,
    COUNT(match_list.id) AS matches
FROM league AS l
-- Join the CTE to the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;

-- Set up your CTE
WITH match_list AS (
  -- Select the league, date, home, and away goals
    SELECT 
  		l.name AS league, 
     	m.date, 
  		m.home_goal, 
  		m.away_goal,
       (m.home_goal + m.away_goal) AS total_goals
    FROM match AS m
    LEFT JOIN league as l ON m.country_id = l.id)
-- Select the league, date, home, and away goals from the CTE
SELECT league, date, home_goal, away_goal
FROM match_list
-- Filter by total goals
WHERE total_goals >= 10;

-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id,
  	   (home_goal + away_goal) AS goals
    FROM match
  	-- Create a list of match IDs to filter data in the CTE
    WHERE id IN (
       SELECT id
       FROM match
       WHERE season = '2013/2014' AND EXTRACT(MONTH FROM date) = 08))
-- Select the league name and average of goals in the CTE
SELECT 
	l.name,
    AVG(match_list.goals)
FROM league AS l
-- Join the CTE onto the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;

Deciding on techniques to use:
- Considerable overlap
- These technique not identical 

Joins:
- Combine 2+ tables
- Simple operations/aggregations

Correlated subqueries:
- Match subqueries & tables 
- Avoid limits of joins
- High processing time

Multiple/nested subqueries:
- Combine multiple subqueries

CTEs:
- Organize subqueries sequentially 
- Can reference each other

Different use cases:
- Joins: 2+ tables (What is the total sales per employee?)
- Correlated subqueries: Who does each employee report to in a company?
- Multiple/nested subqueries: What is the average deal size closed by each sales representative in the quarter?
- CTEs: How did the marketing, sales, growth, engineering teams perform on key metrics in the last quarter?

--Get team names with a subquery
SELECT
	m.date,
    -- Get the home and away team names
    home.hometeam,
    away.awayteam,
    m.home_goal,
    m.away_goal
FROM match AS m
-- Join the home subquery to the match table
LEFT JOIN (
  SELECT match.id, team.team_long_name AS hometeam
  FROM match
  LEFT JOIN team
  ON match.hometeam_id = team.team_api_id) AS home
ON home.id = m.id
-- Join the away subquery to the match table
LEFT JOIN (
  SELECT match.id, team.team_long_name AS awayteam
  FROM match
  LEFT JOIN team
  -- Get the away team ID in the subquery
  ON match.awayteam_id = team.team_api_id) AS away
ON away.id = m.id;

-- Get the team names with Correlated Subqueries
SELECT
    m.date,
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.hometeam_id) AS hometeam,
    -- Connect the team to the match table
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.awayteam_id) AS awayteam,
    -- Select home and away goals
     m.home_goal,
     m.away_goal
FROM match AS m;

-- Get the team names with a CTE
WITH home AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS hometeam, m.home_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.hometeam_id = t.team_api_id),
-- Declare and set up the away CTE
away AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS awayteam, m.away_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.awayteam_id = t.team_api_id)
-- Select date, home_goal, and away_goal
SELECT 
	home.date,
    home.hometeam,
    away.awayteam,
    home.home_goal,
    away.away_goal
-- Join away and home on the id column
FROM home
INNER JOIN away
ON home.id = away.id;


Window Functions
----------------
Working with aggregate values
- Requires you to use GROUP BY with all non-aggregate columns

Window functions:
- Perform calculation on an already generated result set (a window)
- Aggregate calculations 
    - Similar to subqueries in SELECT
    - Running totals, rankings, moving average

-- Subqueries version 
SELECT 
    date, 
    (home_goal + away_goal) AS total_goals,
    (SELECT AVG(home_goal + away_goal) 
     FROM match
     WHERE season = '2013/2014') AS overall_avg
FROM match
WHERE season = '2013/2014';

-- Window function version
SELECT 
    date, 
    (home_goal + away_goal) AS total_goals,
    AVG(home_goal + away_goal) OVER() AS overall_avg
FROM match
WHERE season = '2013/2014';


-- Generate a RANK
SELECT 
    date,
    (home_goal + away_goal) AS total_goals,
    RANK() OVER(ORDER BY home_goal + away_goal DESC) AS goals_rank // Skip next ranking if there is a tie
FROM match
WHERE season = '2013/2014';

Key difference:
- Processed after the main query except for ORDER BY

SELECT 
	-- Select the id, country name, season, home, and away goals
	m.id, 
    c.name AS country, 
    m.season,
	m.home_goal,
	m.away_goal,
    -- Use a window to include the aggregate average in each row
	AVG(m.home_goal + m.away_goal) OVER() AS overall_avg
FROM match AS m
LEFT JOIN country AS c ON m.country_id = c.id;

SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank each league according to the average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal) DESC) AS league_rank  // DESC -> highest rank first
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
ORDER BY league_rank;

OVER and PARTITION BY:
- Calculate separate values for different categories
- Calculate different calculations in the same column 

AVG(home_goal) OVER(PARTITION BY season)

- How many goals were scored in each match compared to the average goals scored in that season?
SELECT 
    date,
    (home_goal + away_goal) AS total_goals,
    AVG(home_goal + away_goal) OVER(PARTITION BY season) AS season_avg
FROM match;

PARTITION BY season, country_id
SELECT 
    c.name,
    m.season,
    (home_goal + away_goal) AS total_goals,
    AVG(home_goal + away_goal) OVER(PARTITION BY season, country_id) AS season_country_avg
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id;

RANK() -> ranking, skipping next ranking if there is a tie
DENSE_RANK() -> ranking, no skipping

SELECT
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
		 ELSE 'away' END AS warsaw_location,
    -- Calculate the average goals scored partitioned by season
    AVG(home_goal) OVER(PARTITION BY season) AS season_homeavg,
    AVG(away_goal) OVER(PARTITION BY season) AS season_awayavg
FROM match
-- Filter the data set for Legia Warszawa matches only
WHERE 
	hometeam_id = 8673
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;

SELECT 
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
	-- Calculate average goals partitioned by season and month
    AVG(home_goal) OVER(PARTITION BY season, 
         	EXTRACT(MONTH FROM date)) AS season_mo_home,
    AVG(away_goal) OVER(PARTITION BY season, 
            EXTRACT(MONTH FROM date)) AS season_mo_away
FROM match
WHERE 
	hometeam_id = 8673
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;

-- Sliding window
- Perform calculations relative to the current row
- Calculate moving averages, cumulative sums, and running totals
- Can be partitioned by a column

ROWS BETWEEN <start> AND <end>

PRECEDING -> before the current row
FOLLOWING -> after the current row
UNBOUNDED PRECEDING -> from the first row
UNBOUNDED FOLLOWING -> to the last row
CURRENT ROW -> the current row

Penggunaa:
Cumulative sum: UNBOUNDED PRECEDING
Rolling sum (n baris sebelumnya): n PRECEDING
Moving average (n sebelum & n setelah): n PRECEDING AND n FOLLOWING
Sisa total dari titik tertentu: UNBOUNDED FOLLOWING

SELECT 
    date, 
    home_goal, 
    away_goal, 
    SUM(home_goal) OVER(ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total_home
FROM match
WHERE hometeam_id = 8673 AND season = '2013/2014';

SELECT 
    date, 
    home_goal, 
    away_goal, 
    SUM(home_goal) OVER(ORDER BY date ROWS BETWEEN 1 AND CURRENT ROW) AS running_total_home
FROM match
WHERE hometeam_id = 8673 AND season = '2013/2014';

SELECT 
	date,
	home_goal,
	away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date  
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total,
    AVG(home_goal) OVER(ORDER BY date
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM match
WHERE 
	hometeam_id = 9908 
	AND season = '2011/2012';

-- Kebalikan
SELECT 
	-- Select the date, home goal, and away goals
	date,
    home_goal,
    away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_total,
    AVG(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_avg
FROM match
WHERE 
	awayteam_id = 9908 
    AND season = '2011/2012';

Setting up the home team CTE:
SELECT 
	m.id, 
    t.team_long_name,
    -- Identify matches as home/away wins or ties
	CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		WHEN m.home_goal < m.away_goal THEN 'MU Loss'
        ELSE 'Tie' END AS outcome
FROM match AS m
-- Left join team on the home team ID and team API id
LEFT JOIN team AS t 
ON m.hometeam_id = t.team_api_id
WHERE 
	-- Filter for 2014/2015 and Manchester United as the home team
	season = '2014/2015'
	AND t.team_long_name = 'Manchester United';

-- Set up the home team CTE
WITH home AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
-- Set up the away team CTE
away AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
-- Select team names, the date and goals
SELECT DISTINCT
    m.date,
    home.team_long_name AS home_team,
    away.team_long_name AS away_team,
    m.home_goal,
    m.away_goal
-- Join the CTEs onto the match table
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
      AND (home.team_long_name = 'Manchester United' 
           OR away.team_long_name = 'Manchester United');


WITH home AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
-- Set up the away team CTE
away AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
		   WHEN m.home_goal < m.away_goal THEN 'MU Win' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
-- Select columns and and rank the matches by goal difference
SELECT DISTINCT
    m.date,
    home.team_long_name AS home_team,
    away.team_long_name AS away_team,
    m.home_goal, m.away_goal,
    RANK() OVER(ORDER BY ABS(home_goal - away_goal) DESC) as match_rank
-- Join the CTEs onto the match table
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
      AND ((home.team_long_name = 'Manchester United' AND home.outcome = 'MU Loss')
      OR (away.team_long_name = 'Manchester United' AND away.outcome = 'MU Loss'));

------------------
Leetcode
------------------

# Write your MySQL query statement below
SELECT p.product_id, 
       IFNULL(ROUND(SUM(p.price * u.units) / SUM(u.units), 2), 0) AS average_price
FROM 
    Prices AS p
LEFT JOIN 
    UnitsSold AS u
ON 
    p.product_id = u.product_id
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY 
    p.product_id;

-- Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).
# Write your MySQL query statement below
SELECT w1.id
FROM Weather AS w1
JOIN Weather AS w2
ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature

SELECT a.machine_id, 
       ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a, 
     Activity b
WHERE 
    a.machine_id = b.machine_id
AND 
    a.process_id = b.process_id
AND 
    a.activity_type = 'start'
AND 
    b.activity_type = 'end'
GROUP BY machine_id


ROW_NUMBER()	Memberi nomor urut unik ke setiap baris
RANK()	Memberi peringkat dengan nilai yang sama memiliki peringkat yang sama, tetapi nomor berikutnya akan meloncat
DENSE_RANK()	Mirip RANK(), tetapi tanpa loncatan
NTILE(N)	Membagi data menjadi N kelompok yang sama besar

LAG()	Mengambil nilai dari baris sebelumnya
LEAD()	Mengambil nilai dari baris berikutnya
FIRST_VALUE()	Mengambil nilai pertama dalam window
LAST_VALUE()	Mengambil nilai terakhir dalam window

SELECT id_karyawan, nama, departemen, gaji,
       ROW_NUMBER() OVER (ORDER BY gaji DESC) AS row_num,
       RANK() OVER (ORDER BY gaji DESC) AS rank_num,
       DENSE_RANK() OVER (ORDER BY gaji DESC) AS dense_rank_num
FROM Karyawan;

ROW_NUMBER() tetap unik meskipun nilai sama.
RANK() mengulang nomor untuk nilai yang sama, tetapi melompati angka berikutnya.
DENSE_RANK() juga mengulang nomor, tetapi tidak melompati angka berikutnya.


-- Return a checked_time column which contains timestamp one hour after the trip has started
SELECT Trip_ID, Start_time, Start_time + INTERVAL '1 hour' AS checked_time
FROM bike_trips;

due_date + INTERVAL '90 days' AS new_due_date

SELECT style, price
FROM wine_region 
WHERE id IN (
    SELECT wine_id
    FROM pairing
)
ORDER BY price, style
LIMIT 5

SELECT name
FROM performers
WHERE 'Reggaeton' = ANY(genre)
ORDER BY name;

SELECT COUNT(reference)
FROM (SELECT reference, date('2020-05-01') - due_date AS days_overdue) AS sub 
WHERE days_overdue > 90
