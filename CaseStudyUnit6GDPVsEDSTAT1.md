# CTaylorMSDS6306402Unit6CaseStudy GDP Vs EDSTAT
Celia Taylor  
June 21, 2016  



#### Case Study of Country GDP and Country Federal Statistics
##### This study is to answer 5 main questions.

##### Question 1	Match the data based on the country shortcode. How many of the IDs match between the data from GDP data file and EDSTAT data file?

#####Question 2	Sort the data frame in ascending order by GDP rank (so United States is last). What is the 13th country in the resulting data frame?

#####Question 3	What are the average GDP rankings for the "High income: OECD" and "High income: nonOECD" groups?

#####Question 4	Plot the GDP for all of the countries. Use ggplot2 to color your plot by Income Group.

#####Question 5	Cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group. How many countries are Lower middle income but among the 38 nations with highest GDP?

#####To answer these questions and learn something about the economics of the countries of the world, the specific data from two different files will be brought together, analyzed, and appropriately handled to show the data.
#####All steps are shown for the integrity of the case study.

#####Download data files, specifically Federal Gross Domestic Product information (GDP) and Federal Statistics by Country (EDSTAT) information.



```r
install.packages("downloader", repos="http://cran.rstudio.com/")
```

```
## Installing package into 'C:/Users/Celia Taylor/Documents/R/win-library/3.2'
## (as 'lib' is unspecified)
```

```
## package 'downloader' successfully unpacked and MD5 sums checked
## 
## The downloaded binary packages are in
## 	C:\Users\Celia Taylor\AppData\Local\Temp\RtmpIvbT7k\downloaded_packages
```

```r
library(downloader)
download("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv", destfile="GDP.csv")
download("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv", destfile="edstat.csv")
#list.files()
```



```r
##### Load packages and functions

#install.packages(c( "plyr",  "brew",  "countrycode",  "devtools", "dplyr", "ggplot2", "googleVis", "knitr", "MCMCpack", "repmis", "RCurl", "rmarkdown", "markdown", "texreg", "tidyr", "WDI", "xtable", "Zelig", "car", "Rtools"), repos="http://cran.rstudio.com/")

#library(plyr)
#library(brew)
#library(countrycode)
#library(devtools)
#library(ggplot2)
#library(googleVis)
#library(knitr) 
#library(MCMCpack) 
#library(repmis) 
#library(RCurl) 
#library(rmarkdown) 
#library(texreg)
#library(tidyr) 
#library(WDI) 
#library(xtable) 
#library(Zelig) 
#library(markdown)
#library(car)
#library(dplyr)
#sessionInfo()
#End of install packages
library(ggplot2)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```
##### Read in edstat.csv file with additional options.

```r
#####
##### Read in edstat.csv file with additional options.  Get rid of any blank lines.
ed <- read.csv("edstat.csv", header = TRUE, sep = ",", stringsAsFactors = FALSE, nrows = 236, blank.lines.skip = TRUE)
str(ed)
```

```
## 'data.frame':	234 obs. of  31 variables:
##  $ CountryCode                                      : chr  "ABW" "ADO" "AFG" "AGO" ...
##  $ Long.Name                                        : chr  "Aruba" "Principality of Andorra" "Islamic State of Afghanistan" "People's Republic of Angola" ...
##  $ Income.Group                                     : chr  "High income: nonOECD" "High income: nonOECD" "Low income" "Lower middle income" ...
##  $ Region                                           : chr  "Latin America & Caribbean" "Europe & Central Asia" "South Asia" "Sub-Saharan Africa" ...
##  $ Lending.category                                 : chr  "" "" "IDA" "IDA" ...
##  $ Other.groups                                     : chr  "" "" "HIPC" "" ...
##  $ Currency.Unit                                    : chr  "Aruban florin" "Euro" "Afghan afghani" "Angolan kwanza" ...
##  $ Latest.population.census                         : chr  "2000" "Register based" "1979" "1970" ...
##  $ Latest.household.survey                          : chr  "" "" "MICS, 2003" "MICS, 2001, MIS, 2006/07" ...
##  $ Special.Notes                                    : chr  "" "" "Fiscal year end: March 20; reporting period for national accounts data: FY." "" ...
##  $ National.accounts.base.year                      : chr  "1995" "" "2002/2003" "1997" ...
##  $ National.accounts.reference.year                 : int  NA NA NA NA 1996 NA NA 1996 NA NA ...
##  $ System.of.National.Accounts                      : int  NA NA NA NA 1993 NA 1993 1993 NA NA ...
##  $ SNA.price.valuation                              : chr  "" "" "VAB" "VAP" ...
##  $ Alternative.conversion.factor                    : chr  "" "" "" "1991-96" ...
##  $ PPP.survey.year                                  : int  NA NA NA 2005 2005 NA 2005 2005 NA NA ...
##  $ Balance.of.Payments.Manual.in.use                : chr  "" "" "" "BPM5" ...
##  $ External.debt.Reporting.status                   : chr  "" "" "Actual" "Actual" ...
##  $ System.of.trade                                  : chr  "Special" "General" "General" "Special" ...
##  $ Government.Accounting.concept                    : chr  "" "" "Consolidated" "" ...
##  $ IMF.data.dissemination.standard                  : chr  "" "" "GDDS" "GDDS" ...
##  $ Source.of.most.recent.Income.and.expenditure.data: chr  "" "" "" "IHS, 2000" ...
##  $ Vital.registration.complete                      : chr  "" "Yes" "" "" ...
##  $ Latest.agricultural.census                       : chr  "" "" "" "1964-65" ...
##  $ Latest.industrial.data                           : int  NA NA NA NA 2005 NA 2001 NA NA NA ...
##  $ Latest.trade.data                                : int  2008 2006 2008 1991 2008 2008 2008 2008 NA 2007 ...
##  $ Latest.water.withdrawal.data                     : int  NA NA 2000 2000 2000 2005 2000 2000 NA 1990 ...
##  $ X2.alpha.code                                    : chr  "AW" "AD" "AF" "AO" ...
##  $ WB.2.code                                        : chr  "AW" "AD" "AF" "AO" ...
##  $ Table.Name                                       : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
##  $ Short.Name                                       : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
```

```r
#Make a backup copy to manipulate
edraw <- ed
#Make sure structure is okay
str(edraw)
```

```
## 'data.frame':	234 obs. of  31 variables:
##  $ CountryCode                                      : chr  "ABW" "ADO" "AFG" "AGO" ...
##  $ Long.Name                                        : chr  "Aruba" "Principality of Andorra" "Islamic State of Afghanistan" "People's Republic of Angola" ...
##  $ Income.Group                                     : chr  "High income: nonOECD" "High income: nonOECD" "Low income" "Lower middle income" ...
##  $ Region                                           : chr  "Latin America & Caribbean" "Europe & Central Asia" "South Asia" "Sub-Saharan Africa" ...
##  $ Lending.category                                 : chr  "" "" "IDA" "IDA" ...
##  $ Other.groups                                     : chr  "" "" "HIPC" "" ...
##  $ Currency.Unit                                    : chr  "Aruban florin" "Euro" "Afghan afghani" "Angolan kwanza" ...
##  $ Latest.population.census                         : chr  "2000" "Register based" "1979" "1970" ...
##  $ Latest.household.survey                          : chr  "" "" "MICS, 2003" "MICS, 2001, MIS, 2006/07" ...
##  $ Special.Notes                                    : chr  "" "" "Fiscal year end: March 20; reporting period for national accounts data: FY." "" ...
##  $ National.accounts.base.year                      : chr  "1995" "" "2002/2003" "1997" ...
##  $ National.accounts.reference.year                 : int  NA NA NA NA 1996 NA NA 1996 NA NA ...
##  $ System.of.National.Accounts                      : int  NA NA NA NA 1993 NA 1993 1993 NA NA ...
##  $ SNA.price.valuation                              : chr  "" "" "VAB" "VAP" ...
##  $ Alternative.conversion.factor                    : chr  "" "" "" "1991-96" ...
##  $ PPP.survey.year                                  : int  NA NA NA 2005 2005 NA 2005 2005 NA NA ...
##  $ Balance.of.Payments.Manual.in.use                : chr  "" "" "" "BPM5" ...
##  $ External.debt.Reporting.status                   : chr  "" "" "Actual" "Actual" ...
##  $ System.of.trade                                  : chr  "Special" "General" "General" "Special" ...
##  $ Government.Accounting.concept                    : chr  "" "" "Consolidated" "" ...
##  $ IMF.data.dissemination.standard                  : chr  "" "" "GDDS" "GDDS" ...
##  $ Source.of.most.recent.Income.and.expenditure.data: chr  "" "" "" "IHS, 2000" ...
##  $ Vital.registration.complete                      : chr  "" "Yes" "" "" ...
##  $ Latest.agricultural.census                       : chr  "" "" "" "1964-65" ...
##  $ Latest.industrial.data                           : int  NA NA NA NA 2005 NA 2001 NA NA NA ...
##  $ Latest.trade.data                                : int  2008 2006 2008 1991 2008 2008 2008 2008 NA 2007 ...
##  $ Latest.water.withdrawal.data                     : int  NA NA 2000 2000 2000 2005 2000 2000 NA 1990 ...
##  $ X2.alpha.code                                    : chr  "AW" "AD" "AF" "AO" ...
##  $ WB.2.code                                        : chr  "AW" "AD" "AF" "AO" ...
##  $ Table.Name                                       : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
##  $ Short.Name                                       : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
```

```r
head(edraw,2)
```

```
##   CountryCode               Long.Name         Income.Group
## 1         ABW                   Aruba High income: nonOECD
## 2         ADO Principality of Andorra High income: nonOECD
##                      Region Lending.category Other.groups Currency.Unit
## 1 Latin America & Caribbean                               Aruban florin
## 2     Europe & Central Asia                                        Euro
##   Latest.population.census Latest.household.survey Special.Notes
## 1                     2000                                      
## 2           Register based                                      
##   National.accounts.base.year National.accounts.reference.year
## 1                        1995                               NA
## 2                                                           NA
##   System.of.National.Accounts SNA.price.valuation
## 1                          NA                    
## 2                          NA                    
##   Alternative.conversion.factor PPP.survey.year
## 1                                            NA
## 2                                            NA
##   Balance.of.Payments.Manual.in.use External.debt.Reporting.status
## 1                                                                 
## 2                                                                 
##   System.of.trade Government.Accounting.concept
## 1         Special                              
## 2         General                              
##   IMF.data.dissemination.standard
## 1                                
## 2                                
##   Source.of.most.recent.Income.and.expenditure.data
## 1                                                  
## 2                                                  
##   Vital.registration.complete Latest.agricultural.census
## 1                                                       
## 2                         Yes                           
##   Latest.industrial.data Latest.trade.data Latest.water.withdrawal.data
## 1                     NA              2008                           NA
## 2                     NA              2006                           NA
##   X2.alpha.code WB.2.code Table.Name Short.Name
## 1            AW        AW      Aruba      Aruba
## 2            AD        AD    Andorra    Andorra
```

