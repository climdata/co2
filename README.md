---
title: "CO2"
author: "KMicha71"
date: "19 8 2019"
output:
  html_document: 
    keep_md: true
  pdf_document: default
---



## CO2 Serie


```sh
 [ -f ./download/monthly_in_situ_co2_mlo.csv ] && mv -f ./download/monthly_in_situ_co2_mlo.csv ./download/monthly_in_situ_co2_mlo.csv.bck
 wget -q -P download http://scrippsco2.ucsd.edu/assets/data/atmospheric/stations/in_situ_co2/monthly/monthly_in_situ_co2_mlo.csv
 sed -i '/^"/d' ./download/monthly_in_situ_co2_mlo.csv
 sed -i '2d;3d' ./download/monthly_in_situ_co2_mlo.csv
```




```r
co2 <- read.csv("./download/monthly_in_situ_co2_mlo.csv", sep=",")
names(co2)[names(co2) == "Yr"] <- "year"
names(co2)[names(co2) == "Mn"] <- "month"
co2 <- subset(co2, -99 < co2$CO2)

co2new <-co2[,c("year","month","CO2")]
co2new$time <- signif(co2new$year + (co2new$month-0.5)/12, digits=6)

co2new <- co2new[order(co2new$time),]

write.table(co2new, file = "./csv/monthly_co2.csv", append = FALSE, quote = TRUE, sep = ",",
            eol = "\n", na = "NA", dec = ".", row.names = FALSE,
            col.names = TRUE, qmethod = "escape", fileEncoding = "UTF-8")
```


```r
require("ggplot2")
```

```
## Loading required package: ggplot2
```

```r
mp <- ggplot()
mp + geom_line(aes(y=co2new$CO2, x=co2new$time), color="blue") 
```

![](README_files/figure-html/plot-1.png)<!-- -->
