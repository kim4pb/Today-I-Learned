# pandas

# quantile

    series.quantile(0.90)

# Datetime, Timestamp, Timedelta

timedelta to histogram

    series_of_timedelta / pd.Timedelta(hours=1)
    series_of_timedelta / pd.Timedelta(minutes=1)

# Type

check if NoneType

    x == type(None)

check if np.nan

    x.isnull()

# groupby aggregation

 `.count()` 는 Null 값을 제외한 모든 값을 세어준다. 중복값 포함

`.nunique()` 는 중복값을 제외한 값을 세어준다

`.first()` 는 그룹별 첫번째 행을 가져온다

cf. 그룹별 n 번째 행 가져오기

    g = df.groupby('id')
    df[g.cumcount() == n - 1]

# Basic

Jupiter notebook 에서 줄바꿈할 때 `\`