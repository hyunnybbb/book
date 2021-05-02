# 올림픽에는 어떤데이터가 ?



## 분석 방향
1. 여성
2. 나이
3. 국적
4. 동계, 하계
5. 동명이인
6. 대한민국데이터
7. 종목별 금메달 나이 평균
8. 결측치의 분포 / 어느종목에서 가장많은지? // 결측치 없는 데이터만 가지고 통계분석  -> 시간대에 따른 나이, 몸무게, 키에 대한 정보
9. 1904년도의 여자 나이대의 특수성 anova
10. 참여 국가수
11. 남녀의 국가별 시간에 따른 비율변화 (추세선그리기)
12. 남녀의 시간별 키, 몸무게 변화그래프
13. 일본과의 비교 , 북한과 비교

## 1. 데이터 전처리

데이터로드


```python
import pandas as pd
```


```python
nr = pd.read_csv("noc_regions.csv")
ae = pd.read_csv("athlete_events.csv")
```


```python
nr.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NOC</th>
      <th>region</th>
      <th>notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AFG</td>
      <td>Afghanistan</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AHO</td>
      <td>Curacao</td>
      <td>Netherlands Antilles</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALB</td>
      <td>Albania</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALG</td>
      <td>Algeria</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AND</td>
      <td>Andorra</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
ae.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Games</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992 Summer</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>A Lamusi</td>
      <td>M</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012 Summer</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Gunnar Nielsen Aaby</td>
      <td>M</td>
      <td>24.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark</td>
      <td>DEN</td>
      <td>1920 Summer</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Football</td>
      <td>Football Men's Football</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900 Summer</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>21.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1988 Winter</td>
      <td>1988</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 500 metres</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
nr.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 230 entries, 0 to 229
    Data columns (total 3 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   NOC     230 non-null    object
     1   region  227 non-null    object
     2   notes   21 non-null     object
    dtypes: object(3)
    memory usage: 5.5+ KB



```python
ae.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 271116 entries, 0 to 271115
    Data columns (total 15 columns):
     #   Column  Non-Null Count   Dtype  
    ---  ------  --------------   -----  
     0   ID      271116 non-null  int64  
     1   Name    271116 non-null  object 
     2   Sex     271116 non-null  object 
     3   Age     261642 non-null  float64
     4   Height  210945 non-null  float64
     5   Weight  208241 non-null  float64
     6   Team    271116 non-null  object 
     7   NOC     271116 non-null  object 
     8   Games   271116 non-null  object 
     9   Year    271116 non-null  int64  
     10  Season  271116 non-null  object 
     11  City    271116 non-null  object 
     12  Sport   271116 non-null  object 
     13  Event   271116 non-null  object 
     14  Medal   39783 non-null   object 
    dtypes: float64(3), int64(2), object(10)
    memory usage: 31.0+ MB



```python
nr.isnull().sum()
```




    NOC         0
    region      3
    notes     209
    dtype: int64



결측치는 region :3 , notes :209개 존재

데이터 상으로 region 영역에 중복데이터가 포함되어있다. 


```python
len(nr['NOC'].unique())
```




    230



noc_regions 데이터의 NOC컬럼은 230의 유니크 값을 가진다 (== 전체 row의 NOC값은 유니크 하다)

## 2.데이터 EDA

### Q1. noc_regions.csv 데이터에서 region이 같지만 NOC 혹은 notes값이 다른 데이터들만 추출하여 duplicates_nation 변수에 저장한 후  region의 알파벳 순으로 정렬  상위 10개의 데이터를 출력하라.


### Ans1.


```python
duplicates_nation = nr.loc[nr.region.isin(nr.loc[set(range(len(nr)))-set(list(nr[['region']].drop_duplicates().index))].region)].sort_values('region').reset_index(drop=True)
```


```python
duplicates_nation.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NOC</th>
      <th>region</th>
      <th>notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ANZ</td>
      <td>Australia</td>
      <td>Australasia</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AUS</td>
      <td>Australia</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CAN</td>
      <td>Canada</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NFL</td>
      <td>Canada</td>
      <td>Newfoundland</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CHN</td>
      <td>China</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>HKG</td>
      <td>China</td>
      <td>Hong Kong</td>
    </tr>
    <tr>
      <th>6</th>
      <td>CZE</td>
      <td>Czech Republic</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>TCH</td>
      <td>Czech Republic</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>BOH</td>
      <td>Czech Republic</td>
      <td>Bohemia</td>
    </tr>
    <tr>
      <th>9</th>
      <td>FRG</td>
      <td>Germany</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Q2. athlete_events.csv를 이용하여 duplicates_nation에 존재하는 NOC마다 몇 개의 데이터가 있는지 확인하라. 결과는 'duplicate_counts' 컬럼으로 만들고 상위10개데이터를 출력하라.


```python
def count_NOC(x):
    df = ae.loc[ae.NOC ==x]
    return len(df)

