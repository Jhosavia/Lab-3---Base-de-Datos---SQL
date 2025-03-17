# Lab-3---Base-de-Datos---SQL

# 0 *SELECT basics
    1.  Introducing the 'world' table of countries
        SELECT population FROM world
        WHERE name = 'Germany'
        
    2.  Scandinavia
        SELECT name, population FROM world
        WHERE name IN ('Sweden', 'Norway', 'Denmark');
    
    3.  Just the right size
        SELECT name, area FROM world
        WHERE area BETWEEN 200000 AND 250000
# 1 *SELECT name
    Pattern Matching Strings
    1.  SELECT name FROM world
  WHERE name LIKE 'Y%'
    
    2.  SELECT name FROM world
  WHERE name LIKE '%Y'
    
    3.  SELECT name FROM world
  WHERE name LIKE '%X%'
    
    4.  SELECT name FROM world
  WHERE name LIKE '%land'
    
    5.  SELECT name FROM world
  WHERE name LIKE 'C%ia'
    
    6.  SELECT name FROM world
  WHERE name LIKE '%oo%'
    
    7.  SELECT name FROM world
  WHERE name LIKE '%a%a%a%'
    
    8.  SELECT name FROM world
 WHERE name LIKE '_t%'
ORDER BY name
    
    9.  SELECT name FROM world
 WHERE name LIKE '%o__o%'
    
    10.  SELECT name FROM world
 WHERE name LIKE '____'
    Harder Questions
    
    11. SELECT name FROM world
 WHERE name LIKE '%' AND capital = name
    
    12. SELECT name FROM world
WHERE capital = CONCAT(name, ' City')
    
    13. SELECT capital, name FROM world
WHERE capital LIKE CONCAT('%', name, '%')
    
    14. SELECT capital, name FROM world
WHERE capital LIKE CONCAT(name, ' %')
   OR capital LIKE CONCAT(name, '-%')
    
    15. SELECT name, REPLACE(capital, name, '') AS extension
FROM world
WHERE capital LIKE CONCAT(name, '%')
AND capital != name;

# 2 *SELECT from World
    1.  SELECT name, continent, population FROM world
    
    2.  SELECT name FROM world
WHERE population >= 200000000
    
    3.  SELECT name, gdp/population FROM world
WHERE population > 200000000
    
    4.  SELECT name, population / 1000000 AS population_millions
FROM world
WHERE continent = 'South America'
    
    5.  SELECT name, population FROM world
 WHERE name IN ('France', 'Germany', 'Italy')
    
    6.  SELECT name FROM world
WHERE name LIKE '%United%'
    
    7.  SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000
    
    8.  SELECT name, population, area FROM world
WHERE (area > 3000000 AND population <= 250000000)
   OR (population > 250000000 AND area <= 3000000)
    9.  
    
    10.  SELECT name, ROUND(gdp / population, -3) AS per_capita_gdp FROM world
WHERE gdp >= 1000000000000
    
    11.  SELECT name, capital FROM world
 WHERE LENGTH(name) = LENGTH(capital)
    
    12.  SELECT name, capital FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1)
  AND name <> capital
    
    13.  SELECT name FROM world
WHERE name NOT LIKE '% %'
  AND name LIKE '%a%'    
  AND name LIKE '%e%'      
  AND name LIKE '%i%'     
  AND name LIKE '%o%'      
  AND name LIKE '%u%' 


# 3 *SELECT from Nobel
    
    1.  SELECT yr, subject, winner FROM nobel
 WHERE yr = 1950
    
    2.  SELECT winner FROM nobel
 WHERE yr = 1962 AND subject = 'literature'
    
    3.  SELECT yr, subject FROM nobel
WHERE winner = 'Albert Einstein'
    
    4.  SELECT winner FROM nobel
WHERE subject = 'peace' AND yr >= 2000
    
    5.  SELECT yr, subject, winner FROM nobel
