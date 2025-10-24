# Основы обработки данных с помощью R и Dplyr
tolmachevnikita04@yandex.ru

## Цель работы:

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Программное обеспечение Windows 10
2.  Rstudio Desktop
3.  Наше рабочее окружение

``` r
sessionInfo()
```

    R version 4.5.1 (2025-06-13 ucrt)
    Platform: x86_64-w64-mingw32/x64
    Running under: Windows 11 x64 (build 26100)

    Matrix products: default
      LAPACK version 3.12.1

    locale:
    [1] LC_COLLATE=Russian_Russia.utf8  LC_CTYPE=Russian_Russia.utf8   
    [3] LC_MONETARY=Russian_Russia.utf8 LC_NUMERIC=C                   
    [5] LC_TIME=Russian_Russia.utf8    

    time zone: Europe/Moscow
    tzcode source: internal

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    loaded via a namespace (and not attached):
     [1] compiler_4.5.1    fastmap_1.2.0     cli_3.6.5         tools_4.5.1      
     [5] htmltools_0.5.8.1 rstudioapi_0.17.1 yaml_2.3.10       rmarkdown_2.29   
     [9] knitr_1.50        jsonlite_2.0.0    xfun_0.53         digest_0.6.37    
    [13] rlang_1.1.6       evaluate_1.0.5   

## План

Используя R и среду разработки RstudioIDE, выполнить задания.

