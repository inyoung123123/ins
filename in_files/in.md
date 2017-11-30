여성의 초혼 데이터 분석 - 연도별/지역별에 따라
================

초록
====

전체 분석 절차와 결론 이 연구는 일차적으로 전국을 수도권/비수도권으로 나누어 2016년 한 해의 여성의 초혼 건에 대해 분석했고, 다음으로 좀 더 세부적인 데이터를 위해서 읍/면/동 별 2016년 초혼건을 분석했다. 마지막으로는 앞서 분석한 자료의 시간에 따른 추이를 알아보기 위해 2007년~2016년 십년간의 데이터를 분석했다. 이 연구의 결론은 첫째, 인구와 결혼건수는 비례한다. 둘째, 초혼 평균 연령대의 빈도 분포는 거의 비슷하다. 셋째, 이러한 양상은 십년이라는 시간동안 많은 차이를 보이지 않았다. 이러한 분석의 자세한 내용은 다음의 내용에서 다룰것이다.

1.서론
======

#### 1) 분석 주제

요즈음 다양한 이유로 결혼 연령대가 늦춰지거나 비혼주의자가 늘고 있을 뿐 아니라, 단순히 대가족이던 과거와 달리 핵가족, 맞벌이 가족, 딩크족, 동거부부, 딩펫족 등 다양한 가족 형태가 공존하는 추세이다. 뿐만 아니라 도시에서는 비교적 유동적이고 진보적인 가족의 모습이 나타나고 있는 반면, 노인층의 비율이 높은 시골에서는 여전히 가부장적인 이데올로기가 고착화 된 경우가 많다. 그렇기 때문에 초혼 연령이 과거와 어떻게 다른지, 또 지역별로 초혼의 양상이 어떻게 나타나는지에 대해서 분석하였다.

#### 2-1) 데이터 선정 이유

정확한 혼인의 연령대는 곧 재혼을 포함하지 않은 초혼 데이터라고 판단을 했다. 재혼은 초혼을 한 사람 또한 포함될 수 있기 때문에 데이터 분석이 더 어려워질것이라고 생각했다. 또한 다각적으로 초혼에 대해 접근하기 위해서 연령별/지역별 등 한 파일 안에 많은 데이터가 들어있는 '시도/초혼연령별 혼인' 데이터를 선택했다.

#### 2-2) 데이터 소개

이 연구의 목적은 '초혼 건수를 연도별, 지역별, 연령별 등 다양한 시각으로 접근'하는 것이다. 연구에 사용된 자료는 국가통계포털에 공개된 '시도/초혼연령별 혼인'자료이다. 실증분석자료는 8개의 광역시(서울특별시, 부산광역시, 인천광역시, 대구광역시, 대전광역시, 광주광역시, 울산광역시, 세종특별자치시)와 9개의 도(경기도, 경상남도, 경상북도, 충청남도, 충청북도, 전라남도, 전라북도, 강원도, 제주특별자치도)를 표본으로 2007년~2016년의 10개년 통계자료를 통합하여 구축됐다.

2.본론
======

#### 기본 패키지 설치

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
```

#### 파일 불러오기 및 데이터 전처리

``` r
marriage <- read.csv("marriage.csv")
head(marriage)
```

    ##   시도별 연령별    시점   남편   아내
    ## 1   전국     계 2007 년 285413 280738
    ## 2   전국     계 2008 년 270236 264469
    ## 3   전국     계 2009 년 255751 250674
    ## 4   전국     계 2010 년 272972 268541
    ## 5   전국     계 2011 년 277369 272551
    ## 6   전국     계 2012 년 275897 270495

``` r
table(is.na(marriage))
```

    ## 
    ## FALSE 
    ## 17200

``` r
marriage <- rename(marriage, city = 시도별, age = 연령별, year = 시점, wife = 아내)
marriage <- marriage %>% 
  filter(city != '국외')
head(marriage)
```

    ##   city age    year   남편   wife
    ## 1 전국  계 2007 년 285413 280738
    ## 2 전국  계 2008 년 270236 264469
    ## 3 전국  계 2009 년 255751 250674
    ## 4 전국  계 2010 년 272972 268541
    ## 5 전국  계 2011 년 277369 272551
    ## 6 전국  계 2012 년 275897 270495

#### 1) 2016년 한 해 동안 수도권과 비수도권 내 여성의 초혼 데이터의 차이를 알아보기 위해 전체 데이터에서 필요한 지역을 뽑아 데이터를 분석하였다. '계'와 '국외'는 필요하지 않은 정보로 분석하지 않았다. 이 때 그래프는 연령별 혼인 건수의 차이를 알기 위해 막대그래프를 사용하였다.

##### 1-1) 먼저 2016년의 수도권/비수도권의 전체 평균을 분석하였다.

``` r
full_mean1 <- marriage %>% 
  filter(city %in% c('서울특별시', '경기도', '인천광역시', year == '2016 년' )) %>% 
  filter(age != '계') %>% 
  select(wife) %>% 
  summarise(meana = mean(wife))
