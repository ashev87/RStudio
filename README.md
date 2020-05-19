# RStudio
general functions for R

## connecting to mysql database

ucscDB <- dbConnect(MySQL(), user="genome",
                    host="website")
# get a list of databases
result <- dbGetQuery(ucscDB, 'show databases;'); dbDisconnect(ucscDB)
# connecting to specific db
gh19 <- dbConnect(MySQL(), user="genome", db="hg19",
                    host="website")
allTables <- dbListTables(gh19)
lenght(allTables)
# listing the fields in specific table
dbListFields(db, "table")
# quering a specific table
dbGetQuery(db, "select count(*) from table")
# read from table
data <- dbReadTable(db, "table")
# subset
query <- dbSendQuery(db, "select * from table where")
result <- fetch(query)
result <- fetch(query, n=10); dbClearResult(query) # to clear the query from the db
# disconnet from db
dbDisconnect(db)