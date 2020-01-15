# SQL MODE EASY

### Asian Population

    SELECT SUM(CITY.population)
    FROM CITY
    JOIN COUNTRY 
    ON CITY.countrycode = COUNTRY.code
    WHERE COUNTRY.continent = 'ASIA'

### African Cities

    SELECT CITY.name
    FROM CITY
    JOIN COUNTRY
    ON CITY.countrycode = COUNTRY.code
    WHERE COUNTRY.continent = 'AFRICA'

### Average Population of Each Continent

`AVG(*NUMBERS*)` : AVERAGE

`FLOOR(*NUMBER*)` : ROUND DOWN

    SELECT COUNTRY.continent, FLOOR(AVG(CITY.population))
    FROM CITY
    JOIN COUNTRY
    ON CITY.countrycode = COUNTRY.code
    GROUP BY COUNTRY.continent

### The Blunder

`CEIL(*NUMBER*)` : ROUND UP

`REPLACE(*COLUMN NAME*, *CHARACTER you want to replaced from* *CHARACTER you want to replace into*)` : REPLACE characters in column

`CONVERT(*COLUMN*, *DATA TYPE(DIGIT)*)` : CONVERT data type of column

    SELECT CEIL(AVG(salary) - AVG(REPLACE(CONVERT(salary, CHAR(4)), '0', '')))
    FROM EMPLOYEES