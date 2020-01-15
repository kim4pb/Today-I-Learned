# SQL HackerRank MEDIUM

### New Companies

    		SELECT c.company_code, c.founder
    		     , COUNT(DISTINCT(l.lead_manager_code))
    		     , COUNT(DISTINCT(l.senior_manager_code))
    		     , COUNT(DISTINCT(l.manager_code))
    		     , COUNT(DISTINCT(l.employee_code))
    			FROM Company c
    INNER JOIN EMPLOYEE l
    				ON c.company_code = l.company_code
    	GROUP BY c.company_code, c.founder
    	ORDER BY c.company_code

### Ollivander's Inventory(푸는중)

    -- SELECT w.id, p.age, w.coins_needed, w.power
    -- FROM Wands w
    -- INNER JOIN Wands_Property p
    -- ON w.code = p.code
    -- WHERE p.is_evil = 0
    -- GROUP BY w.power, p.age
    -- ORDER BY w.power DESC, p.age DESC
    
    -- 같은 power, age 인 경우 더 싼 것만 남긴다
    -- SELECT p.age
    --      , MIN(w.coins_needed) AS mininum_coins_needed
    --      , w.power
    -- FROM wands w
    -- INNER JOIN Wands_property p
    -- ON w.code = p.code
    -- WHERE p.is_evil = 0
    -- GROUP BY w.power, p.age
    -- ORDER BY w.power DESC, p.age DESC
    -- INNER JOIN 
    
    -- power age 젤 큰 항목보다 비싼 것 전부 날리기
    -- 그 다음 항목보다 비싼 것 전부 날리기
    -- SELECT w.power, p.age, w.coins_needed
    -- FROM wands w
    -- INNER JOIN wands_property p
    -- ON w.code = p.code
    -- WHERE p.is_evil = 0 AND w.power = MAX(w.power) AND p.age = MAX(p.age)
    
    SELECT a.min_coins
    FROM
    (SELECT w.power
         , p.age
         , MIN(w.coins_needed) AS min_coins
    FROM wands w
    INNER JOIN Wands_property p
    ON w.code = p.code
    WHERE p.is_evil = 0
    GROUP BY w.power, p.age
    ORDER BY w.power DESC, p.age DESC
    LIMIT 1) a
    
    SELECT b.id, a.age, a.min, a.power
    FROM (
    SELECT p.age, MIN(w.coins_needed)AS min, w.power
    FROM wands w
    INNER JOIN wands_property p
    ON w.code = p.code
    WHERE p.is_evil = 0
    GROUP BY p.age, w.power
    -- ORDER BY w.power DESC, p.age DESC 
    ) a
    INNER JOIN wands b
    ON a.power = b.power AND a.min = b.coins_needed
    ORDER BY a.power DESC, a.age DESC

2020-01-14 without Window Function

왜 MAX(w.power) 가 되야하는지 모르겠음...

    SELECT b.id
         , a.age
         , a.min
         , a.power
    FROM (
    SELECT wp.age age
         , MIN(w.coins_needed) min
         , MAX(w.power) power
    FROM Wands w
    INNER JOIN Wands_Property wp
    ON w.code = wp.code
    WHERE wp.is_evil = 0
    GROUP BY w.power, wp.age ) a
    
    INNER JOIN (
    SELECT w2.id id
         , wp2.age age
         , w2.coins_needed coins_needed
         , w2.power power
    FROM Wands w2
    INNER JOIN Wands_Property wp2
    ON w2.code = wp2.code
    WHERE wp2.is_evil = 0) b
    ON a.age = b.age 
    AND a.min = b.coins_needed
    AND a.power = b.power
    ORDER BY a.power DESC, a.age DESC

2020-01-14 with Window Function (Oracle)

    SELECT a.id
         , a.age
         , a.coins_needed
         , a.power
    FROM (
        SELECT w.id
         , w.coins_needed
         , wp.age
         , w.power
         , row_number() OVER (PARTITION BY age, power ORDER BY w.coins_needed ASC) row_num
    FROM Wands w
    INNER JOIN Wands_Property wp
    ON w.code = wp.code
    WHERE wp.is_evil = 0) a
    WHERE row_num = 1
    ORDER BY a.power DESC, a.age DESC
    ;

Discussion with window function

    SELECT A.id, A.age, A.coins_needed, A.power 
    FROM 
        (SELECT w1.id as id
    				  , age
    				  , coins_needed
    				  , power
    					, row_number() OVER(PARTITION BY age, power 
    											   ORDER BY coins_needed asc) as rownum FROM wands w1 
    		inner join wands_property w2 
    		on w1.code = w2.code
    		where is_evil = 0) A
    where A.rownum = 1
    order by power desc, age desc;

