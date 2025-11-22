# Исследование метаданных DNS трафика
tolmachevnikita04@yandex.ru

## Цель работы

1.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  Программное обеспечение ОС Windows 11 Pro
2.  RStudio
3.  Интерпретатор языка R 4.5.1

## Шаги:

1 Импорт данных

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
library(tidyverse)
```

    Warning: пакет 'tidyverse' был собран под R версии 4.5.2

    Warning: пакет 'ggplot2' был собран под R версии 4.5.2

    Warning: пакет 'tibble' был собран под R версии 4.5.2

    Warning: пакет 'tidyr' был собран под R версии 4.5.2

    Warning: пакет 'readr' был собран под R версии 4.5.2

    Warning: пакет 'purrr' был собран под R версии 4.5.2

    Warning: пакет 'forcats' был собран под R версии 4.5.2

    Warning: пакет 'lubridate' был собран под R версии 4.5.2

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ forcats   1.0.1     ✔ readr     2.1.6
    ✔ ggplot2   4.0.1     ✔ stringr   1.5.2
    ✔ lubridate 1.9.4     ✔ tibble    3.3.0
    ✔ purrr     1.2.0     ✔ tidyr     1.3.1

    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(httr)
library(jsonlite)
```


    Присоединяю пакет: 'jsonlite'

    Следующий объект скрыт от 'package:purrr':

        flatten