```r
tail(edraw,2)
```

```
##     CountryCode            Long.Name Income.Group             Region
## 233         ZMB   Republic of Zambia   Low income Sub-Saharan Africa
## 234         ZWE Republic of Zimbabwe   Low income Sub-Saharan Africa
##     Lending.category Other.groups   Currency.Unit Latest.population.census
## 233              IDA         HIPC  Zambian kwacha                     2000
## 234            Blend              Zimbabwe dollar                     2002
##     Latest.household.survey
## 233               DHS, 2007
## 234            DHS, 2005/06
##                                                                  Special.Notes
## 233                                                                           
## 234 Fiscal year end: June 30; reporting period for national accounts data: CY.
##     National.accounts.base.year National.accounts.reference.year
## 233                        1994                               NA
## 234                        1990                               NA
##     System.of.National.Accounts SNA.price.valuation
## 233                          NA                 VAB
## 234                          NA                 VAB
##     Alternative.conversion.factor PPP.survey.year
## 233                       1990-92            2005
## 234                    1991, 1998            2005
##     Balance.of.Payments.Manual.in.use External.debt.Reporting.status
## 233                              BPM5                    Preliminary
## 234                              BPM5                         Actual
##     System.of.trade Government.Accounting.concept
## 233         General                     Budgetary
## 234         General                  Consolidated
##     IMF.data.dissemination.standard
## 233                            GDDS
## 234                            GDDS
##     Source.of.most.recent.Income.and.expenditure.data
## 233                                      IHS, 2004-05
## 234                                                  
##     Vital.registration.complete Latest.agricultural.census
## 233                                                   1990
## 234                                                   1960
##     Latest.industrial.data Latest.trade.data Latest.water.withdrawal.data
## 233                     NA              2008                         2000
## 234                   1995              2008                         2002
##     X2.alpha.code WB.2.code Table.Name Short.Name
## 233            ZM        ZM     Zambia     Zambia
## 234            ZW        ZW   Zimbabwe   Zimbabwe
```
##### Read in GDP.csv file with additional options.

```r
##### Read in GDP.csv file with additional options.
gdp <- read.csv("GDP.csv", header = FALSE, sep = ",", stringsAsFactors=FALSE, nrows = 236, blank.lines.skip = TRUE)

#Make a backup copy to manipulate
gdpraw <- gdp
#Make sure structure is okay
str(gdpraw)
```

```
## 'data.frame':	236 obs. of  10 variables:
##  $ V1 : chr  "" "" "" "" ...
##  $ V2 : chr  "Gross domestic product 2012" "" "" "Ranking" ...
##  $ V3 : logi  NA NA NA NA NA NA ...
##  $ V4 : chr  "" "" "" "Economy" ...
##  $ V5 : chr  "" "" "(millions of" "US dollars)" ...
##  $ V6 : chr  "" "" "" "" ...
##  $ V7 : logi  NA NA NA NA NA NA ...
##  $ V8 : logi  NA NA NA NA NA NA ...
##  $ V9 : logi  NA NA NA NA NA NA ...
##  $ V10: logi  NA NA NA NA NA NA ...
```

```r
head(gdpraw,2)
```

```
##   V1                          V2 V3 V4 V5 V6 V7 V8 V9 V10
## 1    Gross domestic product 2012 NA          NA NA NA  NA
## 2                                NA          NA NA NA  NA
```

```r
tail(gdpraw,2)
```

```
##      V1 V2 V3          V4         V5 V6 V7 V8 V9 V10
## 235 HIC    NA High income 49,717,634    NA NA NA  NA
## 236 EMU    NA   Euro area 12,192,344    NA NA NA  NA
```
##### Keep only the columns in edraw that are needed for analysis

```r
##### Keep only the columns in edraw that are needed for analysis
edraw1 <- edraw[c(1,3,31)]
str(edraw1)
```

```
## 'data.frame':	234 obs. of  3 variables:
##  $ CountryCode : chr  "ABW" "ADO" "AFG" "AGO" ...
##  $ Income.Group: chr  "High income: nonOECD" "High income: nonOECD" "Low income" "Lower middle income" ...
##  $ Short.Name  : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
```
##### Keep only the columns in gdpraw that are needed for analysis

```r
##### Keep only the columns in gdpraw that are needed for analysis
gdpraw1 <- gdpraw[c(1,2,4,5)]
str(gdpraw1)
```

```
## 'data.frame':	236 obs. of  4 variables:
##  $ V1: chr  "" "" "" "" ...
##  $ V2: chr  "Gross domestic product 2012" "" "" "Ranking" ...
##  $ V4: chr  "" "" "" "Economy" ...
##  $ V5: chr  "" "" "(millions of" "US dollars)" ...
```
##### Rename headers in edraw to good and descriptive names without spaces

```r
###### Rename headers in edraw to good and descriptive names without spaces
edraw2 <- edraw1
names(edraw2) <- c("EdCountryCode", "IncomeGroup", "ShortName")
str(edraw2)
```

```
## 'data.frame':	234 obs. of  3 variables:
##  $ EdCountryCode: chr  "ABW" "ADO" "AFG" "AGO" ...
##  $ IncomeGroup  : chr  "High income: nonOECD" "High income: nonOECD" "Low income" "Lower middle income" ...
##  $ ShortName    : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
```
##### Rename headers in gdpraw to good and descriptive names without spaces

```r
##### Rename headers in gdpraw to good and descriptive names without spaces
str(gdpraw1)
```

```
## 'data.frame':	236 obs. of  4 variables:
##  $ V1: chr  "" "" "" "" ...
##  $ V2: chr  "Gross domestic product 2012" "" "" "Ranking" ...
##  $ V4: chr  "" "" "" "Economy" ...
##  $ V5: chr  "" "" "(millions of" "US dollars)" ...
```

```r
head(gdpraw1, 2)
```

```
##   V1                          V2 V4 V5
## 1    Gross domestic product 2012      
## 2
```

```r
#Get rid of top rows that do not have good information
gdpraw2 <- gdpraw1[6:236,]
names(gdpraw2) <- c("GdpCountryCode", "Ranking", "Economy", "MillionsOfDollars")
str(gdpraw2)
```

```
## 'data.frame':	231 obs. of  4 variables:
##  $ GdpCountryCode   : chr  "USA" "CHN" "JPN" "DEU" ...
##  $ Ranking          : chr  "1" "2" "3" "4" ...
##  $ Economy          : chr  "United States" "China" "Japan" "Germany" ...
##  $ MillionsOfDollars: chr  " 16,244,600 " " 8,227,103 " " 5,959,718 " " 3,428,131 " ...
```

```r
head(gdpraw2, 2)
```

```
##   GdpCountryCode Ranking       Economy MillionsOfDollars
## 6            USA       1 United States       16,244,600 
## 7            CHN       2         China        8,227,103
```

```r
tail(gdpraw2, 2)
```

```
##     GdpCountryCode Ranking     Economy MillionsOfDollars
## 235            HIC         High income        49,717,634
## 236            EMU           Euro area        12,192,344
```

##### Keep only the rows in gdpraw1 that are needed for analysis

```r
#Lines 6 through 195 have good information on Ranking and MillionsOfDollars, 
#so make a data frame that concentrates on that data with the added benefit of
#eliminating extraneous and possible troublesome (NAs) data.
gdpAnal1 <- gdpraw1[6:195,]
names(gdpAnal1) <- c("GdpCountryCode", "Ranking", "Economy", "MillionsOfDollars")
str(gdpAnal1)
```

```
## 'data.frame':	190 obs. of  4 variables:
##  $ GdpCountryCode   : chr  "USA" "CHN" "JPN" "DEU" ...
##  $ Ranking          : chr  "1" "2" "3" "4" ...
##  $ Economy          : chr  "United States" "China" "Japan" "Germany" ...
##  $ MillionsOfDollars: chr  " 16,244,600 " " 8,227,103 " " 5,959,718 " " 3,428,131 " ...
```

```r
#Force "Ranking" to be an integer for easier sorting
gdpAnal1$Ranking <- as.integer(gdpAnal1$Ranking)
#Check structure and make sure integer conversion worked.
str(gdpAnal1)
```

```
## 'data.frame':	190 obs. of  4 variables:
##  $ GdpCountryCode   : chr  "USA" "CHN" "JPN" "DEU" ...
##  $ Ranking          : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Economy          : chr  "United States" "China" "Japan" "Germany" ...
##  $ MillionsOfDollars: chr  " 16,244,600 " " 8,227,103 " " 5,959,718 " " 3,428,131 " ...
```

```r
head(gdpAnal1, 2)
```

```
##   GdpCountryCode Ranking       Economy MillionsOfDollars
## 6            USA       1 United States       16,244,600 
## 7            CHN       2         China        8,227,103
```

```r
tail(gdpAnal1, 2)
```

```
##     GdpCountryCode Ranking  Economy MillionsOfDollars
## 194            KIR     189 Kiribati              175 
## 195            TUV     190   Tuvalu               40
```

#####Question 1	Match the data based on the country shortcode. How many of the IDs match? 
##### Merge data frames by CountryCode

```r
#This is the data frame that has the larger set to find all matching country shortcode IDs from the #from column 1 of GDP data and column 1 of the edstats data.
MergeEdGdp4 <- merge(edraw2, gdpraw2, by.x = "EdCountryCode", by.y = "GdpCountryCode", sort = TRUE, all = FALSE, incomparables = NULL)
str(MergeEdGdp4)
```

```
## 'data.frame':	224 obs. of  6 variables:
##  $ EdCountryCode    : chr  "ABW" "ADO" "AFG" "AGO" ...
##  $ IncomeGroup      : chr  "High income: nonOECD" "High income: nonOECD" "Low income" "Lower middle income" ...
##  $ ShortName        : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
##  $ Ranking          : chr  "161" "" "105" "60" ...
##  $ Economy          : chr  "Aruba" "Andorra" "Afghanistan" "Angola" ...
##  $ MillionsOfDollars: chr  " 2,584 " ".." " 20,497 " " 114,147 " ...
```

```r
head(MergeEdGdp4, 2)
```

