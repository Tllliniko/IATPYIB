# Третья практика
tolmchevniki04@yandex.ru@yandex.ru

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Программное обеспечение Windows 11 Home
2.  Visual Studio
3.  Интерпретатор языка R 4.1

## План

Используя R и среду разработки RstudioIDE, выполнить задания

## Шаги:


    > library(nycflights13)
    > library(dplyr)
    > # Задание 1.Сколько встроенных в пакет nycflights13 датафреймов?
    > data(package = "nycflights13")
    > 
    > 
    > data(package = "nycflights13")
    > 
    > ls("package:nycflights13")
    [1] "airlines" "airports" "flights" 
    [4] "planes"   "weather" 
    > 
    > # Задание 2. Сколько строк в каждом датафрейме?
    > analyze_dataframe <- function(df, df_name) {}
    > analyze_dataframe <- function(df, df_name) {cat ("\n=== Анализ датафрейма:", df_name, "===\n")}
    > analyze_dataframe <- function(df, df_name) {cat ("\n=== Анализ датафрейма:", df_name, "===\n")
    +  cat ("Количество строк:", nrow(df), "\n")
    +  cat("Количество столбцов", ncol(df), "n")
    +  cat("Структура данных:\n")
    +  glimpse(df)
    + }
    > 
    > for(i in 1:length(df_list)){
    +     analyze_dataframe(df_list[[i]],df_names[i])
    + }
    Ошибка: объект 'df_list' не найден

    > 
    > # Задание 1: Сколько встроенных в пакет nycflights13 датафреймов?
    > 

    > cat("Задание 1: Датафреймы в пакете nycflights13\n")
    Задание 1: Датафреймы в пакете nycflights13
    > data_frames <- ls("package:nycflights13")
    > print(data_frames)
    [1] "airlines" "airports" "flights" 
    [4] "planes"   "weather" 
    > df_list <- list(flights, airlines, airports, planes, weather)
    > 
    > df_names <- c("flights", "airlines", "airports", "planes", "weather")
    > 
    > cat("Количество датафреймов", length(data_frames), "\n\n")
    Количество датафреймов 5 

    > # Задания 2-4: Анализ структуры данных
    > cat("=== Задания 2-4 ===\n")
    === Задания 2-4 ===
    > 
    > cat("1. flights:\n")
    1. flights:
    > cat(" Строк:", nrow(flights), "\n")
     Строк: 336776 
    > cat(" Столбцов:", ncol(flights), "\n")
     Столбцов: 19 
    > glimpse(flights)
    Rows: 336,776
    Columns: 19
    $ year           <int> 2013, 2013, 2013, 2…
    $ month          <int> 1, 1, 1, 1, 1, 1, 1…
    $ day            <int> 1, 1, 1, 1, 1, 1, 1…
    $ dep_time       <int> 517, 533, 542, 544,…
    $ sched_dep_time <int> 515, 529, 540, 545,…
    $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4…
    $ arr_time       <int> 830, 850, 923, 1004…
    $ sched_arr_time <int> 819, 830, 850, 1022…
    $ arr_delay      <dbl> 11, 20, 33, -18, -2…
    $ carrier        <chr> "UA", "UA", "AA", "…
    $ flight         <int> 1545, 1714, 1141, 7…
    $ tailnum        <chr> "N14228", "N24211",…
    $ origin         <chr> "EWR", "LGA", "JFK"…
    $ dest           <chr> "IAH", "IAH", "MIA"…
    $ air_time       <dbl> 227, 227, 160, 183,…
    $ distance       <dbl> 1400, 1416, 1089, 1…
    $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6…
    $ minute         <dbl> 15, 29, 40, 45, 0, …
    $ time_hour      <dttm> 2013-01-01 05:00:0…
    > cat("\n")

    > cat("2. airlines:\n")
    2. airlines:
    > cat(" Строк:", nrow(airlines), "\n")
     Строк: 16 
    > cat(" Столбцов:", ncol(airlines), "\n")
     Столбцов: 2 
    > glimpse(airlines)
    Rows: 16
    Columns: 2
    $ carrier <chr> "9E", "AA", "AS", "B6", "D…
    $ name    <chr> "Endeavor Air Inc.", "Amer…
    > 
    > cat("\n")

    > 
    > cat("3. airports:\n")
    3. airports:
    > cat(" Строк:", nrow(airports), "\n")
     Строк: 1458 
    > cat(" Строк:", nrow(airlines), "\n")
     Строк: 16 
    > cat(" Столбцов:", ncol(airports), "\n")
     Столбцов: 8 
    > glimpse(airports)
    Rows: 1,458
    Columns: 8
    $ faa   <chr> "04G", "06A", "06C", "06N", …
    $ name  <chr> "Lansdowne Airport", "Moton …
    $ lat   <dbl> 41.13047, 32.46057, 41.98934…
    $ lon   <dbl> -80.61958, -85.68003, -88.10…
    $ alt   <dbl> 1044, 264, 801, 523, 11, 159…
    $ tz    <dbl> -5, -6, -6, -5, -5, -5, -5, …
    $ dst   <chr> "A", "A", "A", "A", "A", "A"…
    $ tzone <chr> "America/New_York", "America…
    > 
    > cat("\n")

    > cat("4. planes:\n")
    4. planes:
    > cat(" Строк:", nrow(planes), "\n")
     Строк: 3322 
    > cat(" Столбцов:", ncol(planes), "\n")
     Столбцов: 9 
    > glimpse(planes)
    Rows: 3,322
    Columns: 9
    $ tailnum      <chr> "N10156", "N102UW", "…
    $ year         <int> 2004, 1998, 1999, 199…
    $ type         <chr> "Fixed wing multi eng…
    $ manufacturer <chr> "EMBRAER", "AIRBUS IN…
    $ model        <chr> "EMB-145XR", "A320-21…
    $ engines      <int> 2, 2, 2, 2, 2, 2, 2, …
    $ seats        <int> 55, 182, 182, 182, 55…
    $ speed        <int> NA, NA, NA, NA, NA, N…
    $ engine       <chr> "Turbo-fan", "Turbo-f…
    > 
    > cat("\n")

    > cat("5. weather:\n")
    5. weather:
    > cat(" Строк:", nrow(weather), "\n")
     Строк: 26115 
    > cat(" Столбцов:", ncol(weather), "\n")
     Столбцов: 15 
    > glimpse(weather)
    Rows: 26,115
    Columns: 15
    $ origin     <chr> "EWR", "EWR", "EWR", "E…
    $ year       <int> 2013, 2013, 2013, 2013,…
    $ month      <int> 1, 1, 1, 1, 1, 1, 1, 1,…
    $ day        <int> 1, 1, 1, 1, 1, 1, 1, 1,…
    $ hour       <int> 1, 2, 3, 4, 5, 6, 7, 8,…
    $ temp       <dbl> 39.02, 39.02, 39.02, 39…
    $ dewp       <dbl> 26.06, 26.96, 28.04, 28…
    $ humid      <dbl> 59.37, 61.63, 64.43, 62…
    $ wind_dir   <dbl> 270, 250, 240, 250, 260…
    $ wind_speed <dbl> 10.35702, 8.05546, 11.5…
    $ wind_gust  <dbl> NA, NA, NA, NA, NA, NA,…
    $ precip     <dbl> 0, 0, 0, 0, 0, 0, 0, 0,…
    $ pressure   <dbl> 1012.0, 1012.3, 1012.5,…
    $ visib      <dbl> 10, 10, 10, 10, 10, 10,…
    $ time_hour  <dttm> 2013-01-01 01:00:00, 2…
    > 
    > cat("\n")

    > 
    > #Задание 5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных
    > (представлено в наборах дан- ных)?
    Ошибка: неожиданный символ в "(представлено в"

    > airlines %>%
    + distinct(carrier) %>%
    + nrow()
    [1] 16
    > #Задание 6.
    > flights %>%
    + filter(month == 5, dest == "JFK") %>%
    + nrow()
    [1] 0
    > 
    > #Задание 7.
    > airports %>%
    + arrange(desc(lat)) %>%
    + select(name, lat, lon) %>%
    + head(1)
    # A tibble: 1 × 3
      name                      lat   lon
      <chr>                   <dbl> <dbl>
    1 Dillant Hopkins Airport  72.3  42.9
    > 
    > #Задание 8.
    > airports %>%
    + arrange(desc(alt)) %>%
    + select(name, alt) %>%
    + head(1)
    # A tibble: 1 × 2
      name        alt
      <chr>     <dbl>
    1 Telluride  9078
    > 
    > #Задание9.
    > planes %>%
    + arrange (year) %>%
    + filter(!is.na(year)) %>%
    + select(tailnum, year) %>%
    + head(10)
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
    > 

    > #Задание 10.
    > weather %>%
    + filter(month == 9, origin == "JFK") %>%
    + summarise(avg_temp_c = mean ((temp - 32) * 5/9, na.rm = TRUE))
    # A tibble: 1 × 1
      avg_temp_c
           <dbl>
    1       19.4
    > 
    > #Задание11
    > flights %>%
    + filter(month == 6) %>%
    + count(carrier, sort = TRUE) %>%
    + left_join(airlines, by = "carrier") %>%
    + head(1)
    # A tibble: 1 × 3
      carrier     n name                 
      <chr>   <int> <chr>                
    1 UA       4975 United Air Lines Inc.
    > 
    > #Задание 12
    > flights %>%
    + group_by(carrier) %>%
    + summarise(total_delay = sum(dep_delay, na.rm = TRUE)) %>%
    + left_join(airlines, by = "carrier") %>%
    + arrange(desc(total_delay)) %>%
    + head(1)
    # A tibble: 1 × 3
      carrier total_delay name                  
      <chr>         <dbl> <chr>                 
    1 EV          1024829 ExpressJet Airlines I…
    > flights %>%
    + filter(dep_delay > 0) %>%
    + group_by(carrier) %>%
    + summarise(avg_delay = mean(dep_delay, na.rm = TRUE), n_delayed = n()) %>%
    + left_join(airlines, by = "carrier") %>%
    + arrange(desc(avg_delay)) 
    # A tibble: 16 × 4
       carrier avg_delay n_delayed name         
       <chr>       <dbl>     <int> <chr>        
     1 OO           58           9 SkyWest Airl…
     2 YV           53.0       233 Mesa Airline…
     3 EV           50.3     23139 ExpressJet A…
     4 9E           48.9      7063 Endeavor Air…
     5 F9           45.1       341 Frontier Air…
     6 MQ           44.9      8031 Envoy Air    
     7 HA           44.8        69 Hawaiian Air…
     8 FL           40.8      1654 AirTran Airw…
     9 B6           39.8     21445 JetBlue Airw…
    10 DL           37.4     15241 Delta Air Li…
    11 AA           37.2     10162 American Air…
    12 WN           34.9      6558 Southwest Ai…
    13 VX           34.5      2225 Virgin Ameri…
    14 US           33.1      4775 US Airways I…
    15 AS           31.3       226 Alaska Airli…
    16 UA           29.9     27261 United Air L…
    > 

## Оценка результата

В ходе лабораторной работы мы приобрели практические навыки работы с
функциями пакета dplyr, такими как select(), filter(), mutate(),
arrange() и group_by(), для обработки данных.

## Вывод

В конечном итоге, можно сказать, что мы укрепили понимание основных
типов данных в языке R и улучшили практические навыки работы с функциями
обработки данных из пакета dplyr, включая select(), filter(), mutate(),
arrange() и group_by().
