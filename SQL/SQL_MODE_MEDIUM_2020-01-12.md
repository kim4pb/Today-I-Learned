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