```
##   EdCountryCode          IncomeGroup ShortName Ranking Economy
## 1           ABW High income: nonOECD     Aruba     161   Aruba
## 2           ADO High income: nonOECD   Andorra         Andorra
##   MillionsOfDollars
## 1            2,584 
## 2                ..
```

```r
tail(MergeEdGdp4, 2)
```

```
##     EdCountryCode IncomeGroup ShortName Ranking  Economy MillionsOfDollars
## 223           ZMB  Low income    Zambia     104   Zambia           20,678 
## 224           ZWE  Low income  Zimbabwe     134 Zimbabwe            9,802
```
##### There are 224 matching IDs or country shortcodes.  This includes regional or economic country shortcode IDs.

#####Question 2	Sort the data frame in ascending order by GDP rank (so United States is last). What is the 13th country in the resulting data frame?

```r
#Utilize that gdpAnal1 data frame that has the 190 countries with good Ranking 
#and MillionsOfDollars
#SSD - South Sudan doesn't match because not in edstat data.
SortGdpRank189 <- merge(edraw2, gdpAnal1, by.x = "EdCountryCode", by.y = "GdpCountryCode", sort = TRUE, all = FALSE, incomparables = NULL)
#Keep SSD - South Sudan in data frame to utilize Ranking and MillionsOfDollars data.
SortGdpRank <- merge(edraw2, gdpAnal1, by.x = "EdCountryCode", by.y = "GdpCountryCode", sort = TRUE, all.y = TRUE, incomparables = NULL)
#Make sure structure is OK and all 190 countries are there.
str(SortGdpRank189)
```

```
## 'data.frame':	189 obs. of  6 variables:
##  $ EdCountryCode    : chr  "ABW" "AFG" "AGO" "ALB" ...
##  $ IncomeGroup      : chr  "High income: nonOECD" "Low income" "Lower middle income" "Upper middle income" ...
##  $ ShortName        : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ Ranking          : int  161 105 60 125 32 26 133 172 12 27 ...
##  $ Economy          : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ MillionsOfDollars: chr  " 2,584 " " 20,497 " " 114,147 " " 12,648 " ...
```

```r
str(SortGdpRank)
```

```
## 'data.frame':	190 obs. of  6 variables:
##  $ EdCountryCode    : chr  "ABW" "AFG" "AGO" "ALB" ...
##  $ IncomeGroup      : chr  "High income: nonOECD" "Low income" "Lower middle income" "Upper middle income" ...
##  $ ShortName        : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ Ranking          : int  161 105 60 125 32 26 133 172 12 27 ...
##  $ Economy          : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ MillionsOfDollars: chr  " 2,584 " " 20,497 " " 114,147 " " 12,648 " ...
```

```r
head(SortGdpRank, 2)
```

```
##   EdCountryCode          IncomeGroup   ShortName Ranking     Economy
## 1           ABW High income: nonOECD       Aruba     161       Aruba
## 2           AFG           Low income Afghanistan     105 Afghanistan
##   MillionsOfDollars
## 1            2,584 
## 2           20,497
```

```r
tail(SortGdpRank, 2)
```

```
##     EdCountryCode IncomeGroup ShortName Ranking  Economy MillionsOfDollars
## 189           ZMB  Low income    Zambia     104   Zambia           20,678 
## 190           ZWE  Low income  Zimbabwe     134 Zimbabwe            9,802
```

```r
#Show SSD South Sudan as list number 155 with NA in IncomGroup.
SortGdpRank
```