full_mean1
```

    ##      meana
    ## 1 2905.313

``` r
full_mean2 <- marriage %>% 
  filter(city != "서울특별시" & city != "경기도" & city != "인천광역시" & city != "읍부" & city != "동부" & city != "면부" & city != '전국' , year == '2016 년') %>% 
  filter(age != '계') %>% 
  select(wife) %>% 
  summarise(meanb = mean(wife))
full_mean2
```

    ##      meanb
    ## 1 486.5667

-   수도권 초혼건수의 2016 전체평균은 2905.313, 비수도권 초혼건수의 전체평균은 486.5667로 수도권이 약 6배가 많았다. 수도권의 인구가 비수도권의 인구보다 많은 것을 감안하여, 인구와 초혼건수는 비례함을 알 수 있다.

##### 1-2) 수도권/비수도권 내 여성의 초혼 건수가 가장 많은 연령을 구하기 위해 연령별 평균을 구하고 막대그래프로 비교하였다.

``` r
수도권 <- marriage %>% 
  filter(city %in% c('서울특별시', '경기도', '인천광역시', year == '2016 년')) %>% 
  filter(age != '계') %>% 
  group_by(age) %>% 
  summarise(meancity = mean(wife)) %>% 
  arrange(desc(meancity))

head(수도권)
```

    ## # A tibble: 6 x 2
    ##         age   meancity
    ##      <fctr>      <dbl>
    ## 1 25 - 29세 21724.5667
    ## 2 30 - 34세 14073.3000
    ## 3 20 - 24세  3687.3333
    ## 4 35 - 39세  2914.2000
    ## 5 40 - 44세   595.1667
    ## 6 15 - 19세   285.1000

``` r
비수도권 <- marriage %>% 
  filter(city != "서울특별시" & city != "경기도" & city != "인천광역시" & city != "읍부" & city != "동부" & city != "면부" & city != '전국' , year == '2016 년') %>% 
  filter(age != '계') %>% 
  group_by(age) %>% 
  summarise(meancoutry = mean(wife)) %>% 
  arrange(desc(meancoutry))

head(비수도권)
```

    ## # A tibble: 6 x 2
    ##         age meancoutry
    ##      <fctr>      <dbl>
    ## 1 25 - 29세 3178.00000
    ## 2 30 - 34세 2446.50000
    ## 3 20 - 24세  751.14286
    ## 4 35 - 39세  637.50000
    ## 5 40 - 44세  137.35714
    ## 6 15 - 19세   82.92857

``` r
graph_city <- ggplot(data = 수도권, aes(x = age, y = meancity)) + geom_col() + coord_flip()
graph_city
```

![](in_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
graph_country <- ggplot(data = 비수도권, aes(x = age, y = meancoutry)) + geom_col() + coord_flip()
graph_country
```

![](in_files/figure-markdown_github/unnamed-chunk-10-1.png)

``` r
library(gridExtra)
```

    ## 
    ## Attaching package: 'gridExtra'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine

``` r
grid.arrange(graph_city, graph_country, nrow=2)
```

![](in_files/figure-markdown_github/unnamed-chunk-11-1.png)

-   여성은 25-29세 경에 가장 많이 초혼을 했으며, 두번째로 많은 건수가 있는 연령대는 30-34세이다. 이는 수도권과 비수도권 모두 비슷하게 나타났다. 그러나 혼인건수가 가장 많은 연령인 25-29세를 기준으로 다른 연령을 비교하였을 때, 비수도권이 수도권보다 15-19세, 30-34세, 40-44세의 그래프가 더 오른쪽으로 긴 모습을 보아 비교적 타 연령의 혼인 건수가 많은 것으로 나타났다.

#### 2) 앞서 시행했던 분석의 오차 범위를 줄이기 위해 수도권/비수도권보다 더 좁은 지역을 분석하였다. 다음은 2016년 한 해 동안 읍/면/동 내 여성의 초혼 데이터를 분석한 내용이다. '계'는 역시 필요하지 않은 정보로 분석하지 않았다. 이 때 그래프는 연령별 혼인 건수의 차이를 알기 위해 막대그래프를 사용하였다.

##### 2-1) 읍/면/동의 데이터의 연령별 평균을 각각 구하고, 이를 막대그래프로 만들어 가장 초혼 빈도가 높은 연령대를 알아보았다.

