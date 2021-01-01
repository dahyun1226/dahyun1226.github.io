# 파이썬 머신러닝 완벽가이드 Chapter 1

# 1. numpy

키워드 중심으로 공부해보자.

`type(x)` x의 타입

`.dtype` 안에있는 데이터

`.ndim` 차원

넘파이의 기반 데이터 타입은 ndarray이다.

ndarray는 같은 데이터 타입만 넣을 수 있다.

만약 섞여있는 리스트 등을 ndarray로 넣으면 더 큰 데이터 타입으로 바뀐다. 참고로 문자열은 크다.

1차원은 (3,)이런식으로 명확하게 표현된다. (3,1)과 구분됨.

`arange(n)` 0부터시작, n-1까지.

`zeros` ((3,2),dtype='int32') default dtype은 float64

`ones` zeros와 마찬가지

`reshape(-1,4)` 10개인데 이러면 에러, reshape(-1,1)형태로 많이 사용한다.

`index` 기본 [x] 0부터 시작이므로 x+1번째 호출 [-1]이 맨 뒤의 값. [-2]는 맨 뒤의 앞 값.

다차원 배열도 특이할 거 없음. 0부터 시작. 당연히 row부터

axis 0이 로우 방향(가로)축, 이후 1, 2,3

슬라이싱 => [1:3] 0부터 2까지 [:5] 처음부터 4까지, 물론 여기서도 처음은 0이라는 거

[:] 은 전체고, 2차원에서  뒤에 오는 []를 없애면 그 줄 전체임, 3차원은 2차원 반환하겠지

팬시 인덱싱 => 리스트나 ndarray로 인덱스 집합을 지정하면 됨. 

array2 = array1[[0,1],2]이런식으로 지정

불린 인덱싱 => 매우 자주 사용된다. 인덱스를 지정하는 [] 내에 조건문을 기재하면 된다.

행렬 정렬 

np.sort(array)는 원 행렬을 그대로 유지, 반환하는 형태고

array.sort()는 원 행렬 자체를 정렬하는 형태.

만약 내림차순으로 하고싶다면 np.sort(array)[::-1]

np.argsort(array)는 정렬된 행렬의 인덱스를 반환 [1 0 3 2]이런식으로

내림차순의 정렬 인덱스도 마찬가지. np.argsort(array)[::-1]이렇게 하면 된다.

선형대수 내적 np.dot(A,B) 전치행렬 np.transpose(A)

# 2. pandas

판다스는 대부분 2차원 데이터를 가진다.핵심 객체는 dataFrame

데이터 읽어오는 3가지 

read_csv , read_table, read_fwf

read_table은 디폴트 delimeter 필드 구분 문자가 탭인것 말고는 read_csv와 차이가 없다.

read_csv('파일명',sep='\t')이런식으로 쓰면 되니까 read_csv만 사용해도 된다

titanic_df=pd.read_csv(r"C:\Users\pc\newPython\kaggleData\titanic\train.csv")

이런식으로 사용한다. 여기서 r 중요하다는 점.

dataframe.info()로 각종 정보를 알 수 있다.

dataframe.describe()로 개략적인 데이터 분포도를 확인 가능하다.

dataframe의 []연산자 내부에 칼럼명을 입력하면 Series 형태로 특정 칼럼 데이터 세트가 반환된다.

그 객체에 value_counts()를 호출하면 유형과 갯수를 알려준다.

titanic_df['Pclass'].value_counts() 이런식으로 사용한다.  이건 dataframe은 없고 series만 가지고 있다.

모든 dataframe과 Series는 index를 반드시 가진다.  

value_counts에서는 인덱스가 의미있는 데이터가 될 것이다. 

dataframe과 리스트, 딕셔너리, ndarray의 상호 변환은 매우 자주 일어난다.

dataframe은 리스트, ndarray와 다르게 칼럼명을 가지고 있다. 없으면 디폴트 칼럼값 자동 생성.

df=pd.DataFrame(array,columns=col_name_list) 요런식.

2행 3열이라면 3개가 필요하겠지.

딕셔너리를 넣으면 key가 칼럼 이름, value는 그 칼럼에 맞춘 값으로 넣어진다.

dataframe을 ndarray로 변환하려면 array=df.values 이렇게 values를 쓴다.

리스트로는 tolist() 딕셔너리로는 to_dict('list') 리스트 형식으로 반환하기 위해 인자로 '리스트'라고 넣는다.

dataframe에서 새로운 칼럼 추가하려면 df['newcolumnname']=0 하면 전부 0으로 칼럼이 생성된다

상숫값을 배정하면 모든 데이터 세트에 일괄 적용된다

데이터 삭제는 drop()을 이용한다.

~~~python
df.drop(labels=None, axis=0,index=None, colums=None, level=None, inplace=False,errors='raise')
~~~