```
##     EdCountryCode          IncomeGroup                      ShortName
## 1             ABW High income: nonOECD                          Aruba
## 2             AFG           Low income                    Afghanistan
## 3             AGO  Lower middle income                         Angola
## 4             ALB  Upper middle income                        Albania
## 5             ARE High income: nonOECD           United Arab Emirates
## 6             ARG  Upper middle income                      Argentina
## 7             ARM  Lower middle income                        Armenia
## 8             ATG  Upper middle income            Antigua and Barbuda
## 9             AUS    High income: OECD                      Australia
## 10            AUT    High income: OECD                        Austria
## 11            AZE  Upper middle income                     Azerbaijan
## 12            BDI           Low income                        Burundi
## 13            BEL    High income: OECD                        Belgium
## 14            BEN           Low income                          Benin
## 15            BFA           Low income                   Burkina Faso
## 16            BGD           Low income                     Bangladesh
## 17            BGR  Upper middle income                       Bulgaria
## 18            BHR High income: nonOECD                        Bahrain
## 19            BHS High income: nonOECD                    The Bahamas
## 20            BIH  Upper middle income         Bosnia and Herzegovina
## 21            BLR  Upper middle income                        Belarus
## 22            BLZ  Lower middle income                         Belize
## 23            BMU High income: nonOECD                        Bermuda
## 24            BOL  Lower middle income                        Bolivia
## 25            BRA  Upper middle income                         Brazil
## 26            BRB High income: nonOECD                       Barbados
## 27            BRN High income: nonOECD                         Brunei
## 28            BTN  Lower middle income                         Bhutan
## 29            BWA  Upper middle income                       Botswana
## 30            CAF           Low income       Central African Republic
## 31            CAN    High income: OECD                         Canada
## 32            CHE    High income: OECD                    Switzerland
## 33            CHL  Upper middle income                          Chile
## 34            CHN  Lower middle income                          China
## 35            CIV  Lower middle income                  Côte d'Ivoire
## 36            CMR  Lower middle income                       Cameroon
## 37            COG  Lower middle income                          Congo
## 38            COL  Upper middle income                       Colombia
## 39            COM           Low income                        Comoros
## 40            CPV  Lower middle income                     Cape Verde
## 41            CRI  Upper middle income                     Costa Rica
## 42            CUB  Upper middle income                           Cuba
## 43            CYP High income: nonOECD                         Cyprus
## 44            CZE    High income: OECD                 Czech Republic
## 45            DEU    High income: OECD                        Germany
## 46            DMA  Upper middle income                       Dominica
## 47            DNK    High income: OECD                        Denmark
## 48            DOM  Upper middle income             Dominican Republic
## 49            DZA  Upper middle income                        Algeria
## 50            ECU  Lower middle income                        Ecuador
## 51            EGY  Lower middle income                          Egypt
## 52            ERI           Low income                        Eritrea
## 53            ESP    High income: OECD                          Spain
## 54            EST High income: nonOECD                        Estonia
## 55            ETH           Low income                       Ethiopia
## 56            FIN    High income: OECD                        Finland
## 57            FJI  Upper middle income                           Fiji
## 58            FRA    High income: OECD                         France
## 59            FSM  Lower middle income                     Micronesia
## 60            GAB  Upper middle income                          Gabon
## 61            GBR    High income: OECD                 United Kingdom
## 62            GEO  Lower middle income                        Georgia
## 63            GHA           Low income                          Ghana
## 64            GIN           Low income                         Guinea
## 65            GMB           Low income                     The Gambia
## 66            GNB           Low income                  Guinea-Bissau
## 67            GNQ High income: nonOECD              Equatorial Guinea
## 68            GRC    High income: OECD                         Greece
## 69            GRD  Upper middle income                        Grenada
## 70            GTM  Lower middle income                      Guatemala
## 71            GUY  Lower middle income                         Guyana
## 72            HKG High income: nonOECD           Hong Kong SAR, China
## 73            HND  Lower middle income                       Honduras
## 74            HRV High income: nonOECD                        Croatia
## 75            HTI           Low income                          Haiti
## 76            HUN    High income: OECD                        Hungary
## 77            IDN  Lower middle income                      Indonesia
## 78            IND  Lower middle income                          India
## 79            IRL    High income: OECD                        Ireland
## 80            IRN  Upper middle income                           Iran
## 81            IRQ  Lower middle income                           Iraq
## 82            ISL    High income: OECD                        Iceland
## 83            ISR    High income: OECD                         Israel
## 84            ITA    High income: OECD                          Italy
## 85            JAM  Upper middle income                        Jamaica
## 86            JOR  Lower middle income                         Jordan
## 87            JPN    High income: OECD                          Japan
## 88            KAZ  Upper middle income                     Kazakhstan
## 89            KEN           Low income                          Kenya
## 90            KGZ           Low income                Kyrgyz Republic
## 91            KHM           Low income                       Cambodia
## 92            KIR  Lower middle income                       Kiribati
## 93            KNA  Upper middle income            St. Kitts and Nevis
## 94            KOR    High income: OECD                          Korea
## 95            KSV  Lower middle income                         Kosovo
## 96            KWT High income: nonOECD                         Kuwait
## 97            LAO           Low income                        Lao PDR
## 98            LBN  Upper middle income                        Lebanon
## 99            LBR           Low income                        Liberia
## 100           LCA  Upper middle income                      St. Lucia
## 101           LKA  Lower middle income                      Sri Lanka
## 102           LSO  Lower middle income                        Lesotho
## 103           LTU  Upper middle income                      Lithuania
## 104           LUX    High income: OECD                     Luxembourg
## 105           LVA High income: nonOECD                         Latvia
## 106           MAC High income: nonOECD               Macao SAR, China
## 107           MAR  Lower middle income                        Morocco
## 108           MCO High income: nonOECD                         Monaco
## 109           MDA  Lower middle income                        Moldova
## 110           MDG           Low income                     Madagascar
## 111           MDV  Lower middle income                       Maldives
## 112           MEX  Upper middle income                         Mexico
## 113           MHL  Lower middle income               Marshall Islands
## 114           MKD  Upper middle income                      Macedonia
## 115           MLI           Low income                           Mali
## 116           MLT High income: nonOECD                          Malta
## 117           MNE  Upper middle income                     Montenegro
## 118           MNG  Lower middle income                       Mongolia
## 119           MOZ           Low income                     Mozambique
## 120           MRT           Low income                     Mauritania
## 121           MUS  Upper middle income                      Mauritius
## 122           MWI           Low income                         Malawi
## 123           MYS  Upper middle income                       Malaysia
## 124           NAM  Upper middle income                        Namibia
## 125           NER           Low income                          Niger
## 126           NGA  Lower middle income                        Nigeria
## 127           NIC  Lower middle income                      Nicaragua
## 128           NLD    High income: OECD                    Netherlands
## 129           NOR    High income: OECD                         Norway
## 130           NPL           Low income                          Nepal
## 131           NZL    High income: OECD                    New Zealand
## 132           OMN High income: nonOECD                           Oman
## 133           PAK  Lower middle income                       Pakistan
## 134           PAN  Upper middle income                         Panama
## 135           PER  Upper middle income                           Peru
## 136           PHL  Lower middle income                    Philippines
## 137           PLW  Upper middle income                          Palau
## 138           PNG  Lower middle income               Papua New Guinea
## 139           POL    High income: OECD                         Poland
## 140           PRI High income: nonOECD                    Puerto Rico
## 141           PRT    High income: OECD                       Portugal
## 142           PRY  Lower middle income                       Paraguay
## 143           QAT High income: nonOECD                          Qatar
## 144           ROM  Upper middle income                        Romania
## 145           RUS  Upper middle income                         Russia
## 146           RWA           Low income                         Rwanda
## 147           SAU High income: nonOECD                   Saudi Arabia
## 148           SDN  Lower middle income                          Sudan
## 149           SEN  Lower middle income                        Senegal
## 150           SGP High income: nonOECD                      Singapore
## 151           SLB           Low income                Solomon Islands
## 152           SLE           Low income                   Sierra Leone
## 153           SLV  Lower middle income                    El Salvador
## 154           SRB  Upper middle income                         Serbia
## 155           SSD                 <NA>                           <NA>
## 156           STP  Lower middle income          São Tomé and Principe
## 157           SUR  Upper middle income                       Suriname
## 158           SVK    High income: OECD                Slovak Republic
## 159           SVN    High income: OECD                       Slovenia
## 160           SWE    High income: OECD                         Sweden
## 161           SWZ  Lower middle income                      Swaziland
## 162           SYC  Upper middle income                     Seychelles
## 163           SYR  Lower middle income           Syrian Arab Republic
## 164           TCD           Low income                           Chad
## 165           TGO           Low income                           Togo
## 166           THA  Lower middle income                       Thailand
## 167           TJK           Low income                     Tajikistan
## 168           TKM  Lower middle income                   Turkmenistan
## 169           TMP  Lower middle income                    Timor-Leste
## 170           TON  Lower middle income                          Tonga
## 171           TTO High income: nonOECD            Trinidad and Tobago
## 172           TUN  Lower middle income                        Tunisia
## 173           TUR  Upper middle income                         Turkey
## 174           TUV  Lower middle income                         Tuvalu
## 175           TZA           Low income                       Tanzania
## 176           UGA           Low income                         Uganda
## 177           UKR  Lower middle income                        Ukraine
## 178           URY  Upper middle income                        Uruguay
## 179           USA    High income: OECD                  United States
## 180           UZB  Lower middle income                     Uzbekistan
## 181           VCT  Upper middle income St. Vincent and the Grenadines
## 182           VEN  Upper middle income                      Venezuela
## 183           VNM  Lower middle income                        Vietnam
## 184           VUT  Lower middle income                        Vanuatu
## 185           WSM  Lower middle income                          Samoa
## 186           YEM  Lower middle income                          Yemen
## 187           ZAF  Upper middle income                   South Africa
## 188           ZAR           Low income                Dem. Rep. Congo
## 189           ZMB           Low income                         Zambia
## 190           ZWE           Low income                       Zimbabwe
##     Ranking                        Economy MillionsOfDollars
## 1       161                          Aruba            2,584 
## 2       105                    Afghanistan           20,497 
## 3        60                         Angola          114,147 
## 4       125                        Albania           12,648 
## 5        32           United Arab Emirates          348,595 
## 6        26                      Argentina          475,502 
## 7       133                        Armenia            9,951 
## 8       172            Antigua and Barbuda            1,134 
## 9        12                      Australia        1,532,408 
## 10       27                        Austria          394,708 
## 11       68                     Azerbaijan           66,605 
## 12      162                        Burundi            2,472 
## 13       25                        Belgium          483,262 
## 14      140                          Benin            7,557 
## 15      128                   Burkina Faso           10,441 
## 16       59                     Bangladesh          116,355 
## 17       76                       Bulgaria           50,972 
## 18       93                        Bahrain           29,044 
## 19      138                   Bahamas, The            8,149 
## 20      111         Bosnia and Herzegovina           17,466 
## 21       69                        Belarus           63,267 
## 22      169                         Belize            1,493 
## 23      149                        Bermuda            5,474 
## 24       96                        Bolivia           27,035 
## 25        7                         Brazil        2,252,664 
## 26      153                       Barbados            4,225 
## 27      113              Brunei Darussalam           16,954 
## 28      167                         Bhutan            1,780 
## 29      117                       Botswana           14,504 
## 30      165       Central African Republic            2,184 
## 31       11                         Canada        1,821,424 
## 32       20                    Switzerland          631,173 
## 33       36                          Chile          269,869 
## 34        2                          China        8,227,103 
## 35       99                  Côte d'Ivoire           24,680 
## 36       98                       Cameroon           25,322 
## 37      121                    Congo, Rep.           13,678 
## 38       30                       Colombia          369,606 
## 39      182                        Comoros              596 
## 40      166                     Cape Verde            1,827 
## 41       81                     Costa Rica           45,104 
## 42       67                           Cuba           68,234 
## 43      102                         Cyprus           22,767 
## 44       51                 Czech Republic          196,446 
## 45        4                        Germany        3,428,131 
## 46      183                       Dominica              480 
## 47       33                        Denmark          314,887 
## 48       72             Dominican Republic           59,047 
## 49       48                        Algeria          205,789 
## 50       64                        Ecuador           84,040 
## 51       38               Egypt, Arab Rep.          262,832 
## 52      159                        Eritrea            3,092 
## 53       13                          Spain        1,322,965 
## 54      103                        Estonia           22,390 
## 55       85                       Ethiopia           41,605 
## 56       43                        Finland          247,546 
## 57      155                           Fiji            3,908 
## 58        5                         France        2,612,878 
## 59      185          Micronesia, Fed. Sts.              326 
## 60      109                          Gabon           18,377 
## 61        6                 United Kingdom        2,471,784 
## 62      114                        Georgia           15,747 
## 63       86                          Ghana           40,711 
## 64      148                         Guinea            5,632 
## 65      175                    Gambia, The              917 
## 66      176                  Guinea-Bissau              822 
## 67      110              Equatorial Guinea           17,697 
## 68       42                         Greece          249,099 
## 69      178                        Grenada              767 
## 70       77                      Guatemala           50,234 
## 71      160                         Guyana            2,851 
## 72       37           Hong Kong SAR, China          263,259 
## 73      108                       Honduras           18,434 
## 74       71                        Croatia           59,228 
## 75      139                          Haiti            7,843 
## 76       58                        Hungary          124,600 
## 77       16                      Indonesia          878,043 
## 78       10                          India        1,841,710 
## 79       46                        Ireland          210,771 
## 80       22             Iran, Islamic Rep.          514,060 
## 81       47                           Iraq          210,280 
## 82      122                        Iceland           13,579 
## 83       40                         Israel          258,217 
## 84        9                          Italy        2,014,670 
## 85      116                        Jamaica           14,755 
## 86       92                         Jordan           31,015 
## 87        3                          Japan        5,959,718 
## 88       50                     Kazakhstan          203,521 
## 89       87                          Kenya           40,697 
## 90      145                Kyrgyz Republic            6,475 
## 91      120                       Cambodia           14,038 
## 92      189                       Kiribati              175 
## 93      178            St. Kitts and Nevis              767 
## 94       15                    Korea, Rep.        1,129,598 
## 95      146                         Kosovo            6,445 
## 96       56                         Kuwait          160,913 
## 97      136                        Lao PDR            9,418 
## 98       83                        Lebanon           42,945 
## 99      168                        Liberia            1,734 
## 100     171                      St. Lucia            1,239 
## 101      70                      Sri Lanka           59,423 
## 102     163                        Lesotho            2,448 
## 103      84                      Lithuania           42,344 
## 104      74                     Luxembourg           55,178 
## 105      94                         Latvia           28,373 
## 106      82               Macao SAR, China           43,582 
## 107      62                        Morocco           95,982 
## 108     147                         Monaco            6,075 
## 109     141                        Moldova            7,253 
## 110     132                     Madagascar            9,975 
## 111     164                       Maldives            2,222 
## 112      14                         Mexico        1,178,126 
## 113     188               Marshall Islands              182 
## 114     135                 Macedonia, FYR            9,613 
## 115     129                           Mali           10,308 
## 116     137                          Malta            8,722 
## 117     151                     Montenegro            4,373 
## 118     130                       Mongolia           10,271 
## 119     118                     Mozambique           14,244 
## 120     154                     Mauritania            4,199 
## 121     127                      Mauritius           10,486 
## 122     152                         Malawi            4,264 
## 123      34                       Malaysia          305,033 
## 124     123                        Namibia           13,072 
## 125     144                          Niger            6,773 
## 126      39                        Nigeria          262,597 
## 127     126                      Nicaragua           10,507 
## 128      18                    Netherlands          770,555 
## 129      23                         Norway          499,667 
## 130     107                          Nepal           18,963 
## 131      55                    New Zealand          167,347 
## 132      66                           Oman           69,972 
## 133      44                       Pakistan          225,143 
## 134      89                         Panama           36,253 
## 135      49                           Peru          203,790 
## 136      41                    Philippines          250,182 
## 137     187                          Palau              228 
## 138     115               Papua New Guinea           15,654 
## 139      24                         Poland          489,795 
## 140      61                    Puerto Rico          101,496 
## 141      45                       Portugal          212,274 
## 142      97                       Paraguay           25,502 
## 143      54                          Qatar          171,476 
## 144      52                        Romania          192,711 
## 145       8             Russian Federation        2,014,775 
## 146     142                         Rwanda            7,103 
## 147      19                   Saudi Arabia          711,050 
## 148      73                          Sudan           58,769 
## 149     119                        Senegal           14,046 
## 150      35                      Singapore          274,701 
## 151     174                Solomon Islands            1,008 
## 152     157                   Sierra Leone            3,796 
## 153     100                    El Salvador           23,864 
## 154      88                         Serbia           37,489 
## 155     131                    South Sudan           10,220 
## 156     186          São Tomé and Principe              263 
## 157     150                       Suriname            5,012 
## 158      63                Slovak Republic           91,149 
## 159      80                       Slovenia           45,279 
## 160      21                         Sweden          523,806 
## 161     158                      Swaziland            3,744 
## 162     173                     Seychelles            1,129 
## 163      65           Syrian Arab Republic           73,672 
## 164     124                           Chad           12,887 
## 165     156                           Togo            3,814 
## 166      31                       Thailand          365,966 
## 167     143                     Tajikistan            6,972 
## 168      91                   Turkmenistan           35,164 
## 169     170                    Timor-Leste            1,293 
## 170     184                          Tonga              472 
## 171     101            Trinidad and Tobago           23,320 
## 172      79                        Tunisia           45,662 
## 173      17                         Turkey          789,257 
## 174     190                         Tuvalu               40 
## 175      95                       Tanzania           28,242 
## 176     106                         Uganda           19,881 
## 177      53                        Ukraine          176,309 
## 178      78                        Uruguay           49,920 
## 179       1                  United States       16,244,600 
## 180      75                     Uzbekistan           51,113 
## 181     180 St. Vincent and the Grenadines              713 
## 182      29                  Venezuela, RB          381,286 
## 183      57                        Vietnam          155,820 
## 184     177                        Vanuatu              787 
## 185     181                          Samoa              684 
## 186      90                    Yemen, Rep.           35,646 
## 187      28                   South Africa          384,313 
## 188     112               Congo, Dem. Rep.           17,204 
## 189     104                         Zambia           20,678 
## 190     134                       Zimbabwe            9,802
```

