1.
Download a sample IPL match data CSV file and load it into a new SQL table called ipl_matches using your preferred SQL tool (MySQL Workbench, DBeaver, or Azure Data Studio).
***SOLUTION***
  CREATE TABLE ipl_matches (
    match_id INT,
    match_date DATE,
    season INT,
    team1 VARCHAR(50),
    team2 VARCHAR(50),
    toss_winner VARCHAR(50),
    match_winner VARCHAR(50),
    venue VARCHAR(100)
);

LOAD DATA INFILE '/path/ipl_matches.csv'
INTO TABLE ipl_matches
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
2.
Write a SQL query to select all matches where the team 'Mumbai Indians' played, then export the query results as a CSV file named mi_matches.csv.

***SOLUTION***
SELECT *
FROM ipl_matches
WHERE team1 = 'Mumbai Indians'
   OR team2 = 'Mumbai Indians';
3.
Connect Microsoft Excel to your SQL database and import the ipl_matches table. Create a simple table in Excel that shows the total matches played by each team.

***SOLUTION***
Step A: Connect Excel to SQL
Excel → Data → Get Data → From Database → From MySQL/SQL Server
Select table: ipl_matches
Step B: Create pivot-style table in Excel

If using Power Query / Pivot Table:

Fields:

Rows: team1, team2 (or unpivot first)
Values: Count of matches
Better SQL (recommended for Excel import):
SELECT team1 AS team, COUNT(*) AS matches_played
FROM ipl_matches
GROUP BY team1

UNION ALL

SELECT team2 AS team, COUNT(*) AS matches_played
FROM ipl_matches
GROUP BY team2;

Then create Pivot Table in Excel:

Rows → team
Values → SUM(matches_played)
4.
Connect Power BI Desktop to your SQL database, import the ipl_matches table, and create a pivot chart showing the number of wins for each team.
***SOLUTION***
4. Power BI pivot chart (wins per team)
Step A: Connect Power BI
Home → Get Data → SQL Server / MySQL
Select ipl_matches
Step B: Create measure (optional DAX)
Wins = COUNT(ipl_matches[match_winner])
Step C: Visual (Pivot Chart)
Axis: match_winner
Values: Count of match_id
Result:
Bar chart showing number of wins per team
Top teams like Mumbai Indians, CSK, etc.
5.
Use Python (with pandas and sqlalchemy) to read the mi_matches.csv file, filter matches where 'Mumbai Indians' won, and insert those rows into a new SQL table called mi_wins.<br><br><em><strong>Hint:</strong> Use pandas.read_csv(), DataFrame filtering, and to_sql() for this task.</em>

***SOLUTION***
import pandas as pd
from sqlalchemy import create_engine

# Load CSV
df = pd.read_csv("mi_matches.csv")

# Filter matches where Mumbai Indians won
mi_wins = df[df['match_winner'] == 'Mumbai Indians']

# Create DB connection (example MySQL)
engine = create_engine("mysql+pymysql://root:password@localhost/ipl_db")

# Insert into new table
mi_wins.to_sql("mi_wins", con=engine, if_exists="replace", index=False)

print("MI wins inserted successfully!")