duplicates_nation['duplicate_counts']  = duplicates_nation['NOC'].apply(count_NOC)
```


```python
duplicates_nation.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NOC</th>
      <th>region</th>
      <th>notes</th>
      <th>duplicate_counts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ANZ</td>
      <td>Australia</td>
      <td>Australasia</td>
      <td>86</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AUS</td>
      <td>Australia</td>
      <td>NaN</td>
      <td>7638</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CAN</td>
      <td>Canada</td>
      <td>NaN</td>
      <td>9733</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NFL</td>
      <td>Canada</td>
      <td>Newfoundland</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CHN</td>
      <td>China</td>
      <td>NaN</td>
      <td>5141</td>
    </tr>
    <tr>
      <th>5</th>
      <td>HKG</td>
      <td>China</td>
      <td>Hong Kong</td>
      <td>685</td>
    </tr>
    <tr>
      <th>6</th>
      <td>CZE</td>
      <td>Czech Republic</td>
      <td>NaN</td>
      <td>1874</td>
    </tr>
    <tr>
      <th>7</th>
      <td>TCH</td>
      <td>Czech Republic</td>
      <td>NaN</td>
      <td>4404</td>
    </tr>
    <tr>
      <th>8</th>
      <td>BOH</td>
      <td>Czech Republic</td>
      <td>Bohemia</td>
      <td>153</td>
    </tr>
    <tr>
      <th>9</th>
      <td>FRG</td>
      <td>Germany</td>
      <td>NaN</td>
      <td>3315</td>
    </tr>
  </tbody>
</table>
</div>



### Q3. athlete_events 데이터 


```python
ae.isnull().sum()
```




    ID             0
    Name           0
    Sex            0
    Age         9474
    Height     60171
    Weight     62875
    Team           0
    NOC            0
    Games          0
    Year           0
    Season         0
    City           0
    Sport          0
    Event          0
    Medal     231333
    dtype: int64




```python
ae
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Games</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992 Summer</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>A Lamusi</td>
      <td>M</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012 Summer</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Gunnar Nielsen Aaby</td>
      <td>M</td>
      <td>24.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark</td>
      <td>DEN</td>
      <td>1920 Summer</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Football</td>
      <td>Football Men's Football</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900 Summer</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>21.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1988 Winter</td>
      <td>1988</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 500 metres</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>271111</th>
      <td>135569</td>
      <td>Andrzej ya</td>
      <td>M</td>
      <td>29.0</td>
      <td>179.0</td>
      <td>89.0</td>
      <td>Poland-1</td>
      <td>POL</td>
      <td>1976 Winter</td>
      <td>1976</td>
      <td>Winter</td>
      <td>Innsbruck</td>
      <td>Luge</td>
      <td>Luge Mixed (Men)'s Doubles</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>271112</th>
      <td>135570</td>
      <td>Piotr ya</td>
      <td>M</td>
      <td>27.0</td>
      <td>176.0</td>
      <td>59.0</td>
      <td>Poland</td>
      <td>POL</td>
      <td>2014 Winter</td>
      <td>2014</td>
      <td>Winter</td>
      <td>Sochi</td>
      <td>Ski Jumping</td>
      <td>Ski Jumping Men's Large Hill, Individual</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>271113</th>
      <td>135570</td>
      <td>Piotr ya</td>
      <td>M</td>
      <td>27.0</td>
      <td>176.0</td>
      <td>59.0</td>
      <td>Poland</td>
      <td>POL</td>
      <td>2014 Winter</td>
      <td>2014</td>
      <td>Winter</td>
      <td>Sochi</td>
      <td>Ski Jumping</td>
      <td>Ski Jumping Men's Large Hill, Team</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>271114</th>
      <td>135571</td>
      <td>Tomasz Ireneusz ya</td>
      <td>M</td>
      <td>30.0</td>
      <td>185.0</td>
      <td>96.0</td>
      <td>Poland</td>
      <td>POL</td>
      <td>1998 Winter</td>
      <td>1998</td>
      <td>Winter</td>
      <td>Nagano</td>
      <td>Bobsleigh</td>
      <td>Bobsleigh Men's Four</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>271115</th>
      <td>135571</td>
      <td>Tomasz Ireneusz ya</td>
      <td>M</td>
      <td>34.0</td>
      <td>185.0</td>
      <td>96.0</td>
      <td>Poland</td>
      <td>POL</td>
      <td>2002 Winter</td>
      <td>2002</td>
      <td>Winter</td>
      <td>Salt Lake City</td>
      <td>Bobsleigh</td>
      <td>Bobsleigh Men's Four</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>271116 rows × 15 columns</p>
</div>




```python

```


```python

```


```python

```


```python

```

## 3.통계분석

## 4.장바구니분석

## 5.시계열 분석

## 6.예측 모델링


```python

```