```r
#Manipulate a backup copy
SortGdpRank1 <- SortGdpRank
#
SortGdpRank2 <- SortGdpRank1[order(SortGdpRank1$Ranking, decreasing = TRUE), ]
#Make sure structure is ok and show the first 13 on list and that USA is last. 
str(SortGdpRank2)
```

```
## 'data.frame':	190 obs. of  6 variables:
##  $ EdCountryCode    : chr  "TUV" "KIR" "MHL" "PLW" ...
##  $ IncomeGroup      : chr  "Lower middle income" "Lower middle income" "Lower middle income" "Upper middle income" ...
##  $ ShortName        : chr  "Tuvalu" "Kiribati" "Marshall Islands" "Palau" ...
##  $ Ranking          : int  190 189 188 187 186 185 184 183 182 181 ...
##  $ Economy          : chr  "Tuvalu" "Kiribati" "Marshall Islands" "Palau" ...
##  $ MillionsOfDollars: chr  " 40 " " 175 " " 182 " " 228 " ...
```

```r
#Show the first 13 countries in newly ordered data frame.
head(SortGdpRank2, 13)
```

```
##     EdCountryCode         IncomeGroup                      ShortName
## 174           TUV Lower middle income                         Tuvalu
## 92            KIR Lower middle income                       Kiribati
## 113           MHL Lower middle income               Marshall Islands
## 137           PLW Upper middle income                          Palau
## 156           STP Lower middle income          São Tomé and Principe
## 59            FSM Lower middle income                     Micronesia
## 170           TON Lower middle income                          Tonga
## 46            DMA Upper middle income                       Dominica
## 39            COM          Low income                        Comoros
## 185           WSM Lower middle income                          Samoa
## 181           VCT Upper middle income St. Vincent and the Grenadines
## 69            GRD Upper middle income                        Grenada
## 93            KNA Upper middle income            St. Kitts and Nevis
##     Ranking                        Economy MillionsOfDollars
## 174     190                         Tuvalu               40 
## 92      189                       Kiribati              175 
## 113     188               Marshall Islands              182 
## 137     187                          Palau              228 
## 156     186          São Tomé and Principe              263 
## 59      185          Micronesia, Fed. Sts.              326 
## 170     184                          Tonga              472 
## 46      183                       Dominica              480 
## 39      182                        Comoros              596 
## 185     181                          Samoa              684 
## 181     180 St. Vincent and the Grenadines              713 
## 69      178                        Grenada              767 
## 93      178            St. Kitts and Nevis              767
```

```r
#Show that USA is last with rank #1.
tail(SortGdpRank2, 1)
```

```
##     EdCountryCode       IncomeGroup     ShortName Ranking       Economy
## 179           USA High income: OECD United States       1 United States
##     MillionsOfDollars
## 179       16,244,600
```
##### There are 190 countries from the GDP table that have a valid ranking.  When sorted from lowest ranking to the highest ranking (USA), the 13th country in the sorted data frame is St. Kitts and Nevis.  St. Kitts and Nevis has a tied Ranking to Grenada.
########  Please Note there is no information for SSD South Sudan (155 on list) in the edstat data but there is data in the GDP data. South Sudan was formed July 9, 2011. The data on this country that is not available is the Income Group Data.  According to http://www.worldbank.org/en/country/southsudan/overview#1, "The country’s growth domestic product (GDP) per capita in 2014 was $1,111. Outside the oil sector, livelihoods are concentrated in low productive, unpaid agriculture and pastoralists work, accounting for around 15% of GDP. In fact, 85% of the working population is engaged in non-wage work, chiefly in agriculture (78%)."

 
#####Question 3	What are the average GDP rankings for the "High income: OECD" and "High income: nonOECD" groups? 

```r
#Work with backup copy and use data frame without South Sudan
SortGdpRank3 <- SortGdpRank189
#Collect the rows into a new data frame that have "High income: OECD" in the IncomeGroup column
HighIncOECD <- SortGdpRank3[SortGdpRank3$IncomeGroup == "High income: OECD", c(1,2,3,4,5,6) ]
#Collect the rows into a new data frame that have "High income: nonOECD" in the IncomeGroup column
HighIncNonOECD <- SortGdpRank3[SortGdpRank3$IncomeGroup == "High income: nonOECD", c(1,2,3,4,5,6)]
#Make sure structure is ok and find out how many are in each group.
str(HighIncOECD)
```

```
## 'data.frame':	30 obs. of  6 variables:
##  $ EdCountryCode    : chr  "AUS" "AUT" "BEL" "CAN" ...
##  $ IncomeGroup      : chr  "High income: OECD" "High income: OECD" "High income: OECD" "High income: OECD" ...
##  $ ShortName        : chr  "Australia" "Austria" "Belgium" "Canada" ...
##  $ Ranking          : int  12 27 25 11 20 51 4 33 13 43 ...
##  $ Economy          : chr  "Australia" "Austria" "Belgium" "Canada" ...
##  $ MillionsOfDollars: chr  " 1,532,408 " " 394,708 " " 483,262 " " 1,821,424 " ...
```

```r
HighIncOECD
```

```
##     EdCountryCode       IncomeGroup       ShortName Ranking
## 9             AUS High income: OECD       Australia      12
## 10            AUT High income: OECD         Austria      27
## 13            BEL High income: OECD         Belgium      25
## 31            CAN High income: OECD          Canada      11
## 32            CHE High income: OECD     Switzerland      20
## 44            CZE High income: OECD  Czech Republic      51
## 45            DEU High income: OECD         Germany       4
## 47            DNK High income: OECD         Denmark      33
## 53            ESP High income: OECD           Spain      13
## 56            FIN High income: OECD         Finland      43
## 58            FRA High income: OECD          France       5
## 61            GBR High income: OECD  United Kingdom       6
## 68            GRC High income: OECD          Greece      42
## 76            HUN High income: OECD         Hungary      58
## 79            IRL High income: OECD         Ireland      46
## 82            ISL High income: OECD         Iceland     122
## 83            ISR High income: OECD          Israel      40
## 84            ITA High income: OECD           Italy       9
## 87            JPN High income: OECD           Japan       3
## 94            KOR High income: OECD           Korea      15
## 104           LUX High income: OECD      Luxembourg      74
## 128           NLD High income: OECD     Netherlands      18
## 129           NOR High income: OECD          Norway      23
## 131           NZL High income: OECD     New Zealand      55
## 139           POL High income: OECD          Poland      24
## 141           PRT High income: OECD        Portugal      45
## 157           SVK High income: OECD Slovak Republic      63
## 158           SVN High income: OECD        Slovenia      80
## 159           SWE High income: OECD          Sweden      21
## 178           USA High income: OECD   United States       1
##             Economy MillionsOfDollars
## 9         Australia        1,532,408 
## 10          Austria          394,708 
## 13          Belgium          483,262 
## 31           Canada        1,821,424 
## 32      Switzerland          631,173 
## 44   Czech Republic          196,446 
## 45          Germany        3,428,131 
## 47          Denmark          314,887 
## 53            Spain        1,322,965 
## 56          Finland          247,546 
## 58           France        2,612,878 
## 61   United Kingdom        2,471,784 
## 68           Greece          249,099 
## 76          Hungary          124,600 
## 79          Ireland          210,771 
## 82          Iceland           13,579 
## 83           Israel          258,217 
## 84            Italy        2,014,670 
## 87            Japan        5,959,718 
## 94      Korea, Rep.        1,129,598 
## 104      Luxembourg           55,178 
## 128     Netherlands          770,555 
## 129          Norway          499,667 
## 131     New Zealand          167,347 
## 139          Poland          489,795 
## 141        Portugal          212,274 
## 157 Slovak Republic           91,149 
## 158        Slovenia           45,279 
## 159          Sweden          523,806 
## 178   United States       16,244,600
```

```r
str(HighIncNonOECD)
```

```
## 'data.frame':	23 obs. of  6 variables:
##  $ EdCountryCode    : chr  "ABW" "ARE" "BHR" "BHS" ...
##  $ IncomeGroup      : chr  "High income: nonOECD" "High income: nonOECD" "High income: nonOECD" "High income: nonOECD" ...
##  $ ShortName        : chr  "Aruba" "United Arab Emirates" "Bahrain" "The Bahamas" ...
##  $ Ranking          : int  161 32 93 138 149 153 113 102 103 110 ...
##  $ Economy          : chr  "Aruba" "United Arab Emirates" "Bahrain" "Bahamas, The" ...
##  $ MillionsOfDollars: chr  " 2,584 " " 348,595 " " 29,044 " " 8,149 " ...
```

```r
HighIncNonOECD
```