``` r
download.file(
  "https://storage.yandexcloud.net/dataset.ctfsec/dns.zip", 
  destfile = "dns.zip"
)

unzip("dns.zip")

dns_data <- read.table(
  "dns.log",
  header = FALSE,
  sep = "\t",
  comment.char = "#",
  fill = TRUE
)
```

    Warning in scan(file = file, what = what, sep = sep, quote = quote, dec = dec,
    : EOF внутри закавыченной строки

2 Добавьте пропущенные данные о структуре данных (назначении столбцов)

``` r
colnames(dns_data) <- c(
  "ts", "uid", "id.orig_h", "id.orig_p", "id.resp_h", "id.resp_p",
  "proto", "trans_id", "rtt", "query", "qclass", "qclass_name",
  "qtype", "qtype_name", "rcode", "rcode_name", "AA", "TC", "RD",
  "RA", "Z", "answers", "TTLs"
)
```

3 Преобразуйте данные в столбцах в нужный формат

``` r
dns_data <- dns_data %>%
  mutate(
    ts = as.POSIXct(ts, origin = "1970-01-01"),
    id.orig_p = as.integer(id.orig_p),
    id.resp_p = as.integer(id.resp_p),
    trans_id = as.integer(trans_id)
  )
```

4 Просмотрите общую структуру данных с помощью функции glimpse()

``` r
glimpse(dns_data)
```

    Rows: 320,840
    Columns: 23
    $ ts          <dttm> 2012-03-16 16:30:05, 2012-03-16 16:30:15, 2012-03-16 16:3…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7Bs…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "19…
    $ id.orig_p   <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "1…
    $ id.resp_p   <int> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "u…
    $ trans_id    <int> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62…
    $ rtt         <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ query       <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass      <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C…
    $ qclass_name <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32"…
    $ qtype       <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB…
    $ qtype_name  <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode       <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-…
    $ rcode_name  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ TC          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRU…
    $ RD          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ RA          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0…
    $ Z           <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…

### Анализ данных

4 Сколько участников информационного обмена в сети Доброй Организации?

``` r
unique_participants <- dns_data %>%
  summarise(
    unique_sources = n_distinct(id.orig_h),
    unique_destinations = n_distinct(id.resp_h),
    total_unique = n_distinct(c(id.orig_h, id.resp_h))
  )
print(unique_participants)
```

      unique_sources unique_destinations total_unique
    1            201                1182         1288

5 Какое соотношение участников обмена внутри сети и участников обращений
к внешним ресурсам?

``` r
all_ips <- unique(c(dns_data$id.orig_h, dns_data$id.resp_h))
length(all_ips[grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", all_ips)])/length(setdiff(all_ips, all_ips[grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", all_ips)]))
```

    [1] 15.72727

6 Найдите топ-10 участников сети, проявляющих наибольшую сетевую
активность.

``` r
dns_data %>%  count(id.orig_h, sort = TRUE) %>%  head(10)
```

             id.orig_h     n
    1    10.10.117.210 75943
    2   192.168.202.93 15725
    3    10.10.117.209 14222
    4   192.168.202.97 14216
    5  192.168.202.110 13372
    6  192.168.202.103 12818
    7   192.168.203.63 11962
    8   192.168.202.76 10950
    9  192.168.229.252  9530
    10  192.168.202.83  9165

7 Найдите топ-10 доменов, к которым обращаются пользователи сети и
соответственное количество обращений

``` r
top_domains <- dns_data %>% 
  count(query, sort = TRUE) %>% 
  slice(1:10)

top_domains
```

      query      n
    1     1 317087
    2     -   2483
    3     3    824
    4 32769    446

8 Опеределите базовые статистические характеристики (функция summary() )
интервала времени между последовательным обращениями к топ-10 доменам.

``` r
top_domains_list <- top_domains$query

time_intervals <- dns_data %>%
  filter(query %in% top_domains_list) %>%
  arrange(query, ts) %>%
  group_by(query) %>%
  mutate(
    time_diff = as.numeric(difftime(ts, lag(ts), units = "secs"))
  ) %>%
  filter(!is.na(time_diff))

time_intervals %>%
  group_by(query) %>%
  summarise(
    min    = min(time_diff),
    q1     = quantile(time_diff, 0.25),
    median = median(time_diff),
    mean   = mean(time_diff),
    q3     = quantile(time_diff, 0.75),
    max    = max(time_diff)
  )
```

    # A tibble: 4 × 7
      query   min      q1  median    mean    q3    max
      <chr> <dbl>   <dbl>   <dbl>   <dbl> <dbl>  <dbl>
    1 -         0 0.01000 0.655    35.5   2.89  49787.
    2 1         0 0       0.01000   0.278 0.120 49669.
    3 3         0 0       0.01000  39.5   1.02   7116.
    4 32769     0 0.0600  1.65    195.    2.21  52833.

9 Часто вредоносное программное обеспечение использует DNS канал в
качестве канала управления, периодически отправляя запросы на
подконтрольный злоумышленникам DNS сервер. По периодическим запросам на
один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP
адреса в исследуемом датасете?

``` r
suspicious_activity <- dns_data %>%
  group_by(id.orig_h, query) %>%
  summarise(
    request_count = n(),
    .groups = 'drop'
  ) %>%
  filter(request_count > 10) %>%
  arrange(desc(request_count))
periodic_requests <- dns_data %>%
  semi_join(suspicious_activity, by = c("id.orig_h", "query")) %>%
  arrange(id.orig_h, query, ts) %>%
  group_by(id.orig_h, query) %>%
  mutate(time_diff = as.numeric(difftime(ts, lag(ts), units = "secs"))) %>%
  filter(!is.na(time_diff)) %>%
  summarise(
    request_count = n(),
    mean_interval = mean(time_diff),
    sd_interval = sd(time_diff),
    cv = sd(time_diff) / mean(time_diff),
    .groups = 'drop'
  ) %>%
  filter(cv < 0.5) %>%
  arrange(cv)
print(periodic_requests)
```

    # A tibble: 0 × 6
    # ℹ 6 variables: id.orig_h <chr>, query <chr>, request_count <int>,
    #   mean_interval <dbl>, sd_interval <dbl>, cv <dbl>

10 Определите местоположение (страну, город) и организацию-провайдера
для топ-10 доменов.

``` r
library(dplyr)
library(httr)
library(jsonlite)

top_domains <- dns_data %>% 
  count(query, sort = TRUE) %>% 
  slice(1:10)

top_domains_list <- top_domains$query

domain_ips <- dns_data %>%
  filter(query %in% top_domains_list) %>%
  select(query, answers) %>%
  filter(!is.na(answers) & answers != "-") %>%
  distinct()

get_geolocation <- function(ip) {
  url <- paste0("http://ip-api.com/json/", ip)
  tryCatch({
    response <- GET(url)
    if (status_code(response) == 200) {
      data <- fromJSON(content(response, "text"))
      return(data.frame(
        ip = ip,
        country = ifelse(is.null(data$country), NA, data$country),
        city = ifelse(is.null(data$city), NA, data$city),
        org = ifelse(is.null(data$org), NA, data$org),
        stringsAsFactors = FALSE
      ))
    }
    return(NULL)
  }, error = function(e) {
    return(NULL)
  })
}
geolocation_results <- data.frame()

for (i in 1:min(10, nrow(domain_ips))) {
  ip <- domain_ips$answers[i]
  domain <- domain_ips$query[i]
  
  geo_data <- get_geolocation(ip)
  
  if (!is.null(geo_data)) {
    geo_data$domain <- domain
    geolocation_results <- rbind(geolocation_results, geo_data)
  }
  
  Sys.sleep(1)
}

print(geolocation_results)
```

                      ip country city org domain
    1         120.000000      NA   NA  NA      -
    2      561620.000000      NA   NA  NA      1
    3        4015.000000      NA   NA  NA      1
    4      561577.000000      NA   NA  NA      1
    5       80536.000000      NA   NA  NA      1
    6       80492.000000      NA   NA  NA      1
    7           0.000000      NA   NA  NA      3
    8  1476526080.000000      NA   NA  NA      3
    9      561091.000000      NA   NA  NA      1
    10     561029.000000      NA   NA  NA      1

## Оценка результата

В ходе практической работы была проведена аналитическая обработка
сетевых данных внутреннего сегмента Доброй Организации. Удалось
восстановить отсутствующие метаданные, выполнить анализ DNS-активности и
подготовить ответы на поставленные вопросы.

## Вывод

Таким образом, в процессе выполнения задания были укреплены практические
навыки работы с языком R, освоены ключевые функции пакетов экосистемы
tidyverse, а также получен опыт исследования и интерпретации метаданных
DNS-трафика.
