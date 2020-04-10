# SQL

현재 날짜 반환

    SELECT CURDATE()

타임존 변경

    SELECT CONVERT_TZ(p.requested_at,'+00:00','+09:00')

날짜형식변경

    SELECT DATE_FORMAT(p.requested_at, '%Y-%m-%d')

User Define Function

    -- UDF 함수 정의
    -- 함수명 : GetClassName
    -- 입력 파라미터 : @id
    -- 출력 : varchar(20)
    
    CREATE FUNCTION GetClassName(@id int) 
     RETURNS varchar(20)
    AS
    BEGIN
      DECLARE @name varchar(20)
      SELECT @name = Name FROM Class WHERE Class=@id
      
      -- 함수는 RETURN문으로 결과값 리턴
      RETURN @name
    END