```
##     EdCountryCode          IncomeGroup            ShortName Ranking
## 1             ABW High income: nonOECD                Aruba     161
## 5             ARE High income: nonOECD United Arab Emirates      32
## 18            BHR High income: nonOECD              Bahrain      93
## 19            BHS High income: nonOECD          The Bahamas     138
## 23            BMU High income: nonOECD              Bermuda     149
## 26            BRB High income: nonOECD             Barbados     153
## 27            BRN High income: nonOECD               Brunei     113
## 43            CYP High income: nonOECD               Cyprus     102
## 54            EST High income: nonOECD              Estonia     103
## 67            GNQ High income: nonOECD    Equatorial Guinea     110
## 72            HKG High income: nonOECD Hong Kong SAR, China      37
## 74            HRV High income: nonOECD              Croatia      71
## 96            KWT High income: nonOECD               Kuwait      56
## 105           LVA High income: nonOECD               Latvia      94
## 106           MAC High income: nonOECD     Macao SAR, China      82
## 108           MCO High income: nonOECD               Monaco     147
## 116           MLT High income: nonOECD                Malta     137
## 132           OMN High income: nonOECD                 Oman      66
## 140           PRI High income: nonOECD          Puerto Rico      61
## 143           QAT High income: nonOECD                Qatar      54
## 147           SAU High income: nonOECD         Saudi Arabia      19
## 150           SGP High income: nonOECD            Singapore      35
## 170           TTO High income: nonOECD  Trinidad and Tobago     101
##                  Economy MillionsOfDollars
## 1                  Aruba            2,584 
## 5   United Arab Emirates          348,595 
## 18               Bahrain           29,044 
## 19          Bahamas, The            8,149 
## 23               Bermuda            5,474 
## 26              Barbados            4,225 
## 27     Brunei Darussalam           16,954 
## 43                Cyprus           22,767 
## 54               Estonia           22,390 
## 67     Equatorial Guinea           17,697 
## 72  Hong Kong SAR, China          263,259 
## 74               Croatia           59,228 
## 96                Kuwait          160,913 
## 105               Latvia           28,373 
## 106     Macao SAR, China           43,582 
## 108               Monaco            6,075 
## 116                Malta            8,722 
## 132                 Oman           69,972 
## 140          Puerto Rico          101,496 
## 143                Qatar          171,476 
## 147         Saudi Arabia          711,050 
## 150            Singapore          274,701 
## 170  Trinidad and Tobago           23,320
```

##### The average GDP rankings for the "High income: OECD".  

######## Data processed without South Sudan information.


```r
AvGDPRankOECD <- mean(HighIncOECD$Ranking)
AvGDPRankOECD
```

```
## [1] 32.96667
```
##### The average GDP rankings for the "High income: nonOECD"

```r
AvGDPRankNonOECD <- mean(HighIncNonOECD$Ranking)
AvGDPRankNonOECD
```

```
## [1] 91.91304
```
######## Data processed without South Sudan information.

#####Question 4	Plot the GDP for all of the countries. Use ggplot2 to color your plot by Income Group.

#####   GDP FOR ALL COUNTRIES

```r
#### grouped box plots
#####Take out the commas in the MillionsOfDollars
SortGdpRank3 <- SortGdpRank189
SortGdpRank3$MillionsOfDollars <- gsub(",", "", SortGdpRank3$MillionsOfDollars)
SortGdpRank3$MillionsOfDollars <- gsub(",", "", SortGdpRank3$MillionsOfDollars)

#Force "MillionsOfDollars" to be an integer
SortGdpRank3$MillionsOfDollars <- as.integer(SortGdpRank3$MillionsOfDollars)
str(SortGdpRank3)
```

```
## 'data.frame':	189 obs. of  6 variables:
##  $ EdCountryCode    : chr  "ABW" "AFG" "AGO" "ALB" ...
##  $ IncomeGroup      : chr  "High income: nonOECD" "Low income" "Lower middle income" "Upper middle income" ...
##  $ ShortName        : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ Ranking          : int  161 105 60 125 32 26 133 172 12 27 ...
##  $ Economy          : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ MillionsOfDollars: int  2584 20497 114147 12648 348595 475502 9951 1134 1532408 394708 ...
```

```r
tail(SortGdpRank3, 20)
```

```
##     EdCountryCode          IncomeGroup                      ShortName
## 170           TTO High income: nonOECD            Trinidad and Tobago
## 171           TUN  Lower middle income                        Tunisia
## 172           TUR  Upper middle income                         Turkey
## 173           TUV  Lower middle income                         Tuvalu
## 174           TZA           Low income                       Tanzania
## 175           UGA           Low income                         Uganda
## 176           UKR  Lower middle income                        Ukraine
## 177           URY  Upper middle income                        Uruguay
## 178           USA    High income: OECD                  United States
## 179           UZB  Lower middle income                     Uzbekistan
## 180           VCT  Upper middle income St. Vincent and the Grenadines
## 181           VEN  Upper middle income                      Venezuela
## 182           VNM  Lower middle income                        Vietnam
## 183           VUT  Lower middle income                        Vanuatu
## 184           WSM  Lower middle income                          Samoa
## 185           YEM  Lower middle income                          Yemen
## 186           ZAF  Upper middle income                   South Africa
## 187           ZAR           Low income                Dem. Rep. Congo
## 188           ZMB           Low income                         Zambia
## 189           ZWE           Low income                       Zimbabwe
##     Ranking                        Economy MillionsOfDollars
## 170     101            Trinidad and Tobago             23320
## 171      79                        Tunisia             45662
## 172      17                         Turkey            789257
## 173     190                         Tuvalu                40
## 174      95                       Tanzania             28242
## 175     106                         Uganda             19881
## 176      53                        Ukraine            176309
## 177      78                        Uruguay             49920
## 178       1                  United States          16244600
## 179      75                     Uzbekistan             51113
## 180     180 St. Vincent and the Grenadines               713
## 181      29                  Venezuela, RB            381286
## 182      57                        Vietnam            155820
## 183     177                        Vanuatu               787
## 184     181                          Samoa               684
## 185      90                    Yemen, Rep.             35646
## 186      28                   South Africa            384313
## 187     112               Congo, Dem. Rep.             17204
## 188     104                         Zambia             20678
## 189     134                       Zimbabwe              9802
```

```r
SortGdpRank4 <- SortGdpRank3
SortGdpRank4$MillionsOfDollars <- (SortGdpRank4$MillionsOfDollars/1000000)
str(SortGdpRank4)
```

```
## 'data.frame':	189 obs. of  6 variables:
##  $ EdCountryCode    : chr  "ABW" "AFG" "AGO" "ALB" ...
##  $ IncomeGroup      : chr  "High income: nonOECD" "Low income" "Lower middle income" "Upper middle income" ...
##  $ ShortName        : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ Ranking          : int  161 105 60 125 32 26 133 172 12 27 ...
##  $ Economy          : chr  "Aruba" "Afghanistan" "Angola" "Albania" ...
##  $ MillionsOfDollars: num  0.00258 0.0205 0.11415 0.01265 0.34859 ...
```

```r
tail(SortGdpRank4, 20)
```

```
##     EdCountryCode          IncomeGroup                      ShortName
## 170           TTO High income: nonOECD            Trinidad and Tobago
## 171           TUN  Lower middle income                        Tunisia
## 172           TUR  Upper middle income                         Turkey
## 173           TUV  Lower middle income                         Tuvalu
## 174           TZA           Low income                       Tanzania
## 175           UGA           Low income                         Uganda
## 176           UKR  Lower middle income                        Ukraine
## 177           URY  Upper middle income                        Uruguay
## 178           USA    High income: OECD                  United States
## 179           UZB  Lower middle income                     Uzbekistan
## 180           VCT  Upper middle income St. Vincent and the Grenadines
## 181           VEN  Upper middle income                      Venezuela
## 182           VNM  Lower middle income                        Vietnam
## 183           VUT  Lower middle income                        Vanuatu
## 184           WSM  Lower middle income                          Samoa
## 185           YEM  Lower middle income                          Yemen
## 186           ZAF  Upper middle income                   South Africa
## 187           ZAR           Low income                Dem. Rep. Congo
## 188           ZMB           Low income                         Zambia
## 189           ZWE           Low income                       Zimbabwe
##     Ranking                        Economy MillionsOfDollars
## 170     101            Trinidad and Tobago          0.023320
## 171      79                        Tunisia          0.045662
## 172      17                         Turkey          0.789257
## 173     190                         Tuvalu          0.000040
## 174      95                       Tanzania          0.028242
## 175     106                         Uganda          0.019881
## 176      53                        Ukraine          0.176309
## 177      78                        Uruguay          0.049920
## 178       1                  United States         16.244600
## 179      75                     Uzbekistan          0.051113
## 180     180 St. Vincent and the Grenadines          0.000713
## 181      29                  Venezuela, RB          0.381286
## 182      57                        Vietnam          0.155820
## 183     177                        Vanuatu          0.000787
## 184     181                          Samoa          0.000684
## 185      90                    Yemen, Rep.          0.035646
## 186      28                   South Africa          0.384313
## 187     112               Congo, Dem. Rep.          0.017204
## 188     104                         Zambia          0.020678
## 189     134                       Zimbabwe          0.009802
```

#####   GDP FOR ALL COUNTRIES - Plotted in Color with Y scale in millions

```r
ggplot(SortGdpRank3,  aes(SortGdpRank3$IncomeGroup, SortGdpRank3$MillionsOfDollars, fill=SortGdpRank3$IncomeGroup )) +  geom_point() + geom_boxplot()
```

![](CaseStudyUnit6GDPVsEDSTAT1_files/figure-html/PlotIncGroup2-1.png)<!-- -->

#####   GDP FOR ALL COUNTRIES  - Plotted in Grey with Y scale divided by millions

```r
ggplot(SortGdpRank4,  aes(SortGdpRank4$IncomeGroup, SortGdpRank4$MillionsOfDollars )) +  geom_point() + geom_boxplot()
```

![](CaseStudyUnit6GDPVsEDSTAT1_files/figure-html/PlotIncGroup3-1.png)<!-- -->














#####The USA is such an outlier at 16,244,600 million that the scale is hard to adjust to show the other boxplots' color or grey.
######## Data processed without South Sudan information.

#####Question 5	Cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group. How many countries are Lower middle income but among the 38 nations with highest GDP?


```r
########5	Cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group. How many countries are Lower middle income but among the 38 nations with highest GDP?


QuantGdpRank3 <- SortGdpRank2
QuantGdpRank3$quantile <- ntile(QuantGdpRank3$Ranking, 5)
str(QuantGdpRank3)
```

```
## 'data.frame':	190 obs. of  7 variables:
##  $ EdCountryCode    : chr  "TUV" "KIR" "MHL" "PLW" ...
##  $ IncomeGroup      : chr  "Lower middle income" "Lower middle income" "Lower middle income" "Upper middle income" ...
##  $ ShortName        : chr  "Tuvalu" "Kiribati" "Marshall Islands" "Palau" ...
##  $ Ranking          : int  190 189 188 187 186 185 184 183 182 181 ...
##  $ Economy          : chr  "Tuvalu" "Kiribati" "Marshall Islands" "Palau" ...
##  $ MillionsOfDollars: chr  " 40 " " 175 " " 182 " " 228 " ...
##  $ quantile         : num  5 5 5 5 5 5 5 5 5 5 ...
```

```r
QuantGdpRank3
```

