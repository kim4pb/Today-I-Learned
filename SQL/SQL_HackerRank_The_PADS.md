# SQL HackerRank The PADS

SELECT name
        , CASE WHEN occupation = 'Doctor' THEN '(D)'
                WHEN occupation = 'Singer' THEN '(S)'
                WHEN occupation = 'Actor' THEN '(A)'
                WHEN occupation = 'Professor' THEN '(P)'
                ELSE NULL END AS occupation_for_short
    FROM Occupations
    ORDER BY name

    SELECT CONCAT('There are a total of ', COUNT(*), ' ', occupation, 's.')
    FROM Occupations
    WHERE occupation = 'Doctor'

2020-01-15 pass

`CONCAT( *COLUMN*, *'string'*)` : CONCAT columns and strings

`CONCAT( *'string', 'symbols not string'* COLLATE utf8_general_ci )` : CONCAT with symbols not string

`LOWER(*string*)`: string into lowercase

    (SELECT CONCAT(name, '(', o_short, ')' COLLATE utf8_general_ci) AS name_o
     FROM (
         SELECT name
         , CASE WHEN occupation = 'Doctor' THEN 'D'
                WHEN occupation = 'Professor' THEN 'P'
                WHEN occupation = 'Actor' THEN 'A' 
                WHEN occupation = 'Singer' THEN 'S'
                ELSE NULL
                END o_short
    FROM occupations
     ) b
    )
    
    UNION ALL
    
    (SELECT CONCAT('There are a total of ', occupation_count, ' ', LOWER(occupation), 's.')
    FROM
    (SELECT occupation
         , COUNT(name) occupation_count
    FROM occupations
    GROUP BY occupation
    ORDER BY occupation_count ASC, occupation ASC) a)
    ORDER BY name_o ASC