``` r
읍부 <- marriage %>% 
  filter(city == '읍부' &  year == '2016 년') %>% 
  filter(age != '계') %>% 
  group_by(age) %>% 
  summarise(mean1 = mean(wife)) %>% 
  arrange(desc(mean1))
head(읍부)
```

    ## # A tibble: 6 x 2
    ##         age mean1
    ##      <fctr> <dbl>
    ## 1 25 - 29세  7565
    ## 2 30 - 34세  5657
    ## 3 20 - 24세  2147
    ## 4 35 - 39세  1491
    ## 5 40 - 44세   354
    ## 6 15 - 19세   242

``` r
면부 <- marriage %>% 
  filter(city == '면부' &  year == '2016 년') %>% 
  filter(age != '계') %>% 
  group_by(age) %>% 
  summarise(mean2 = mean(wife)) %>% 
  arrange(desc(mean2))
head(면부)
```

    ## # A tibble: 6 x 2
    ##         age mean2
    ##      <fctr> <dbl>
    ## 1 25 - 29세  5541
    ## 2 30 - 34세  4084
    ## 3 20 - 24세  1654
    ## 4 35 - 39세  1054
    ## 5 40 - 44세   249
    ## 6 15 - 19세   215

``` r
동부 <- marriage %>% 
  filter(city == '동부' &  year == '2016 년') %>% 
  filter(age != '계') %>% 
  group_by(age) %>% 
  summarise(mean3 = mean(wife)) %>% 
  arrange(desc(mean3))
head(동부)
```

    ## # A tibble: 6 x 2
    ##         age mean3
    ##      <fctr> <dbl>
    ## 1 25 - 29세 81318
    ## 2 30 - 34세 69731
    ## 3 35 - 39세 18623
    ## 4 20 - 24세 15266
    ## 5 40 - 44세  3786
    ## 6 15 - 19세  1402

``` r
graph1 <- ggplot(data = 읍부, aes(x = age, y = mean1)) + geom_col() + coord_flip()
graph2 <- ggplot(data = 면부, aes(x = age, y = mean2)) + geom_col() + coord_flip()
graph3 <- ggplot(data = 동부, aes(x = age, y = mean3)) + geom_col() + coord_flip()
library(gridExtra)
grid.arrange(graph1, graph2, graph3, nrow=3)
```

![](in_files/figure-markdown_github/unnamed-chunk-15-1.png)

-   동/읍/면이라는 적은 범위에서도 수도권/비수도권과 크게 그래프 모양이 다르지 않았다. 마찬가지로 25-29세의 연령이 가장 혼인건수가 많고, 다음으로 30-34세가 많은 것으로 나타났다. 특히 읍/면은 그래프 mean1, mean2를 보면 알 수 있듯이 거의 차이를 보이지 않았다.

#### 3) 앞서 분석했던 수도권/비수도권, 읍/면/동 자료들의 시간에 따른 변화를 알아보기 위해 2007년 ~ 2016년 근 10년간의 자료를 추가하였다. 마찬가지로 '계'는 필요하지 않은 정보로 분석하지 않았다. 이 때 그래프는 세 변수를 효과적으로 사용하기 위해 산점도를 사용하였다.

##### 3-1)연도에 따른 연령별 초혼 건수를 알아보기 위해 수도권/비수도권 데이터를 각각 분석하여 x축을 연령별, y축을 평균건수, 점의 색을 연도별로 설정했다.

``` r
#수도권#

city1 <- marriage %>% 
  filter(city %in% c('서울특별시', '경기도', '인천광역시')) %>% 
  filter(age != '계')
city1 <- aggregate(wife~year+age, city1, mean)
g <- ggplot(data = city1, aes(x = age, y=wife)) +geom_point()
g1 <- g+geom_point(aes(color=year), size=2)
g1
```

![](in_files/figure-markdown_github/unnamed-chunk-16-1.png)

``` r
#비수도권#
country1 <- marriage %>% 
  filter(city != "서울특별시" & city != "경기도" & city != "인천광역시" & city != "읍부" & city != "동부" & city != "면부" & city != '전국') %>% 
  filter(age != '계')
country1 <- aggregate(wife~year+age, country1, mean)
g2 <- ggplot(data = country1, aes(x = age, y=wife)) +geom_point()

g22 <- g2+geom_point(aes(color=year), size=2)
g22
```

![](in_files/figure-markdown_github/unnamed-chunk-17-1.png)

``` r
library(gridExtra)
grid.arrange(g1, g22, nrow=2)
```

![](in_files/figure-markdown_github/unnamed-chunk-18-1.png)

-   각 점은 연령에 따른 평균 건수를 나타내며, 각 점의 색은 각기 다른 연도를 나타낸다.