axis =0 이면 로우를 드롭하겠다는 이야기이다. axis=1이면 칼럼이고.

labels는 당연히 그 칼럼 명일거고, axis=0이면 인덱스일것이다

대부분 칼럼을 없애는 거일테니까 axis=1이 많이 쓰인다.

inplace=True를 하면 반환값 없이 자기 자신을 바꾼다. 그래서 보통 False 쓰는게 좋을 것이다.

디폴트 값이 False이다.

판다스는 RDBMS의 Primary Key와 비슷하게 dataframe, series의 레코드를 고유하게 식별하는 객체이다.

dataframe, series의 index 객체만 추출하려면 .index를 통해 가능하다.

하지만 인덱스 객체는 함부로 변경할 수는 없고, 식별용으로만 사용된다.

.reset_index(inplace=False)처럼 메서드를 수행하면 기존 인덱스를 새로운 칼럼으로 추가하고 새롭게 인덱스를 할당한다. series에 reset을 쓰면 df가 반환되는거 주의, inplace=True하면 기존 인덱스 없어진다. 

그러면 series도 series 그대로겠지

예를 들면 위에 있는 count_values를 쓴 녀석에게 쓰면 되겠다.

​

데이터 셀렉션 및 필터링

dataframe에서 유의해야할 부분은 []연산자이다.

이건 칼럼 지정 연산자로 생각하는게 좋다.

~~~python
print(type(titanic_df['Pclass']))
print(type(titanic_df[['Pclass','Survived']]))
print(type(titanic_df['Pclass']==3))
#이건 안쓰는게 낫다.
#print(titanic_df[0:2])
//
<class 'pandas.core.series.Series'>
<class 'pandas.core.frame.DataFrame'>
<class 'pandas.core.series.Series'>
~~~

dataframe의 []는 series와 numpy의 []와는다르다는점에 유의한다.

ix[]는 deprecated되었다. 참고만 하기

ix는 칼럼 명칭 기반 인덱싱이고, 
~~~python
data_df.ix[0:2,[0,1]]

data_df.ix['name',['year','gender']] 
~~~

대충 이 두 예시를 보면 알 것이다.

​

명칭 기반 인덱싱 vs 위치 기반 인덱싱 

명칭 기반 인덱싱을 하면, 명칭이 0인지 아니면 인덱스가 0인지 헷갈리게 되는 경우가 발생하게 된다.

물론 명칭과 위치 기반의 데이터 형이 같다면 명칭 기반이 우선한다고 이해하면 큰 혼선이 없을 수도 있지만, 새롭게 명칭 기반 인덱싱 연산자인 loc와 위치 기반 인덱싱인 iloc를 도입했다.

​

iloc[]

무조건 위치 기반. 슬라이싱과 팬시 및 불린 인덱싱은 된다.

 loc[]

무조건 명칭 기반.

슬라이싱 되는데, 보통의 슬라이싱은 마지막 껄 포함하지 않지만(0:1이면 0뿐) 명칭 기반 슬라이싱은 1까지 포함한다는 점에 유의하자.

​

불린 인덱싱,

여러 개의 조건이 가능하다.
~~~python
print(type(titanic_df[(titanic_df['Pclass']>2)&(titanic_df['Sex']=='female')]))
//
<class 'pandas.core.frame.DataFrame'>
~~~
not 조건일때 !가 아닌 ~ 라는 점에 유의하도록 하자.

​

정렬, Aggregation함수, GroupBy 적용

sort_values()

주요 파라미터는 by=['Name'](칼럼 지정), ascending=True(기본이 오름차순), inplace=False

aggregation 함수는 .mean(), .max() .count() .sum()등이 있다

groupby는 해당 칼럼으로 묶이는 것.
~~~python
tg=titanic_df.groupby(by='Pclass')
type(tg)
//
pandas.core.groupby.generic.DataFrameGroupBy
tg=titanic_df.groupby(by='Pclass')
agg_format={'Age':['max','min'],'SibSp':'sum','Fare':'mean'}
tg.agg(agg_format)
//
	Age	SibSp	Fare
max	min	sum	mean
Pclass				
1	80.0	0.92	90	84.154687
2	70.0	0.67	74	20.662183
3	74.0	0.42	302	13.675550
~~~

결손 데이터 처리하기 

.isna()를 쓰면 true나 false로 바뀐다.

.fillna(x)로 NaN 들을 다 x로 바꿀 수 있다.

​

apply lambda 식으로 데이터 가공

map과 자주 사용된다.

.apply(lamda x : fun(x)) 식으로 하면 된다.

~~~python
a=[1,2,3]
squares = map(lambda x: x**2 ,a)
print(type(squares))
list(squares)
~~~