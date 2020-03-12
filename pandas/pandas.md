# pandas

# quantile

    series.quantile(0.90)

quantile 역함수 inverse function

    from scipy import stats
    stats.percentileofscore(data_range, input_data)

DataFrame 수치정보 요약

*`DataFrame*.describe()`

*`DataFrame*.describe(percentiles=*percentile_list*)` : 보고싶은 퍼센티지를 지정해줄 수 있음

# contain strings

`*string*.isin( *series* )` : string 이 series 안에 포함되어 있는지 ( `True` `False` ) 출력해준다 

`cf.` string 이 series 안에 포함되어 있지 않은 경우만 출력하고 싶으면

`*string*.isin(*series*) == False`

`*series*.str.contains(*string*)` : string 이 series 안에 포함되어 있는지 ( `True` `False` ) 출력해준다

string concat

`''.join(*list*)`

# Datetime, Timestamp, Timedelta

timedelta to histogram

    series_of_timedelta / pd.Timedelta(hours=1)
    series_of_timedelta / pd.Timedelta(minutes=1)

weekday name

    *datetime*.dt.year
    *datetime*.dt.month
    *datetime*.dt.day
    *datetime*.dt.dayofweek # 0, 1, 2, 3, 4, 5, 6 ( 0 : Monday, 1 : Tuesday )
    *datetime*.dt.weekday_name # e.g. Tuesday

today variable

    today = pd.Timestamp("today").strftime("%Y-%m-%d")

# Type

check if NoneType

    x == type(None)

NoneType Object 다루기

    for i in event_applications_for_merge['timestamps']:
        if isinstance(i, str):
            if 'step4' in literal_eval(i).keys():
                steps.append('step4')
            elif 'step3' in literal_eval(i).keys():
                steps.append('step3')
            elif 'step2' in literal_eval(i).keys():
                steps.append('step2')
            elif 'step1' in literal_eval(i).keys():
                steps.append('step1')

check if np.nan

    x.isnull()

# groupby aggregation

 `.count()` 는 Null 값을 제외한 모든 값을 세어준다. 중복값 포함 `cf` `.value_counts()`

`.nunique()` 는 중복값을 제외한 값을 세어준다

`.first()` 는 그룹별 첫번째 행을 가져온다

cf. 그룹별 n 번째 행 가져오기

    g = df.groupby('id')
    df[g.cumcount() == n - 1]

dictionary 두개 합치기

    dict(dict2, **dict1)

string 으로 들어있는 dictionary 를 dict type 으로 바꾸기

    from ast import literal_eval
    literal_eval(*string_type_dict*)

string 으로 들어있는 dictionary series  ( None 값 포함 ) 의 값들을 dict type 으로 바꾸기

    input_info = []
    for i in event_applications['information']:
         if isinstance(i, str):
              input_info.append(literal_eval(i))

최대공약수 찾기

    from math import gcd
    gcd(5, 10)

LAG

  `*DataFrame*.shift(1)` : 1 행 앞의 값

  `*DataFrame*.shift(-1)` : 1 행 뒤의 값

리스트 원소 세기

    # 리스트의 원소가 숫자나 문자일 때만 가능
    from collections import Counter
    Counter([set(x) for x in list])
    
    # 리스트의 원소가 set, list일 때도 가능
    from itertools import groupby
    [len(list(group)) for key, group in groupby([set(x) for x in list])]

# ETC

Jupiter notebook 에서 줄바꿈할 때 `\`