# SQL MODE MEDIUM

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