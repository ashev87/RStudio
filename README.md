# RStudio
general functions for R

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