Discussion without window function

    SELECT t2.id
    		 , t.age
    		 , t.min_coin
    		 , t.power 
    FROM (
    SELECT wp.age age 
     		 , MIN(w.coins_needed) min_coin 
    		 , MAX(w.power) power  
    	FROM Wands w, Wands_Property wp
     WHERE wp.code = w.code 
    	 AND wp.is_evil = 0  
    GROUP BY wp.age, w.power
    ) AS t, 
    
    (
    SELECT w2.id id
     		 , wp2.age age
    		 , w2.coins_needed min_coin
    		 , w2.power power 
    	FROM Wands w2, Wands_Property wp2
     WHERE wp2.code = w2.code AND wp2.is_evil = 0
    ) AS t2 
    
    WHERE t2.age = t.age 
    	AND t2.min_coin = t.min_coin 
    	AND t2.power = t.power
    ORDER BY Bt.power DESC, t.age DESC;

### Top Competitors

    		SELECT h.hacker_id, h.name
    			FROM challenges c
    INNER JOIN submissions s
    				ON c.challenge_id = s.challenge_id
    INNER JOIN Hackers h
    				ON s.hacker_id = h.hacker_id
    INNER JOIN difficulty d
    				ON c.difficulty_level = d.difficulty_level
    		 WHERE d.score = s.score
    	GROUP BY h.hacker_id, h.name
    		HAVING COUNT(h.hacker_id) > 1
    	ORDER BY COUNT(h.hacker_id) DESC, h.hacker_id ASC

### Contest Leaderboard

예전에 입력해둔 코드

    SELECT df.hacker_id, h.name, sum(df.scr)
    FROM 
        (
        SELECT s.hacker_id, s.challenge_id, MAX(s.score) AS scr
        FROM Submissions s
        GROUP BY s.hacker_id, s.challenge_id
        ) AS df INNER JOIN hackers AS h ON h.hacker_id = df.hacker_id
    
    GROUP BY df.hacker_id, h.name
    HAVING SUM(df.scr) > 0
    ORDER BY SUM(df.scr) DESC, df.hacker_id

2020-01-15

    	SELECT a.hacker_id, a.name, SUM(score) AS total_score
    		FROM (
    					SELECT h.hacker_id, h.name, s.challenge_id, MAX(score) score
    						FROM hackers h
    			INNER JOIN submissions s
    							ON h.hacker_id = s.hacker_id
    				GROUP BY h.hacker_id, h.name, s.challenge_id
    					) a
    GROUP BY a.hacker_id, a.name
    	HAVING total_score > 0
    ORDER BY total_score DESC, a.hacker_id ASC

### Challenges

예전에 써둔 코드

    SELECT c.hacker_id, h.name, COUNT(c.challenge_id) as C
    FROM challenges c JOIN hackers h 
    ON h.hacker_id = c.hacker_id
    GROUP BY c.hacker_id, h.name
    HAVING C = (SELECT MAX(cnt)
    FROM (
        SELECT hacker_id, COUNT(challenge_id) cnt
    FROM challenges
    GROUP BY hacker_id
    ) df)
    
    UNION
    
    SELECT c.hacker_id, h.name, COUNT(c.challenge_id) as C
    FROM challenges c JOIN hackers h ON h.hacker_id = c.hacker_id
    GROUP BY c.hacker_id, h.name
    HAVING (
        SELECT COUNT(cnt) 
        FROM (
            SELECT hacker_id, COUNT(challenge_id) cnt
            FROM challenges
            GROUP BY hacker_id
        ) df2
    ) = 1

2020-01-05 Trying

    SELECT h.hacker_id, h.name, total_number_of_challenges
    FROM (
        SELECT h.hacker_id, h.name, COUNT(c.challenge_id) AS total_number_of_challenges
    FROM hackers h
    INNER JOIN challenges c
    ON h.hacker_id = c.hacker_id
    GROUP BY h.hacker_id, h.name
    ORDER BY total_number_of_challenges DESC, hacker_id
    ) a
    INNER JOIN ( 
    SELECT total_number_of_challeges, COUNT(hacker_id) AS number_of_hackers
    FROM a
    GROUP BY total_number_of_challenges
    HAVING total_number_of_challenges > number_of_hackers
    ) b
    ON a.total_number_of_challenges = b.total_number_of_challenges
    ORDER BY total_number_of_challenges DESC