---
title: "Stringency_Economy"
author: "David Hood"
date: "14/08/2020"
output: 
  html_document: 
    keep_md: yes
---



## Putting some sources together

Just sharing some intitial code that might be useful when thinking about stringency vs "the economy". There are some helper libraries needed:


```r
library(OECD)
```

### Stringency itself

Current csv files downloaded from Oxford and stored in the working directory, if a file is not already present. To download a fresh copy, delete the file from the working directory (this is to be able to work with cached data when you want to then easily update)


```r
if(!file.exists("stringency.csv")){
  download.file(url = "https://raw.githubusercontent.com/OxCGRT/covid-policy-tracker/master/data/OxCGRT_latest.csv",
                destfile = "stringency.csv", method = "libcurl", mode="wb")
}
```

### Covid cases

Data Source is the EU CDC, which is a easy to download spreadsheet, but has irregular revisions occasionally through it because that is what the , and the last few days deaths/cases may not be stable numbers for some countries. 


```r
if(!file.exists("covid.csv")){
  download.file(url = "https://opendata.ecdc.europa.eu/covid19/casedistribution/csv",
                destfile = "covid.csv", method = "libcurl", mode="wb")
}
```

### OECD quarterly unemployment 

Here as an example of using the OECD helper library to get quarterly unemployment data from stats.oecd.org by reading the data into memory with the helper library then saving it as a csv


```r
if(!file.exists("oecd_unemployment.csv")){
  oecd_dataset <- "STLABOUR"
  filter_list <- list(c("AUS", "AUT", "BEL", "CAN", "CHL", "COL", "CZE", "DNK", "EST", "FIN", "FRA", "DEU", "GRC", "HUN", "ISL", "IRL", "ISR", "ITA", "JPN", "KOR", "LVA", "LTU", "LUX", "MEX", "NLD", "NZL", "NOR", "POL", "PRT", "SVK", "SVN", "ESP", "SWE", "CHE", "TUR", "GBR", "USA", "BRA", "IDN", "RUS", "ZAF"),"LRUNTTTT","ST","Q")
  oecd_data <- get_dataset(dataset = oecd_dataset, filter = filter_list,start_time = "2018-Q4", end_time = "2020-Q2")
  write.csv(oecd_data, "oecd_unemployment.csv", row.names = FALSE)
  rm(oecd_data)
}
```

