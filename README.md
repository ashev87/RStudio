# RStudio
general functions for R

## General functions

### installing and loading packages

install.packages("readxl")
library("readxl")

### installing and loading swirl

library(swirl)
install_from_swirl("Getting and Cleaning Data")
swirl()



## MySQL DB - RMySQL library
install.packages("RMySQL")
library(RMySQL)
### connecting to mysql database
ucscDB <- dbConnect(MySQL(), user="genome",
                    host="website")
### get a list of databases
result <- dbGetQuery(ucscDB, 'show databases;'); dbDisconnect(ucscDB)
### connecting to specific db
gh19 <- dbConnect(MySQL(), user="genome", db="hg19",
                    host="website")
allTables <- dbListTables(gh19)
lenght(allTables)
### listing the fields in specific table
dbListFields(db, "table")
### quering a specific table
dbGetQuery(db, "select count(*) from table")
### read from table
data <- dbReadTable(db, "table")
### subset
query <- dbSendQuery(db, "select * from table where")
result <- fetch(query)
#### receive only 10 results of the query
result <- fetch(query, n=10); dbClearResult(query) # to clear the query from the db
### disconnet from db !important!
dbDisconnect(db)

## HDF5 DB - RMySQL library hdf5group.org
source("http://bioconductor.org/biocLite.R")
bioCLite("rhdf5")
library(rhdf5)
created = h5createFile("example.h5")
### create group
created = h5createGroup("example.h5", "foo")
created = h5createGroup("example.h5", "baa")
created = h5createGroup("example.h5", "foo/foobaa")
### see the groups created
h5ls("example.h5")
### write to specific group
A = matrix(1:10, nr=5, nc=2)
h5write(A, "example.h5", "foo/A")
B = array(seq(0.1, 2.0, by=0.1), dim=c(5,2,2))
attr(B, "scale") <- "liter"
h5write(B, "example.h5", "foo/foobaa/B")
### write a  data frame
df = data.frame(1L:5L, seq(0,1,length.out=5), c("ab, "cde", "fghi", "a", "s"), stringAsFactors=FALSE)
h5write(df, "example.h5", "df")
### reading data
readA = h5read("example.h5", "foo/A")
### writing and reading in chunks and to specific place in db
h5wirte(c(12,13,14), "example.h5", "foo/A", index=list(1:3, 1))
h5read("example.h5", "foo/A")

## reading from the web / webscrapping
### getting data off webpages - readLines()
con = url("http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en")
htmlCode = readLines(con)
close(con)
htmlCode
### parsing with XML
library(XML)
url <- "http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en"
html <- htmlTreeParse(url, useInternalNodes=T)
### get title of page
xpathSApply(html, "//title", xmlValue)
### get the number of citations
xpathSApply(html, "//td[@id='col-citeby']", xmlValue)
### same from GET command from httr package
library(httr); html2 = GET(url)
content2 = content(html2, as="text")
parsedHtml = htmlParse(content2, asText=TRUE)
xpathSApple(parsedHmtl, "//title", xmlValue)

### accessing webpages with password
pg2 = GET("http://httpbin.org/basic-auth/user/passwd", authenticate("user", "passwd"))
pg2
### using handles (cookies)
google = handle("http://google.com")
pg1 = GET(handle=google, path="/")
pg2 = GET(handle=google, path="search")

## reading from APIs
### create an application for example on twitter and create credetials
library(httr)
myapp = oauth_app("twitter",
                  key="yourConsumerKey", secret="yourConsumerSecret")
sig = sign_oauth1.0(myapp,
                    token = "yourToker",
                    token_secret = "yourTokenSecret")
homeTL = GET("http://api.twitter.com/1.1/statuses/home_timeline.json", sig)
### converting the json data - rjsonio package
library(rjsonio)
json1 = content(homeTL)
json2 = jsonlite::fromJSON(toJSON(json1))
json2[1,1:4]

## reading from other sources
### useful packages
find packages in google "MySQL R package"

### interacting more directly with files
file - open a connection to a text fiel
url - open a connection to a url
gzfile - open a connection to a .gz file
bzfile - open a connection to a .bz2 file
?connections for more information
! remember to close connections

### reading images
jpeg
readbitmap
png
EBImage
### reading geographic data
rdgal
rgeos
raster
### reading music data
tuneR
seewave

### loading files from the internet

url <- c('https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx')
file <- download.file(url, destfile = "data.xlsx")

### loading file into dataframe

dat <- read_excel("data2.xlsx", range = "G18:O23")

df <- ?read_xlsx(".\\data2.xlsx", rowIndex = 18:23, colIndex = 7:15)
read.xlsx

## common functions
###length
length(data)
###class
class(data)
###subset of column in dataframe
nrow(subset(data, VAL == "24"))
### getwd
getwd()

## dplyr functions
###create object
cran <- tbl_df(mydf)
###select columns
select(cran, columns)
###filter columns
filter(cran, column1 == x, column2 == y)
###sorting the values
arrange(cran2, desc(ip_id)
### adding a column to the data object
mutate(cran3, size_mb = size / 2^20)
### summarizing the columns
summarize(cran, avg_bytes = mean(size)) 

## exploring the dataset
head(db, n = 5)
tail(db, n = 5)
### summarize the db
summary(db)
str(db)
### looking at the quantile distribution of a column
quantile(db$column, na.rm=TRUE)
quantile(db$column, probs = c(0.5, 0.75, 0.9))
### making a table to explore the data
table(db$column, useNA="ifany")
### make a 2-D table by passing 2 columns to see the relationship between 2 variables
table(db$column1, db$column2)
### making cross tabs to compare the data
xt <- xtabs(countedvariable ~ rowsvariable + colvariable, data = DF)
### crosstabs against all the variables
xt = xtabs(countervariable ~ ., data=DF)
ftable(xt) # making a flat table to see all in one table

## checking for missing values
### check if columns contain NA
sum(is.na(db$column))
any(is.na(db§column))
### check if all the values in a column satisfy a condition
all(db$column > 0)
### take sums across rows and columns
colSums(is.na(db))
all(colSums(is.na(db))== 0)

## finding values with specific characteristics
table(db$column %in% c("value"))
table(db$column %in% c("value1", "value2", "value3"))
### subsetting the db with this logic
db[db$column %in% c("value"),]

## Size of data set
object.size(db) # size in bytes
print(object.size(db), units = "Mb")

## Creating new variables

### getting data from the web
if(!file.exists("./data")){dir.create("./data")}
fileUrl <- "url"
download.file(fileUrl, destfile = "./data/filename.csv", method = "curl")
data <- read.csv("./data/filename.csv")

### creating sequences
s1 <- seq(start, stop, by= 2) # create a sequence increasing by 2
s2 <- seq(start, stop, length = 3) # create a sequence of length 3
x <- c(1,3,6,12,3); seq(along = x) # create a sequence of the same size as a vector

### subsetting variables
db$conditioncolumn = db$column %in% c("value1","value2")
table(db$conditioncolumn)

### creating binary variables
db$conditioncolumn = ifelse(db$column < 0, TRUE, FALSE)
table(db$condtioncolumn, db$column < 0)

### creating categorical variables
db$conditioncol = cut(db$column, breaks=quantile(condition$column)) # break up the values accroding to some condition in this case quantile
table(db$conditioncol)
table(db$conditioncol, db$column) # see how the values fall into the created groups. See how a quantitative variable into a categorical variable

#### easier version with a library
library(Hmisc)
db$conditioncol = cut2(db$column, g=4) # break into 4 groups

### creating factor variables
db$newcol = factor(db$conditioncol)

#### levels of factor variables (example)
yesno <- sample(c("yes", "no"), size = 10, replace = TRUE)
yesnofac <- factor(yesno, levels = c("yes", "no"))
releve(yesnofac, ref="yes")
as.numeric(yesnofac)

#### cutting produces factor variables
library(Hmisc)
db$conditioncol = cut2(db$column, g=4)

#### using the mutate function to do it at the same time
library(Hmisc); library(plyr)
db2 = mutate(db, newcol = cut2(conditioncol, g = 4))

### common transforms
abs(x) absolute value
sqrt(x) square root
ceilings(x) ceiling(3.475) is 4
floor(x) floor(3.475) is 3
round(x, digits = n) round(3.475, digits =2) is 3.48
signif(x, digits = n) signif(3.475, digits = 2) is 3.5
cos(x), sin(x) etc
log(x), log2(x), log10(x)
exp(x)