## Шаги:

    > library(dplyr)
    Ошибка в library(dplyr) : нет пакета под названием ‘dplyr’

    > install.packages("dplyr")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Никита/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимости ‘utf8’, ‘pkgconfig’, ‘generics’, ‘pillar’, ‘tibble’, ‘tidyselect’
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/utf8_1.2.6.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/pkgconfig_2.0.3.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/generics_0.1.4.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/pillar_1.11.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/tibble_3.3.0.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/tidyselect_1.2.1.zip'
    пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/dplyr_1.1.4.zip'
    пакет ‘utf8’ успешно распакован, MD5-суммы проверены
    пакет ‘pkgconfig’ успешно распакован, MD5-суммы проверены
    пакет ‘generics’ успешно распакован, MD5-суммы проверены
    пакет ‘pillar’ успешно распакован, MD5-суммы проверены
    пакет ‘tibble’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyselect’ успешно распакован, MD5-суммы проверены
    пакет ‘dplyr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Никита\AppData\Local\Temp\Rtmp6FICuZ\downloaded_packages
    > library(dplyr)

    Присоединяю пакет: ‘dplyr’

    Следующие объекты скрыты от ‘package:stats’:

        filter, lag

    Следующие объекты скрыты от ‘package:base’:

        intersect, setdiff, setequal, union
    > starwars
    # A tibble: 87 × 14
       name       height  mass hair_color skin_color eye_color birth_year sex   gender homeworld
       <chr>       <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr>  <chr>    
     1 Luke Skyw…    172    77 blond      fair       blue            19   male  mascu… Tatooine 
     2 C-3PO         167    75 NA         gold       yellow         112   none  mascu… Tatooine 
     3 R2-D2          96    32 NA         white, bl… red             33   none  mascu… Naboo    
     4 Darth Vad…    202   136 none       white      yellow          41.9 male  mascu… Tatooine 
     5 Leia Orga…    150    49 brown      light      brown           19   fema… femin… Alderaan 
     6 Owen Lars     178   120 brown, gr… light      blue            52   male  mascu… Tatooine 
     7 Beru Whit…    165    75 brown      light      blue            47   fema… femin… Tatooine 
     8 R5-D4          97    32 NA         white, red red             NA   none  mascu… Tatooine 
     9 Biggs Dar…    183    84 black      light      brown           24   male  mascu… Tatooine 
    10 Obi-Wan K…    182    77 auburn, w… fair       blue-gray       57   male  mascu… Stewjon  
    # ℹ 77 more rows
    # ℹ 4 more variables: species <chr>, films <list>, vehicles <list>, starships <list>
    # ℹ Use `print(n = ...)` to see more rows
    > # Задание 1.Количество строк
    > starwars %>% nrow()
    [1] 87
    > # Задание 2. Сколько столбцов в датафрейме?
    > starwars %>% ncol()
    [1] 14
    > 
    > # Задание 3. Как просмотреть примерный вид датафрейма?
    > starwars %>% glimpse()
    Rows: 87
    Columns: 14
    $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Organa", "Owen…
    $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 228, 180, 173…
    $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.0, 84.0, NA,…
    $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", NA, "black", …
    $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "light", "whi…
    $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue", "red", "bro…
    $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, 41.9, 64.0, …
    $ sex        <chr> "male", "none", "none", "male", "female", "male", "female", "none", "ma…
    $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "feminine", "mascul…
    $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "Tatooine", "T…
    $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Human", "Droid",…
    $ films      <list> <"A New Hope", "The Empire Strikes Back", "Return of the Jedi", "Reven…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imperial Speede…
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1", <>, <>, <>,…
    > 
    > # Задание 4. Сколько уникальных рас персонажей (species) представлено в данных?
    > starwars$species %>% unique() %>% length()
    [1] 38
    > 
    > # Задание 5. Найти самого высокого персонажа.
    > starwars %>%
    + select(name,height)
    # A tibble: 87 × 2
       name               height
       <chr>               <int>
     1 Luke Skywalker        172
     2 C-3PO                 167
     3 R2-D2                  96
     4 Darth Vader           202
     5 Leia Organa           150
     6 Owen Lars             178
     7 Beru Whitesun Lars    165
     8 R5-D4                  97
     9 Biggs Darklighter     183
    10 Obi-Wan Kenobi        182
    # ℹ 77 more rows
    # ℹ Use `print(n = ...)` to see more rows
    > arrange(desc(height)) %>%
    + 
    + head(1)
    Ошибка: объект 'height' не найден

    > 
    > 
    > 
    > starwars %>%
    + select(name,height) %>%
    + arrange(desc(height)) %>%
    + head(1)
    # A tibble: 1 × 2
      name        height
      <chr>        <int>
    1 Yarael Poof    264
    > 
    > # Задание 6. Найти всех персонажей ниже 170
    > starwars %>%
    + select(name,height) %>%
    + filter(height <170)
    # A tibble: 22 × 2
       name                  height
       <chr>                  <int>
     1 C-3PO                    167
     2 R2-D2                     96
     3 Leia Organa              150
     4 Beru Whitesun Lars       165
     5 R5-D4                     97
     6 Yoda                      66
     7 Mon Mothma               150
     8 Wicket Systri Warrick     88
     9 Nien Nunb                160
    10 Watto                    137
    # ℹ 12 more rows
    # ℹ Use `print(n = ...)` to see more rows
    > 
    > # Задание 7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по
    > формуле
    Ошибка: объект 'формуле' не найден

    > # Задание 7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по
    > формуле
    Ошибка: объект 'формуле' не найден

    > # Задание 7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать
    > 
    > starwars_with_bmi <- starwars %>%
    + select(name, mass, height) %>% 
    + mutate(height_m = height /100) %>%
    + mutate(bmi = mass / (height_m^2))
    > View(starwars_with_bmi)
    > print(starwars_with_bmi)
    # A tibble: 87 × 5
       name           mass height height_m   bmi
       <chr>         <dbl>  <int>    <dbl> <dbl>
     1 Luke Skywalk…    77    172     1.72  26.0
     2 C-3PO            75    167     1.67  26.9
     3 R2-D2            32     96     0.96  34.7
     4 Darth Vader     136    202     2.02  33.3
     5 Leia Organa      49    150     1.5   21.8
     6 Owen Lars       120    178     1.78  37.9
     7 Beru Whitesu…    75    165     1.65  27.5
     8 R5-D4            32     97     0.97  34.0
     9 Biggs Darkli…    84    183     1.83  25.1
    10 Obi-Wan Keno…    77    182     1.82  23.2
    # ℹ 77 more rows
    # ℹ Use `print(n = ...)` to see more rows
    > 
    > # Задание 8. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению
    > массы (mass) к росту (height) персонажей.
    Ошибка: неожиданный символ в "массы (mass) к"

    > starwars %>% 
    + select(name, mass, height) %>%
    + mutate(mass_to_height = mass / height) %>%
    + arrange(desc(mass_to_height)) %>%
    + head(10)
    # A tibble: 10 × 4
       name           mass height mass_to_height
       <chr>         <dbl>  <int>          <dbl>
     1 Jabba Desili…  1358    175          7.76 
     2 Grievous        159    216          0.736
     3 IG-88           140    200          0.7  
     4 Owen Lars       120    178          0.674
     5 Darth Vader     136    202          0.673
     6 Jek Tono Por…   110    180          0.611
     7 Bossk           113    190          0.595
     8 Tarfful         136    234          0.581
     9 Dexter Jetts…   102    198          0.515
    10 Chewbacca       112    228          0.491
    > 
    > # Задание 9. Найти средний возраст персонажей каждой расы вселенной Звездных войн
    > 
    > starwars %>%
    + group_by(species) %>%
    + summarise(avg_birth_year = mean(birth_year, na.rm = TRUE))
    # A tibble: 38 × 2
       species   avg_birth_year
       <chr>              <dbl>
     1 Aleena             NaN  
     2 Besalisk           NaN  
     3 Cerean              92  
     4 Chagrian           NaN  
     5 Clawdite           NaN  
     6 Droid               53.3
     7 Dug                NaN  
     8 Ewok                 8  
     9 Geonosian          NaN  
    10 Gungan              52  
    # ℹ 28 more rows
    # ℹ Use `print(n = ...)` to see more rows
    > 
    > Задание 10. Найти самый распространенный цвет глаз персонажей вселенной Звездных
    Ошибка: неожиданная числовая константа в "Задание 10."

    > 
    > 
    > starwars %>%
    + count(eye_color, sort = TRUE) %>%
    + HEAD(1)
    Ошибка в HEAD(., 1) : не могу найти функцию "HEAD"

    > starwars %>%
    + count(eye_color, sort = TRUE) %>%
    + head(1)
    # A tibble: 1 × 2
      eye_color     n
      <chr>     <int>
    1 brown        21
    > 
    > Задание 11. Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн
    Ошибка: неожиданная числовая константа в "Задание 11."

    > starwars %>%
    + mutate(name_lenght = nchar(name)) %>%
    + group_by(species) %>%
    + summaries(avg_name_length mean(name_lenght, na.rm =TRUE))
    Ошибка: неожиданный символ в:
    "group_by(species) %>%
    summaries(avg_name_length mean"

    > 
    > starwars %>%
    + mutate(name_lenght = nchar(name)) %>%
    + summarie(avg_name_length = mean(name_lenght, na.rm =TRUE))
    Ошибка в summarie(., avg_name_length = mean(name_lenght, na.rm = TRUE)) : 
      не могу найти функцию "summarie"

    > starwars %>%
    + mutate(name_lenght = nchar(name)) %>%
    + group_by(species) %>%
    + summarise(avg_name_length = mean(name_lenght, na.rm =TRUE))
    # A tibble: 38 × 2
       species   avg_name_length
       <chr>               <dbl>
     1 Aleena              12   
     2 Besalisk            15   
     3 Cerean              12   
     4 Chagrian            10   
     5 Clawdite            10   
     6 Droid                4.83
     7 Dug                  7   
     8 Ewok                21   
     9 Geonosian           17   
    10 Gungan              11.7 
    # ℹ 28 more rows
    # ℹ Use `print(n = ...)` to see more rows

## Оценка результата

В результате лабораторной работы мы развили практические навыки
использования функций обработки данных пакета dplyr – функции select(),
filter(), mutate(), arrange(), group_by()

## Вывод

Таким образом, мы закрепили знания базовых типов данных языка R, а также
развили практические навыки использования функций обработки данных
пакета dplyr – функции select(), filter(), mutate(), arrange(),
group_by()