-   두 그래프 모두 같은 색의 점끼리 비교를 해보았을 때, 모든 점이 25-29세 -&gt; 30-34세 순의 높이로 찍혀 있다. 이는 2016년과 비교하여 비슷한 결과로 초혼을 하는 연령대는 10년간의 차이가 크게 없었음을 알 수 있다. 또한 두 그래프 모두 24-29세를 기준으로 2007년의 점이 가장 위에 있고, 2016년의 점은 가장 아래에 있어 십년 사이에 초혼 건수가 크게 줄었음을 알 수 있다. 뿐만 아니라 열개의 연도를 각각 2007년-2012년, 2013년-2016년으로 나누어 보았을 때 25-20세에는 전자의 점들이 위쪽에 찍혀있는 반면, 30-34세에는 후자의 점들이 위쪽의 찍혀있다. 이는 여성의 초혼 연령이 점점 낮아지고 있음을 의미한다. 이 외에도 2016년에는 2007년 존재하지 않던 40세 이후의 '황혼결혼' 사례가 생겼음을 볼 수 있다.

##### 3-2) 연도에 따른 연령별 초혼 건수를 알아보기 위해 읍/면/동 데이터를 각각 분석하여 x축을 연령별, y축을 평균건수, 점의 색을 연도별로 설정했다.

``` r
읍 <- marriage %>% 
  filter(city == '읍부') %>% 
  filter(age != '계')
면 <- marriage %>% 
  filter(city == '면부') %>% 
  filter(age != '계') 
동 <- marriage %>% 
  filter(city == '동부') %>% 
  filter(age != '계')
```

``` r
읍 <- aggregate(wife~year+age, 읍, mean)
h <- ggplot(data = 읍, aes(x = age, y=wife)) +geom_point()
h+geom_point(aes(color=year), size=2)
```

![](in_files/figure-markdown_github/unnamed-chunk-20-1.png)

``` r
면 <- aggregate(wife~year+age, 면, mean)
h2 <- ggplot(data = 면, aes(x = age, y=wife)) +geom_point()
h2+geom_point(aes(color=year), size=2)  
```

![](in_files/figure-markdown_github/unnamed-chunk-21-1.png)

``` r
동 <- aggregate(wife~year+age, 동, mean)
h3 <- ggplot(data = 동, aes(x = age, y=wife)) +geom_point()
h3+geom_point(aes(color=year), size=2)
```

![](in_files/figure-markdown_github/unnamed-chunk-22-1.png)

-   전체적으로 세 그래프는 앞서 분석한 수도권/비수도권 그래프와 크게 다른 점이 없었다. 세 그래프에 찍혀있는 같은 색의 점을 비교해보았을 때, 모든 점이 25-29세에 가장 높은 위치에 있었으며, 두번째는 마찬가지로 30-34세가 차지했다. 2016년을 나타내는 분홍색점 또한 수도권/비수도권의 그래프와 마찬가지로 대부분의 연령대에 찍혀 있었다. 뿐만 아니라 특히 눈여겨보아야할 점은 읍/면에서 20-24세에 초혼하는 건수가 동에 비해 높다는 점이다. 이를 보아 인구가 적은 지역일수록 일찍 결혼한다는 것을 알 수 있다.

3.결론
======

#### 1)최종 결론

첫째, 인구와 결혼건수는 비례하며, 수도권과 비수도권의 전체평균을 비교해 보았을 때 수도권의 평균이 비수도권보다 약 6배 높았다. 둘째, 여성의 초혼 건수는 대부분 25-29세에 가장 많고, 다음으로 많은 건수는 30-34세로 나타났다. 셋째, 이러한 양상은 십년이라는 시간동안 큰 차이를 보이지 않았으나, 양적으로는 결혼 건수가 점점 줄고 있음을 볼 수 잇었다. 마지막으로 현 결혼은 과거보다 유동적인 특징을 띠고 있다. 결론적으로 서론에 설명한 바와 같이 여성의 초혼 연령이 점점 늦어지고 있을 뿐 아니라 혼인 건수 또한 줄어들고 있음이 데이터 상으로 드러났다.

#### 2) 한계점, 비판점

수도권/비수도권에서 미처 분석하지 못한 자세한 부분들을 읍/면/동을 분석함으로서 얻을 수 있다고 생각했지만, 거의 비슷한 결과가 나와 읍/면/동의 데이터가 사실상 무의미했다. 또한 연령별 건수의 평균을 구하여 각 변수의 최댓값과 최솟값을 무시했다는 한계점이 있다.

#### 3) 추후분석방향

만약 평균값 대신 중간값을 구한다면 보다 최대/최솟값을 고려하는 분석이 될 것이다. 또한 더 많은 연도의 데이터를 분석하면 연도의 따른 차이를 더 쉽게 볼 수 있을 것이라 예상한다.
