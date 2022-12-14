Germany Gas Storage
================
Deniz Firat

## PREPARE

### Load needed packages

``` r
library("tidyverse")
library("janitor")
library("lubridate")
library("hms")
library("jsonlite")
```

### Import data sets [(Source)](https://agsi.gie.eu/)

``` r
de <- fromJSON("StorageData_GIE_2016-04-07_2022-09-16.json", flatten = TRUE)
```

### Add new year and month column

``` r
de$year <- format(as.Date(de$gasDayStart), "%Y")
de$month <- format(as.Date(de$gasDayStart), "%m-%d")
```

### Exclude 2022 for calculations

``` r
de_edit <- de[de$year != 2022, ]
```

### Calcuate daily average

``` r
de_average <- de_edit %>% 
    group_by(month) %>% 
    summarise(Mean = mean(full))
```

### Calcuate daily maximum

``` r
de_max <- de_edit %>% 
    group_by(month) %>% 
    summarise(Max = max(full))
```

### Calcuate daily minimum

``` r
de_min <- de_edit %>% 
    group_by(month) %>% 
    summarise(Min = min(full))
```

### Split data set into each year

``` r
de2022 <- de[de$year == 2022, ]
de2021 <- de[de$year == 2021, ]
de2020 <- de[de$year == 2020, ]
de2019 <- de[de$year == 2019, ]
de2018 <- de[de$year == 2018, ]
de2017 <- de[de$year == 2017, ]
```

### Save new data set

``` r
write.csv(de, "de-gas-storage.csv")
write.csv(de_average, "de-gas-storage-average.csv")
write.csv(de_max, "de-gas-storage-max.csv")
write.csv(de_min, "de-gas-storage-min.csv")
write.csv(de2022, "de-gas-storage2022.csv")
write.csv(de2021, "de-gas-storage2021.csv")
write.csv(de2020, "de-gas-storage2020.csv")
write.csv(de2019, "de-gas-storage2019.csv")
write.csv(de2018, "de-gas-storage2018.csv")
write.csv(de2017, "de-gas-storage2017.csv")
```
