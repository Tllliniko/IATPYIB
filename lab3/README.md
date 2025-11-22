# Третья практика
tolmchevnikita04@yandex.ru

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Rstudio Desktop;
2.  Интерпретатор языка R 4.1;
3.  Программный пакет `dplyr` и `knitr`.

## План

Проанализировать встроенный в пакет dplyr набор данных `nycflights13` с
помощью языка R и ответить на вопросы.

## Решение:

1 Установка датафрейма.

    Устанавливаю пакет в ‘C:/Users/Admin/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимости ‘utf8’, ‘pillar’, ‘pkgconfig’, ‘tibble’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/utf8_1.2.6.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/pillar_1.11.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/pkgconfig_2.0.3.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/tibble_3.3.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/nycflights13_1.0.2.zip'
    пакет ‘utf8’ успешно распакован, MD5-суммы проверены
    пакет ‘pillar’ успешно распакован, MD5-суммы проверены
    пакет ‘pkgconfig’ успешно распакован, MD5-суммы проверены
    пакет ‘tibble’ успешно распакован, MD5-суммы проверены
    пакет ‘nycflights13’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Admin\AppData\Local\Temp\RtmpyqmN2R\downloaded_packages

2 Загрузка библиотек

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.5.2


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

``` r
library(nycflights13)
```

    Warning: пакет 'nycflights13' был собран под R версии 4.5.2

``` r
library(knitr)
```

3 Сколько встроенных в пакет `nycflights13` датафреймов?

``` r
data(package = "nycflights13")$results[, "Item"]
```

    [1] "airlines" "airports" "flights"  "planes"   "weather" 

4 Сколько строк в каждом датафрейме?

``` r
sapply(ls("package:nycflights13"), function(df) nrow(get(df, "package:nycflights13")))
```

    airlines airports  flights   planes  weather 
          16     1458   336776     3322    26115 

5 Сколько столбцов в каждом датафрейме?

``` r
sapply(ls("package:nycflights13"), function(df) ncol(get(df, "package:nycflights13")))
```

    airlines airports  flights   planes  weather 
           2        8       19        9       15 

6 Как просмотреть примерный вид датафрейма?

``` r
head(airlines)
```

    # A tibble: 6 × 2
      carrier name                    
      <chr>   <chr>                   
    1 9E      Endeavor Air Inc.       
    2 AA      American Airlines Inc.  
    3 AS      Alaska Airlines Inc.    
    4 B6      JetBlue Airways         
    5 DL      Delta Air Lines Inc.    
    6 EV      ExpressJet Airlines Inc.

``` r
head(airports)
```

    # A tibble: 6 × 8
      faa   name                             lat   lon   alt    tz dst   tzone      
      <chr> <chr>                          <dbl> <dbl> <dbl> <dbl> <chr> <chr>      
    1 04G   Lansdowne Airport               41.1 -80.6  1044    -5 A     America/Ne…
    2 06A   Moton Field Municipal Airport   32.5 -85.7   264    -6 A     America/Ch…
    3 06C   Schaumburg Regional             42.0 -88.1   801    -6 A     America/Ch…
    4 06N   Randall Airport                 41.4 -74.4   523    -5 A     America/Ne…
    5 09J   Jekyll Island Airport           31.1 -81.4    11    -5 A     America/Ne…
    6 0A9   Elizabethton Municipal Airport  36.4 -82.2  1593    -5 A     America/Ne…

7 Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных
(представлено в наборах данных)?

``` r
length(unique(nycflights13::flights$carrier))
```

    [1] 16

8 Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

``` r
nycflights13::flights %>%
      filter(dest == "JFK", month == 5) %>%
      summarise(flights_count = n())
```

    # A tibble: 1 × 1
      flights_count
              <int>
    1             0

9 Какой самый северный аэропорт?

``` r
nycflights13::airports %>%
      filter(lat == max(lat, na.rm = TRUE))
```

    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     <NA> 

10 Какой аэропорт самый высокогорный (находится выше всех над уровнем
моря)?

``` r
airports %>%
  filter(alt == max(alt, na.rm = TRUE))
```

    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

11 Какие бортовые номера у самых старых самолетов?

``` r
planes %>%
  filter(!is.na(year)) %>%
  arrange(year) %>%
  select(tailnum, year) %>%
  head(10)
```

    # A tibble: 10 × 2
       tailnum  year
       <chr>   <int>
     1 N381AA   1956
     2 N201AA   1959
     3 N567AA   1959
     4 N378AA   1963
     5 N575AA   1963
     6 N14629   1965
     7 N615AA   1967
     8 N425AA   1968
     9 N383AA   1972
    10 N364AA   1973

12 Какая средняя температура воздуха была в сентябре в аэропорту John F
Kennedy Intl (в градусах Цельсия).

``` r
weather %>%
  filter(origin == "JFK", month == 9) %>%
  summarise(mean_temp_c = (mean(temp, na.rm = TRUE) - 32)* 5/9) %>%
  pull(mean_temp_c)
```

    [1] 19.38764

13 Самолеты какой авиакомпании совершили больше всего вылетов в июне?

``` r
flights %>% left_join(airlines, join_by(carrier)) %>% filter(month == 6)%>%group_by(name) %>% summarise(amount = n()) %>% arrange(desc(amount)) %>% slice(1) %>% select(name, amount) |> knitr::kable(format='markdown')
```

<table>
<thead>
<tr>
<th style="text-align: left;">name</th>
<th style="text-align: right;">amount</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">United Air Lines Inc.</td>
<td style="text-align: right;">4975</td>
</tr>
</tbody>
</table>

14 Самолеты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>% left_join(airlines, join_by(carrier)) %>% group_by(name) %>% filter(arr_delay > 0 & year == 2013) %>% summarise(amount = n()) %>% arrange(desc(amount)) %>% slice(1) %>% select(name, amount) |> knitr::kable(format='markdown')
```

<table>
<thead>
<tr>
<th style="text-align: left;">name</th>
<th style="text-align: right;">amount</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">ExpressJet Airlines Inc.</td>
<td style="text-align: right;">24484</td>
</tr>
</tbody>
</table>

## Оценка результата

В ходе выполнения работы были рассмотрены все компоненты набора данных
nycflights13, выполнены операции фильтрации, группировки, сортировки и
объединения таблиц. Все вопросы успешно решены.

## Вывод

В результате выполнения лабораторной работы были закреплены навыки
обработки данных в R, а также освоены основные функции пакета dplyr.
Применение реального набора данных позволило на практике отработать
приёмы аналитической обработки таблиц.