```
##     EdCountryCode          IncomeGroup                      ShortName
## 174           TUV  Lower middle income                         Tuvalu
## 92            KIR  Lower middle income                       Kiribati
## 113           MHL  Lower middle income               Marshall Islands
## 137           PLW  Upper middle income                          Palau
## 156           STP  Lower middle income          São Tomé and Principe
## 59            FSM  Lower middle income                     Micronesia
## 170           TON  Lower middle income                          Tonga
## 46            DMA  Upper middle income                       Dominica
## 39            COM           Low income                        Comoros
## 185           WSM  Lower middle income                          Samoa
## 181           VCT  Upper middle income St. Vincent and the Grenadines
## 69            GRD  Upper middle income                        Grenada
## 93            KNA  Upper middle income            St. Kitts and Nevis
## 184           VUT  Lower middle income                        Vanuatu
## 66            GNB           Low income                  Guinea-Bissau
## 65            GMB           Low income                     The Gambia
## 151           SLB           Low income                Solomon Islands
## 162           SYC  Upper middle income                     Seychelles
## 8             ATG  Upper middle income            Antigua and Barbuda
## 100           LCA  Upper middle income                      St. Lucia
## 169           TMP  Lower middle income                    Timor-Leste
## 22            BLZ  Lower middle income                         Belize
## 99            LBR           Low income                        Liberia
## 28            BTN  Lower middle income                         Bhutan
## 40            CPV  Lower middle income                     Cape Verde
## 30            CAF           Low income       Central African Republic
## 111           MDV  Lower middle income                       Maldives
## 102           LSO  Lower middle income                        Lesotho
## 12            BDI           Low income                        Burundi
## 1             ABW High income: nonOECD                          Aruba
## 71            GUY  Lower middle income                         Guyana
## 52            ERI           Low income                        Eritrea
## 161           SWZ  Lower middle income                      Swaziland
## 152           SLE           Low income                   Sierra Leone
## 165           TGO           Low income                           Togo
## 57            FJI  Upper middle income                           Fiji
## 120           MRT           Low income                     Mauritania
## 26            BRB High income: nonOECD                       Barbados
## 122           MWI           Low income                         Malawi
## 117           MNE  Upper middle income                     Montenegro
## 157           SUR  Upper middle income                       Suriname
## 23            BMU High income: nonOECD                        Bermuda
## 64            GIN           Low income                         Guinea
## 108           MCO High income: nonOECD                         Monaco
## 95            KSV  Lower middle income                         Kosovo
## 90            KGZ           Low income                Kyrgyz Republic
## 125           NER           Low income                          Niger
## 167           TJK           Low income                     Tajikistan
## 146           RWA           Low income                         Rwanda
## 109           MDA  Lower middle income                        Moldova
## 14            BEN           Low income                          Benin
## 75            HTI           Low income                          Haiti
## 19            BHS High income: nonOECD                    The Bahamas
## 116           MLT High income: nonOECD                          Malta
## 97            LAO           Low income                        Lao PDR
## 114           MKD  Upper middle income                      Macedonia
## 190           ZWE           Low income                       Zimbabwe
## 7             ARM  Lower middle income                        Armenia
## 110           MDG           Low income                     Madagascar
## 155           SSD                 <NA>                           <NA>
## 118           MNG  Lower middle income                       Mongolia
## 115           MLI           Low income                           Mali
## 15            BFA           Low income                   Burkina Faso
## 121           MUS  Upper middle income                      Mauritius
## 127           NIC  Lower middle income                      Nicaragua
## 4             ALB  Upper middle income                        Albania
## 164           TCD           Low income                           Chad
## 124           NAM  Upper middle income                        Namibia
## 82            ISL    High income: OECD                        Iceland
## 37            COG  Lower middle income                          Congo
## 91            KHM           Low income                       Cambodia
## 149           SEN  Lower middle income                        Senegal
## 119           MOZ           Low income                     Mozambique
## 29            BWA  Upper middle income                       Botswana
## 85            JAM  Upper middle income                        Jamaica
## 138           PNG  Lower middle income               Papua New Guinea
## 62            GEO  Lower middle income                        Georgia
## 27            BRN High income: nonOECD                         Brunei
## 188           ZAR           Low income                Dem. Rep. Congo
## 20            BIH  Upper middle income         Bosnia and Herzegovina
## 67            GNQ High income: nonOECD              Equatorial Guinea
## 60            GAB  Upper middle income                          Gabon
## 73            HND  Lower middle income                       Honduras
## 130           NPL           Low income                          Nepal
## 176           UGA           Low income                         Uganda
## 2             AFG           Low income                    Afghanistan
## 189           ZMB           Low income                         Zambia
## 54            EST High income: nonOECD                        Estonia
## 43            CYP High income: nonOECD                         Cyprus
## 171           TTO High income: nonOECD            Trinidad and Tobago
## 153           SLV  Lower middle income                    El Salvador
## 35            CIV  Lower middle income                  Côte d'Ivoire
## 36            CMR  Lower middle income                       Cameroon
## 142           PRY  Lower middle income                       Paraguay
## 24            BOL  Lower middle income                        Bolivia
## 175           TZA           Low income                       Tanzania
## 105           LVA High income: nonOECD                         Latvia
## 18            BHR High income: nonOECD                        Bahrain
## 86            JOR  Lower middle income                         Jordan
## 168           TKM  Lower middle income                   Turkmenistan
## 186           YEM  Lower middle income                          Yemen
## 134           PAN  Upper middle income                         Panama
## 154           SRB  Upper middle income                         Serbia
## 89            KEN           Low income                          Kenya
## 63            GHA           Low income                          Ghana
## 55            ETH           Low income                       Ethiopia
## 103           LTU  Upper middle income                      Lithuania
## 98            LBN  Upper middle income                        Lebanon
## 106           MAC High income: nonOECD               Macao SAR, China
## 41            CRI  Upper middle income                     Costa Rica
## 159           SVN    High income: OECD                       Slovenia
## 172           TUN  Lower middle income                        Tunisia
## 178           URY  Upper middle income                        Uruguay
## 70            GTM  Lower middle income                      Guatemala
## 17            BGR  Upper middle income                       Bulgaria
## 180           UZB  Lower middle income                     Uzbekistan
## 104           LUX    High income: OECD                     Luxembourg
## 148           SDN  Lower middle income                          Sudan
## 48            DOM  Upper middle income             Dominican Republic
## 74            HRV High income: nonOECD                        Croatia
## 101           LKA  Lower middle income                      Sri Lanka
## 21            BLR  Upper middle income                        Belarus
## 11            AZE  Upper middle income                     Azerbaijan
## 42            CUB  Upper middle income                           Cuba
## 132           OMN High income: nonOECD                           Oman
## 163           SYR  Lower middle income           Syrian Arab Republic
## 50            ECU  Lower middle income                        Ecuador
## 158           SVK    High income: OECD                Slovak Republic
## 107           MAR  Lower middle income                        Morocco
## 140           PRI High income: nonOECD                    Puerto Rico
## 3             AGO  Lower middle income                         Angola
## 16            BGD           Low income                     Bangladesh
## 76            HUN    High income: OECD                        Hungary
## 183           VNM  Lower middle income                        Vietnam
## 96            KWT High income: nonOECD                         Kuwait
## 131           NZL    High income: OECD                    New Zealand
## 143           QAT High income: nonOECD                          Qatar
## 177           UKR  Lower middle income                        Ukraine
## 144           ROM  Upper middle income                        Romania
## 44            CZE    High income: OECD                 Czech Republic
## 88            KAZ  Upper middle income                     Kazakhstan
## 135           PER  Upper middle income                           Peru
## 49            DZA  Upper middle income                        Algeria
## 81            IRQ  Lower middle income                           Iraq
## 79            IRL    High income: OECD                        Ireland
## 141           PRT    High income: OECD                       Portugal
## 133           PAK  Lower middle income                       Pakistan
## 56            FIN    High income: OECD                        Finland
## 68            GRC    High income: OECD                         Greece
## 136           PHL  Lower middle income                    Philippines
## 83            ISR    High income: OECD                         Israel
## 126           NGA  Lower middle income                        Nigeria
## 51            EGY  Lower middle income                          Egypt
## 72            HKG High income: nonOECD           Hong Kong SAR, China
## 33            CHL  Upper middle income                          Chile
## 150           SGP High income: nonOECD                      Singapore
## 123           MYS  Upper middle income                       Malaysia
## 47            DNK    High income: OECD                        Denmark
## 5             ARE High income: nonOECD           United Arab Emirates
## 166           THA  Lower middle income                       Thailand
## 38            COL  Upper middle income                       Colombia
## 182           VEN  Upper middle income                      Venezuela
## 187           ZAF  Upper middle income                   South Africa
## 10            AUT    High income: OECD                        Austria
## 6             ARG  Upper middle income                      Argentina
## 13            BEL    High income: OECD                        Belgium
## 139           POL    High income: OECD                         Poland
## 129           NOR    High income: OECD                         Norway
## 80            IRN  Upper middle income                           Iran
## 160           SWE    High income: OECD                         Sweden
## 32            CHE    High income: OECD                    Switzerland
## 147           SAU High income: nonOECD                   Saudi Arabia
## 128           NLD    High income: OECD                    Netherlands
## 173           TUR  Upper middle income                         Turkey
## 77            IDN  Lower middle income                      Indonesia
## 94            KOR    High income: OECD                          Korea
## 112           MEX  Upper middle income                         Mexico
## 53            ESP    High income: OECD                          Spain
## 9             AUS    High income: OECD                      Australia
## 31            CAN    High income: OECD                         Canada
## 78            IND  Lower middle income                          India
## 84            ITA    High income: OECD                          Italy
## 145           RUS  Upper middle income                         Russia
## 25            BRA  Upper middle income                         Brazil
## 61            GBR    High income: OECD                 United Kingdom
## 58            FRA    High income: OECD                         France
## 45            DEU    High income: OECD                        Germany
## 87            JPN    High income: OECD                          Japan
## 34            CHN  Lower middle income                          China
## 179           USA    High income: OECD                  United States
##     Ranking                        Economy MillionsOfDollars quantile
## 174     190                         Tuvalu               40         5
## 92      189                       Kiribati              175         5
## 113     188               Marshall Islands              182         5
## 137     187                          Palau              228         5
## 156     186          São Tomé and Principe              263         5
## 59      185          Micronesia, Fed. Sts.              326         5
## 170     184                          Tonga              472         5
## 46      183                       Dominica              480         5
## 39      182                        Comoros              596         5
## 185     181                          Samoa              684         5
## 181     180 St. Vincent and the Grenadines              713         5
## 69      178                        Grenada              767         5
## 93      178            St. Kitts and Nevis              767         5
## 184     177                        Vanuatu              787         5
## 66      176                  Guinea-Bissau              822         5
## 65      175                    Gambia, The              917         5
## 151     174                Solomon Islands            1,008         5
## 162     173                     Seychelles            1,129         5
## 8       172            Antigua and Barbuda            1,134         5
## 100     171                      St. Lucia            1,239         5
## 169     170                    Timor-Leste            1,293         5
## 22      169                         Belize            1,493         5
## 99      168                        Liberia            1,734         5
## 28      167                         Bhutan            1,780         5
## 40      166                     Cape Verde            1,827         5
## 30      165       Central African Republic            2,184         5
## 111     164                       Maldives            2,222         5
## 102     163                        Lesotho            2,448         5
## 12      162                        Burundi            2,472         5
## 1       161                          Aruba            2,584         5
## 71      160                         Guyana            2,851         5
## 52      159                        Eritrea            3,092         5
## 161     158                      Swaziland            3,744         5
## 152     157                   Sierra Leone            3,796         5
## 165     156                           Togo            3,814         5
## 57      155                           Fiji            3,908         5
## 120     154                     Mauritania            4,199         5
## 26      153                       Barbados            4,225         5
## 122     152                         Malawi            4,264         4
## 117     151                     Montenegro            4,373         4
## 157     150                       Suriname            5,012         4
## 23      149                        Bermuda            5,474         4
## 64      148                         Guinea            5,632         4
## 108     147                         Monaco            6,075         4
## 95      146                         Kosovo            6,445         4
## 90      145                Kyrgyz Republic            6,475         4
## 125     144                          Niger            6,773         4
## 167     143                     Tajikistan            6,972         4
## 146     142                         Rwanda            7,103         4
## 109     141                        Moldova            7,253         4
## 14      140                          Benin            7,557         4
## 75      139                          Haiti            7,843         4
## 19      138                   Bahamas, The            8,149         4
## 116     137                          Malta            8,722         4
## 97      136                        Lao PDR            9,418         4
## 114     135                 Macedonia, FYR            9,613         4
## 190     134                       Zimbabwe            9,802         4
## 7       133                        Armenia            9,951         4
## 110     132                     Madagascar            9,975         4
## 155     131                    South Sudan           10,220         4
## 118     130                       Mongolia           10,271         4
## 115     129                           Mali           10,308         4
## 15      128                   Burkina Faso           10,441         4
## 121     127                      Mauritius           10,486         4
## 127     126                      Nicaragua           10,507         4
## 4       125                        Albania           12,648         4
## 164     124                           Chad           12,887         4
## 124     123                        Namibia           13,072         4
## 82      122                        Iceland           13,579         4
## 37      121                    Congo, Rep.           13,678         4
## 91      120                       Cambodia           14,038         4
## 149     119                        Senegal           14,046         4
## 119     118                     Mozambique           14,244         4
## 29      117                       Botswana           14,504         4
## 85      116                        Jamaica           14,755         4
## 138     115               Papua New Guinea           15,654         4
## 62      114                        Georgia           15,747         3
## 27      113              Brunei Darussalam           16,954         3
## 188     112               Congo, Dem. Rep.           17,204         3
## 20      111         Bosnia and Herzegovina           17,466         3
## 67      110              Equatorial Guinea           17,697         3
## 60      109                          Gabon           18,377         3
## 73      108                       Honduras           18,434         3
## 130     107                          Nepal           18,963         3
## 176     106                         Uganda           19,881         3
## 2       105                    Afghanistan           20,497         3
## 189     104                         Zambia           20,678         3
## 54      103                        Estonia           22,390         3
## 43      102                         Cyprus           22,767         3
## 171     101            Trinidad and Tobago           23,320         3
## 153     100                    El Salvador           23,864         3
## 35       99                  Côte d'Ivoire           24,680         3
## 36       98                       Cameroon           25,322         3
## 142      97                       Paraguay           25,502         3
## 24       96                        Bolivia           27,035         3
## 175      95                       Tanzania           28,242         3
## 105      94                         Latvia           28,373         3
## 18       93                        Bahrain           29,044         3
## 86       92                         Jordan           31,015         3
## 168      91                   Turkmenistan           35,164         3
## 186      90                    Yemen, Rep.           35,646         3
## 134      89                         Panama           36,253         3
## 154      88                         Serbia           37,489         3
## 89       87                          Kenya           40,697         3
## 63       86                          Ghana           40,711         3
## 55       85                       Ethiopia           41,605         3
## 103      84                      Lithuania           42,344         3
## 98       83                        Lebanon           42,945         3
## 106      82               Macao SAR, China           43,582         3
## 41       81                     Costa Rica           45,104         3
## 159      80                       Slovenia           45,279         3
## 172      79                        Tunisia           45,662         3
## 178      78                        Uruguay           49,920         3
## 70       77                      Guatemala           50,234         3
## 17       76                       Bulgaria           50,972         2
## 180      75                     Uzbekistan           51,113         2
## 104      74                     Luxembourg           55,178         2
## 148      73                          Sudan           58,769         2
## 48       72             Dominican Republic           59,047         2
## 74       71                        Croatia           59,228         2
## 101      70                      Sri Lanka           59,423         2
## 21       69                        Belarus           63,267         2
## 11       68                     Azerbaijan           66,605         2
## 42       67                           Cuba           68,234         2
## 132      66                           Oman           69,972         2
## 163      65           Syrian Arab Republic           73,672         2
## 50       64                        Ecuador           84,040         2
## 158      63                Slovak Republic           91,149         2
## 107      62                        Morocco           95,982         2
## 140      61                    Puerto Rico          101,496         2
## 3        60                         Angola          114,147         2
## 16       59                     Bangladesh          116,355         2
## 76       58                        Hungary          124,600         2
## 183      57                        Vietnam          155,820         2
## 96       56                         Kuwait          160,913         2
## 131      55                    New Zealand          167,347         2
## 143      54                          Qatar          171,476         2
## 177      53                        Ukraine          176,309         2
## 144      52                        Romania          192,711         2
## 44       51                 Czech Republic          196,446         2
## 88       50                     Kazakhstan          203,521         2
## 135      49                           Peru          203,790         2
## 49       48                        Algeria          205,789         2
## 81       47                           Iraq          210,280         2
## 79       46                        Ireland          210,771         2
## 141      45                       Portugal          212,274         2
## 133      44                       Pakistan          225,143         2
## 56       43                        Finland          247,546         2
## 68       42                         Greece          249,099         2
## 136      41                    Philippines          250,182         2
## 83       40                         Israel          258,217         2
## 126      39                        Nigeria          262,597         2
## 51       38               Egypt, Arab Rep.          262,832         1
## 72       37           Hong Kong SAR, China          263,259         1
## 33       36                          Chile          269,869         1
## 150      35                      Singapore          274,701         1
## 123      34                       Malaysia          305,033         1
## 47       33                        Denmark          314,887         1
## 5        32           United Arab Emirates          348,595         1
## 166      31                       Thailand          365,966         1
## 38       30                       Colombia          369,606         1
## 182      29                  Venezuela, RB          381,286         1
## 187      28                   South Africa          384,313         1
## 10       27                        Austria          394,708         1
## 6        26                      Argentina          475,502         1
## 13       25                        Belgium          483,262         1
## 139      24                         Poland          489,795         1
## 129      23                         Norway          499,667         1
## 80       22             Iran, Islamic Rep.          514,060         1
## 160      21                         Sweden          523,806         1
## 32       20                    Switzerland          631,173         1
## 147      19                   Saudi Arabia          711,050         1
## 128      18                    Netherlands          770,555         1
## 173      17                         Turkey          789,257         1
## 77       16                      Indonesia          878,043         1
## 94       15                    Korea, Rep.        1,129,598         1
## 112      14                         Mexico        1,178,126         1
## 53       13                          Spain        1,322,965         1
## 9        12                      Australia        1,532,408         1
## 31       11                         Canada        1,821,424         1
## 78       10                          India        1,841,710         1
## 84        9                          Italy        2,014,670         1
## 145       8             Russian Federation        2,014,775         1
## 25        7                         Brazil        2,252,664         1
## 61        6                 United Kingdom        2,471,784         1
## 58        5                         France        2,612,878         1
## 45        4                        Germany        3,428,131         1
## 87        3                          Japan        5,959,718         1
## 34        2                          China        8,227,103         1
## 179       1                  United States       16,244,600         1
```