WHERE subject = 'literature' AND yr BETWEEN 1980 AND 1989
    
    6.  SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Thomas Woodrow Wilson',
                  'Jimmy Carter', 'Barack Obama')
    
    7.  SELECT winner FROM nobel
WHERE winner LIKE 'John%';
    
    8.  SELECT yr, subject, winner FROM nobel
WHERE (subject = 'physics' AND yr = 1980)
   OR (subject = 'chemistry' AND yr = 1984)
    
    9.  SELECT yr, subject, winner FROM nobel
WHERE yr = 1980
  AND subject NOT IN ('chemistry', 'medicine')
    
    10.  SELECT yr, subject, winner FROM nobel
WHERE (subject = 'medicine' AND yr < 1910)
   OR (subject = 'literature' AND yr >= 2004)
    
    11.  SELECT yr, subject, winner FROM nobel
WHERE winner = 'Peter Grünberg'
    
    12.  SELECT yr, subject, winner FROM nobel
WHERE winner = 'Eugene O\'Neill'
    
    13.  SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner;
    
    14.  SELECT winner, subject FROM nobel WHERE yr=1984
 ORDER BY  CASE WHEN subject IN ('Physics', 'Chemistry') THEN 1 ELSE 0 END, subject, winner


# 4 *SELECT within SELECT
    1.  SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
    
    2.  SELECT name FROM world WHERE continent = 'Europe'
  AND (gdp / population) > (SELECT gdp / population FROM world WHERE name = 'United Kingdom')
    
    3.  SELECT name, continent
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    WHERE name IN ('Argentina', 'Australia')
)
ORDER BY name
    
    4.  SELECT name, population FROM world
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')
  AND population < (SELECT population FROM world WHERE name = 'Germany')
    
    5.  SELECT name, CONCAT(ROUND((population / (SELECT population FROM world WHERE name = 'Germany')) * 100), '%') AS percentage
FROM world WHERE continent = 'Europe'
    
    6.  SELECT name FROM world
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe' AND gdp IS NOT NULL)
    
    7.  SELECT continent, name, area
FROM world x
WHERE area >= ALL
    (SELECT area
     FROM world y
     WHERE y.continent = x.continent
       AND area > 0)
    
    8.  SELECT continent, name FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent)
    
    9.  SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000
)
    
    10.   SELECT w1.name, w1.continent
FROM world w1
WHERE NOT EXISTS (
    SELECT 1
    FROM world w2
    WHERE w1.continent = w2.continent
      AND w1.name <> w2.name
      AND w1.population <= 3 * w2.population
)


# 5 *SUM and COUNT
    1.  SELECT SUM(population) FROM world
    
    2.  SELECT DISTINCT continent FROM world
    
    3.  SELECT SUM(gdp) AS total_gdp_africa
FROM world
WHERE continent = 'Africa'
    
    4.  SELECT COUNT(*) AS country_count
FROM world
WHERE area >= 1000000
    
    5.  SELECT SUM(population) AS total_population
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
    
    6.  SELECT continent, COUNT(*) AS country_count
FROM world GROUP BY continent
    
    7.  SELECT continent, COUNT(*) AS country_count FROM world
WHERE population >= 10000000 GROUP BY continent
    
    8.  SELECT continent FROM world GROUP BY continent
HAVING SUM(population) >= 100000000


# 6 *JOIN
    1.  SELECT matchid, player FROM goal WHERE teamid = 'GER'
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.

    
# 7 *More JOIN operations
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 8 *Using Null
    1.  
    2.  
    3.  
    4.  
    5.  
    6.  
    7.  
    8.  
    9.  
    10.  
# 8+ *Numeric Examples
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 9- *Window function
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 9+ *COVID 19
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 9 *Self join
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 10 *Tutorial Quizzes
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 11 *Tutorial Student Records
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  


# 12 *Tutorial DDL
    1.  
    
    2.  
    
    3.  
    
    4.  
    
    5.  
    
    6.  
    
    7.  
    
    8.  
    
    9.  
    
    10.  
    
    11.  
    
    12.  
    
    13.  
    
    14.  
    
    15.  

