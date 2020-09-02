# R 프로그래밍

**Goal** : **데이터분석 준전문가 합격을 위하여 R 프로그래밍 기본 문법 및 예제 학습**



### Referenced From

* https://wikidocs.net/33572



### Usage

* 통계 분석
* 그래픽 리포트



### Features

* conditional, loops, user defined recursive functions, input and output 등 기본 기능 제공
* data handling 에 적합한 언어
* 다만, 데이터 분석에 방대한 라이브러리 양



### 개발 환경

* 윈도우10
* https://cran.rstudio.com/
  * [R - Download R 4.0.2](https://cran.rstudio.com/bin/windows/base/R-4.0.2-win.exe)
  * [R Studio](https://download1.rstudio.org/desktop/windows/RStudio-1.3.1056.exe)



### 기본 문법

* req(1,10) : 1이라는 값을 10번 반복한 vector
* seq(1,10,2) : 순차적 vector (ex. 1,3,5,7,9)
* c(1,10) : vector (ex. 1,2,3,4,5,6,7,8,9,10)

* 데이터 분석을 위해서는 `data.frame()` 형태 사용 필수
* R, R studio 설치 후 필요한 라이브러리는 별도 설치해야함
  * ex. `ggplot` 설치

```
> install.packages("ggplot2") # ggplot2라는 패키지 설치
> library(ggplot2) # ggplot2 패키지 부착
```



### 예제 학습

* 로또 번호 만들기
  * 1~45까지 숫자 중 6개 무작위로 뽑음(비복원추출)

```R
> sample(1:45, 6, replace = FALSE)
```





* 데이터 loading
  * Download From : [Click](https://www.dropbox.com/sh/vtqlvrgdts2yfez/AAD_cd49dBcvgBNdz-C-A6TFa?dl=0)

```R
> HR = read.csv('<path of HR_comma_sep.csv>')
```



* 데이터 Handling
  * 분석에서 가장 중요한 부분 차지
  * 기본 데이터 : `Raw 데이터`



#### 조건에 맞는 값 할당

* satisfation_level >= 0.5 `High` not `Low`

```R
> HR$satisfaction_level_group_1 = 
ifelse(HR$satisfaction_level > 0.5, 'High', 'Low')
```



* satisfaction_level > 0.8 `High`, 0.5~0.8 `Mid`, < 0.5 `Low`

```R
> HR$satisfaction_level_group_2 = ifelse(HR$satisfaction_level > 0.8, 'High',
                                     ifelse(HR$satisfaction_level > 0.5,'Mid','Low'))
```





#### 조건에 맞는 데이터 추출



* salary 가 `high` 인 직원들만 추출

```R
> HR_High = subset(HR, salary=='high')
```



* salary 가 `high` 이면서, department 가 `IT`

```R
> HR_High_IT = subset(HR, salary=='high' & department=='IT')
```



* salary 가 `high` 혹은, department 가 `IT`

```R
> HR_High_IT2 = subset(HR, salary=='high' | department=='IT')
```



#### 집계 데이터

* install package(`plyr`)

```R
> install.packages("plyr")
> library(plyr)
```

* ddply 활용 집계 데이터 생성

```R
> SS = ddply(HR, c("department", "salary"), summarise,
            M_SF=mean(satisfaction_level), COUNT=length(department),
            M_WH=round(mean(average_montly_hours),2))
```



### ggplot2

* R, Python 에서 그래프 전용 패키지
* 라이브러리 사용 전 아래 명령어 입력 필수

```R
> library(ggplot2)
```



* **How to draw graph**
  * 축(axis)
    * ggplot(데이터명, aes(x=변수1, y=변수2))
    * ex. ggplot(HR, aes(x=department, y=average_montly_hours))
  * 그래프
    * geom_histogram
      * geom_bar() : 막대도표
      * geom_histogram : 히스토그램
      * geom_boxplot : 박스플롯
      * geom_line : 선 그래프
  * 범례, 제목, 글씨
    * labs() : 범례 제목 수정
    * ggtitle(), 제목 수정
    * xlabs(), ylabs(), x축, y축 이름 수정



* salary 카운트 막대 그래프 그리기

```R
> ggplot(HR, aes=(x=salary)) + geom_bar()
# 색깔 변경
> ggplot(HR, aes=(x=salary)) + geom_bar(fill='royalblue')
# column 별 색깔 변경
> ggplot(HR, aes=(x=salary)) + geom_bar(fill=left)
```



* salary 카운트 막대 그래프 그리기

```R
> ggplot(HR, aes=(x=salary)) + geom_bar()
# 색깔 변경
> ggplot(HR, aes=(x=salary)) + geom_bar(fill='royalblue')
# column 별 색깔 변경
> ggplot(HR, aes=(x=salary)) + geom_bar(fill=left)
```



* satisfaction_level 별 histogram 그리기

```R
> ggplot(HR,aes(x=satisfaction_level))+
   geom_histogram()
```



* satisfaction_level 별 밀도그래프 그리기

```R
> ggplot(HR,aes(x=satisfaction_level))+
   geom_density()
```



* boxplot
  * 이산형 변수에 따라 연속형 변수 분포 차이를 보여주는 그래프
  * x 축이 이산형 변수(True of False), y축이 연속형 변수(소수점)

```R
> ggplot(HR,aes(x=left,y=satisfaction_level)) +
   geom_boxplot(aes(fill = left)) +
   xlab("이직여부") + ylab("만족도") + ggtitle("Boxplot") +
   labs(fill = "이직 여부")
```



* 산점도(scatter plot)
  * 두 연속형 변수 상관관계 2차원 그래프
  * 모델링 전 변수 관계 수립 목적으로 사용

```R
> ggplot(HR,aes(x=average_montly_hours,y=satisfaction_level))+
   geom_point()
```



* 데이터 불러오기 (in my computer)

```R
> HR = read.csv("C:/ndj/github/dongjinnam.github.io/study/r-tutorial/DATA SET(Dropbox)/HR_comma_sep.csv")
```



* 결측치(데이터 값 없음 = `null`)
  * 결측치를 모두 제거할 경우, 막대한 데이터 손실
  * 잘못 대체할 경우, 데이터 편항(bias)
  * 결측치 체크

```
> is.na(IMDB$Metascore) # 결측치면 True, 그렇지 않으면 False
> sum(is.na(IMDB$Metascore)) # Metascore 에 대한 결측치 개수
> colSums(is.na(IMDB)) # 모든 Column 의 대한 결측치 개수
```



* 결측치 제거
  * 극단적인 방법이라 가능한 사용하지 않는 것을 권장

```
> IMDB2 = na.omit(IMDB)
> colSums(is.na(IMDB2))
```



* 특정 변수에 결측치가 존재하는 행만 삭제

```R
> IMDB3 = IMDB[complete.cases(IMDB[ ,12]), ]
> colSums(is.na(IMDB3))
```



* **Most important** : `raw 데이터는 건드리지 않는다.`



* 이상치 뽑아내기
  * 중심값으로부터 많이 떨어져 있는 값.
  * 평균에 막대한 영향을 끼침
  * [1,2,3,4,5] 평균은 3 이지만, [1,2,3,4,100]의 평균은 22

```R
> ggplot(IMDB,aes(x=as.factor(Year),y=Revenue..Millions.))+
  geom_boxplot(aes(fill=as.factor(Year)),outlier.colour = 'red',alpha=I(0.4))+
  xlab("년도") + ylab("수익") + labs(fill = "년도")
```



* Outlier = 이상치
* 이상치 처리 방법
  * 데이터 변형을 통해 Outlier 문제 줄이기



* 문자열 대체
  * Genre 에 해당하는 데이터 문자 ',' 를 ' '로 변경
  * `gsub` 함수는 필수

```R
> IMDB$Genre2 = gsub(",", " ", IMDB$Genre)
```





### 텍스트 마이닝

* 코퍼스 생성
  * 영어 경우, 대문자 소문자가 다른 글자로 인식되기에 변경 작업 필요

```R
> install.packages("tm")
> library(tm)
> CORPUS = Corpus(VectorSource(IMDB$Genre2)) # 코퍼스 생성
> CORPUS_TM = tm_map(CORPUS, removePunctuation) # 특수문자 제거
> CORPUS_TM = tm_map(CORPUS_TM, removeNumbers) # 숫자 제거
> CORPUS_TM = tm_map(CORPUS_TM, tolower) # 알파벳 소문자로 변경
```

* TDM(문서 행렬) 생성
  * 특정 단어를 변수로 만들어서, 분석하려는 목적

```R
> TDM = DocumentTermMatrix(CORPUS_TM) # 문서행렬
> inspect(TDM)
> TDM = as.data.frame(as.matrix(TDM)) # 문서행렬 -> 데이터프레임
> head(TDM)
```



* 기존 데이터와 결합
  * cbind : 행이 동일하고, 순서도 같을 시 옆으로 합치기
  * rbind : 열이 동일하고, 순서도 같을 시 아래로 합치기
  * merge : 열과 행이 다른 두 데이터 셋을 하나의 기준으로 합치기

```R
> IMDB_GENRE = cbind(IMDB, TDM)
```





**Genre** Column 대신 **Description** 활용하여 아래와 같이 실습



```R
> library(tm)
> CORPUS = Corpus(VectorSource(IMDB$Description))
> CORPUS_TM = tm_map(CORPUS, stripWhiteSpace)
> CORPUS_TM = tm_map(CORPUS_TM, removePunctuation)
> CORPUS_TM = tm_map(CORPUS_TM, removeNumbers)
> CORPUS_TM = tm_map(CORPUS_TM, tolower)

> DTM = DocumentTermMatrix(CORPUS_TM)
> inspect(DTM)

# 1단계, stopward 를 통한 단어제거(and, for, from, with 이런 단어들을 제거해주는 것이 분석 수월)
> CORPUS_TM = tm_map(CORPUS_TM, removeWords, c(stopwords("english"),"my", "custom", "words"))

# 2단계, 중복등장 단어 처리 결정
# 특정 단어가 문장에 포함되면 1, 그렇지 않으면 0
> convert_count = function(x) { 
    y <- ifelse(x > 0.1, 0)
    y = as.numeric(y)
    y
}
# 특정 단어가 문장에서 몇 번 등장?
> convert_count = function(x) {
    y <- ifelse(x > 0, 0)
    y = as.numeric(y)
    y
}

# 3단계. 문자열 데이터 시각화
TDM = TermDocumentMatrix(CORPUS_TM)

# 워드클라우드 생성
m = as.matrix(TDM)
v = sort(rowSums(m), decreasing=TRUE) # 빈도수 기준 내림차순 정렬
d = data.frame(word = names(v), freq=v) # 데이터 프레임 생성
library("SnowballC")
library("wordcloud")
library("RColorBrewer")
```