```r
table(QuantGdpRank3$quantile,QuantGdpRank3$IncomeGroup)
```

```
##    
##     High income: nonOECD High income: OECD Low income Lower middle income
##   1                    4                18          0                   5
##   2                    5                10          1                  13
##   3                    8                 1          9                  12
##   4                    4                 1         16                   8
##   5                    2                 0         11                  16
##    
##     Upper middle income
##   1                  11
##   2                   9
##   3                   8
##   4                   8
##   5                   9
```
##### The answer is 5 countries are Lower Middle Income but among the 38 nations with the highest GDP.  The countries are China, India, Indonesia, Thailand, and Egypt.  This data is processed without South Sudan information.

#################################################################################################

##### There were 224 matching country codes.  After processing the matching codes, I then merged only the 190 rows from the GDP data set that had rankings and Millions of Dollars in GDP, so I utilized only the needed information and did not keep working with potentially disruptive data.  So potentially, there were 34 rows that had NA or disruptive data.  Additionally, South Sudan was not in the edstat data, because it is such a new country.  Because there was a lack of data about South Sudan's IncomeGroup, the calculations or graphics that included IncomeGroup were done with 189 countries instead of 190.

#################################################################################################
#################################################################################################
#################################################################################################
#################################################################################################
#################################################################################################
########Document the installed packages and libraries


```r
sessionInfo()
```

```
## R version 3.2.5 (2016-04-14)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 7 x64 (build 7601) Service Pack 1
## 
## locale:
## [1] LC_COLLATE=English_United States.1252 
## [2] LC_CTYPE=English_United States.1252   
## [3] LC_MONETARY=English_United States.1252
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] dplyr_0.4.3    ggplot2_2.1.0  downloader_0.4
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.4      assertthat_0.1   digest_0.6.9     R6_2.1.2        
##  [5] plyr_1.8.4       grid_3.2.5       DBI_0.4          gtable_0.2.0    
##  [9] formatR_1.3      magrittr_1.5     evaluate_0.9     scales_0.4.0    
## [13] stringi_1.0-1    rmarkdown_0.9.6  labeling_0.3     tools_3.2.5     
## [17] stringr_1.0.0    munsell_0.4.3    parallel_3.2.5   yaml_2.1.13     
## [21] colorspace_1.2-6 htmltools_0.3.5  knitr_1.13
```





