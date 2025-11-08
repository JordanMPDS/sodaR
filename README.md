# Package currently under development to be re-listed on CRAN

sodaR
========

[![downloads](https://cranlogs.r-pkg.org/badges/RSocrata)](https://CRAN.R-project.org/package=RSocrata)
[![cran version](https://www.r-pkg.org/badges/version/RSocrata)](https://CRAN.R-project.org/package=RSocrata)

A tool for downloading and uploading Socrata datasets
-----------------------------------------------------

Provided with a URL to a dataset resource published on a [Socrata](https://www.tylertech.com/products/data-insights) webserver,
or a Socrata [SoDA (Socrata Open Data Application Program Interface) web API](https://dev.socrata.com) query,
or a Socrata "human-friendly" URL, ```read.socrata()```
returns an [R data frame](https://stat.ethz.ch/R-manual/R-devel/library/base/html/data.frame.html).
Converts dates to [POSIX](https://stat.ethz.ch/R-manual/R-devel/library/base/html/DateTimeClasses.html) format.
Supports CSV and JSON download file formats from Socrata.
Manages the throttling of data returned from Socrata and allows users to provide an [application token](https://dev.socrata.com/docs/app-tokens.html).
Supports [SoDA query parameters](https://dev.socrata.com/docs/queries.html) in the URL string for further filtering, sorting, and queries.
Upload data to Socrata data portals using "upsert" and "replace" methods.

Use ```ls.socrata()``` to list all datasets available on a Socrata webserver.

## Installation


To get the current released version from CRAN download with your favorite package manager:

Base
```R
install.packages("sodaR")
```
pak
```R
pak::pak("sodaR")
```


Examples
--------

### Reading SoDA valid URLs
```r
earthquakesDataFrame <- read.socrata("https://soda.demo.socrata.com/resource/4334-bgaj.csv")
nrow(earthquakesDataFrame) # 1007 (two "pages")
class(earthquakesDataFrame$Datetime[1]) # POSIXlt
```

### Reading "human-readable" URLs
```r
earthquakesDataFrame <- read.socrata("https://soda.demo.socrata.com/dataset/USGS-Earthquakes-for-2012-11-01-API-School-Demo/4334-bgaj")
nrow(earthquakesDataFrame) # 1007 (two "pages")
class(earthquakesDataFrame$Datetime[1]) # POSIXlt
```

### Using API key to read datasets
```r
token <- "ew2rEMuESuzWPqMkyPfOSGJgE"
earthquakesDataFrame <- read.socrata("https://soda.demo.socrata.com/resource/4334-bgaj.csv", app_token = token)
nrow(earthquakesDataFrame)
```

### Download private datasets from portal
```r
# Store user email and password
socrataEmail <- Sys.getenv("SOCRATA_EMAIL", "mark.silverberg+soda.demo@socrata.com")
socrataPassword <- Sys.getenv("SOCRATA_PASSWORD", "7vFDsGFDUG")

privateResourceToReadCsvUrl <- "https://soda.demo.socrata.com/resource/a9g2-feh2.csv" # dataset

read.socrata(url = privateResourceToReadCsvUrl, email = socrataEmail, password = socrataPassword)
```

### List all datasets on portal
```r
allSitesDataFrame <- ls.socrata("https://soda.demo.socrata.com")
nrow(allSitesDataFrame) # Number of datasets
allSitesDataFrame$title # Names of each dataset
```

### Upload data to portal
```r
# Store user email and password
socrataEmail <- Sys.getenv("SOCRATA_EMAIL", "mark.silverberg+soda.demo@socrata.com")
socrataPassword <- Sys.getenv("SOCRATA_PASSWORD", "7vFDsGFDUG")

datasetToAddToUrl <- "https://soda.demo.socrata.com/resource/xh6g-yugi.json" # dataset
 
# Generate some data
x <- sample(-1000:1000, 1)
y <- sample(-1000:1000, 1)
df_in <- data.frame(x,y)
 
# Upload to Socrata
write.socrata(df_in,datasetToAddToUrl,"UPSERT",socrataEmail,socrataPassword)
```

## Acknowledgments

sodaR is a fork of [RSocrata](https://github.com/Chicago/RSocrata), originally developed by Hugh Devlin, Tom Schenk Jr., and contributors at the City of Chicago. I am grateful for their work.