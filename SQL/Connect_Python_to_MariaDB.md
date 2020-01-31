# Connect Python to MariaDB

## DB 연결해서 mysql 로 데이터 불러오기

    # Python 으로 Maria DB 연결하기
    
    ##-- User defined function for MariaDB SQL query
    def query_MariaDB(query):
        import pandas as pd
        import pymysql
        from datetime import datetime
    # DB Connection # 로컬에 있는 swatchon_develpoment DB와 연결 
        conn =pymysql.connect(
             host=*'host'*
             , port=*port*
             , user=*'username'*
             , password=*'password'*
             , database=*'DB 이름'*)
    
    # start time
        start_tm = datetime.now()
    
    # Get a DataFrame
        global query_result
    
        query_result = pd.read_sql(query, conn)
        
        # Close connection
        end_tm = datetime.now()
    
        print('START TIME : ', str(start_tm))
        print('END TIME : ', str(end_tm))
        print('ELAP time :', str(end_tm - start_tm))
        conn.close()
    
        return query_result

쿼리 날리기 전에 터미널에서 명령어 

    mysql.server start

    ##-- SQL query
    query = '''
        SELECT * 
        FROM *table이름*
    
    '''
    ##-- Excute PostgreSQL SQL in Python
    *DataFrame이름* = query_MariaDB(query)
    print(*DataFrame이름*.shape)
    *DataFrame이름*.sample()