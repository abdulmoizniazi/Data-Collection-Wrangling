# Data-Collection-Wrangling
Al Nafi Data Collection &amp; Wrangling
print(Heelloo!)
#                    Data Collection and Data Wrangling 
#_______________________________________________________________________________

#                      Working Directory

setwd()
getwd()

file.exists('hello-r')
?file.exists

dir.create("happy")
file.exists("happy")

if(!file.exists("popo")){
  dir.create("popo")
}
setwd("./popo")
dir.create("popo")
file.exists("popo")
getwd()

?str
?fread
install.packages("xlsx")
install.packages("readxl")
install.packages("openxlsx")
?read.excel

install.packages("rvest")
library(rvest)
file<- read_html("https://en.wikipedia.org/wiki/Brazil_national_football_team")

?html_text

install.packages("RSQLite")
library(RSQLite)
data("mtcars")
mtcars
mtcars$carnames<- rownames(mtcars)
rownames(mtcars)<- c()
head(mtcars)
getwd()

# Create/connect database 
conn<- dbConnect(RSQLite :: SQLite(),"carsDB.db")

# write mtcars datasets into  a table names mtcars_data
dbWriteTable(conn, "cars_data" , mtcars)

# List all the data available in the database
dbListTables(conn)

# Quick acces fuctions
dbListFields(conn, "cars_data")
dbReadTable(conn, "cars_data")


# DATASETS SAMPLES
datasets::AirPassengers
datasets::Loblolly


# Executing SQL queries
dbGetQuery(conn, "SELECT * FROM cars_data LIMIT 10")

dbGetQuery(conn, " SELECT carnames, hp, cyl FROM cars_data WHERE cyl = 8")



# Get car names and HP starting names with M with 6 & 8 cylinders 
dbGetQuery(conn, "SELECT carnames, hp, cyl FROM cars_data Where carnames LIKE 'M%' and cyl IN (6,8)")


# Get avg HP & mpg by numbers of cylinder group 
dbGetQuery(conn, "SELECT cyl, AVG (hp) AS 'average_hp', AVG (mpg) AS 'average_mpg' FROM cars_data Group by cyl ORDER by average_hp")

avg_hp.cyl <- dbGetQuery(conn, "SELECT cyl, AVG(hp) AS average_hp FROM cars_data GROUP by cyl ORDER by average_hp")
avg_hp.cyl
class(avg_hp.cyl)


#...............................................................................

# INSERT VERIABLES IN QUERIES
mpg<- 18
cyl<- 6
Result<- dbGetQuery(conn, "Select carnames, mpg, cyl From cars_data WHERE mpg >= ? AND cyl == ?", params=c(mpg,cyl))
Result


?dbExecute


# Visualize the table before deletion 
dbGetQuery(conn, "SELECT * FROM cars_data LIMIT 10")

# Delete the column belonging to Mazda RX4
dbExecute(conn, "DELETE FROM cars_data WHERE carnames = 'Mazda RX4'"

# Visualize new table after deletion 
dbGetQuery(conn, "SELECT * FROM cars_data LIMIT 10")

# Insert the data from Mazda RX4 ..... output would be 1
dbExecute(conn, "INSERT INTO cars_data VALUES (21.0,6,160.0,110,3.90,2.620,16.46,0,1,4,4,'Mazda Rx4')")

# See that we re-introduced Mazda RX4 succesfully at the end                       :(
dbGetQuery(conn, "SELECT * From cars_data LIMIT 10")

install.packages("sqldf")
library(sqldf)


# You can fetch all results 
res <- dbSendQuery(conn, "SELECT * FROM cars_data WHERE cyl = 6 ")
dbFetch(res)

# Clear the result 
dbClearResult(res)

#Chunk at a time
res<- dbSendQuery(conn, "SELECT * FROM cars_data WHERE cyl=8")

while(!dbHasCompleted(res)){
  chunk<- dbFetch(res, n=5)
  print(nrow(chunk))
}

# Clear the result
dbClearResult(res)

# Disconnect from the database
dbDisconnect(conn)


library(RMySQL)
Db<-dbConnect(MySQL(), user="genome", host="genome-mysql.cse.ucsc.edu")
Result<-dbGetQuery(ucsc,"show database;") ; dbDisconnect(ucscDb)
Result

library(data.table)
DF = data.frame(x=rnorm(9), y=rep(c("a","b","c"), each=3),z=rnorm(9))
head(DF,3)
?rnorm

table()
DF[,list(x), sum(z)]
DT
 
table(DF$y)
DT[,table(y)]
View(DT)
table(DT$y)

# adding new column
DT[,w: z^z]

# logical operation for new variable 
DT[, a: =  x>0]

#...............................................................................
# Merge data table

DT1<- data.table(x=c('a','a','b','DT1'), y=1:4)
DT1
DT2<- data.table(x=c('a','b','dt2'), z=5:7)
DT2
setkey(DT1,x); setkey(DT2, x)
merge(DT1, DT2)

#...............................................................................
# Data Table sort with data frame
?data.table::fsort

cameradata<-read.csv('cameras.csv')
setwd('C:/Users/Dell/Documents/R/Data-Collection-Wrangling/data')
getwd()
cdata<-read.table('./data/cameras.csv',sep = ',',header = TRUE)
class(cdata)
data.table::fsort(cdata$street)
view(cdata)
#...............................................................................

# Subsetting & sorting
set.seed(13435)
x<-data.frame("var1"=sample(1:5),"var2"=sample(6:10),"var3"=sample(11:15))
x
#x<-x[sample(1:5)]; x$var2[c(1,3)]=NA
x[,1]
x[,"var1"]
x[1:2,"var2"]
#     x[x$var1<=3 & x$var3>11), ]
#     x[(x$var1<=3 & x$var3>15 ), ]

#...............................................................................
 
#                                           Sorting 

?sort()
# sort(x, decreasing = FALSE, na.last = TRUE, ...)    NA will be at last

sort(x$var1)
sort(x$var1, decreasing=TRUE)
sort(x$var2,na.last = TRUE)

################################################################################
# Ordering in DataFrame 
x[order(x$var1),]
x[order(x$var1, x$var3),]
View(DT)
DT[order(DT$x),]
DT[order(DT$y),]
DT[order(-DT$y),]
DT[order(-DT$y, DT$z),]
#...............................................................................
# Dealing with missing values

x<-c(1:4,NA,6:7,NA)

c

x

which(is.na(x))

sum(is.na(x))

colSums(is.na(x))

round(x,2)

df<- data.frame(col1=c(1:3, col2=c(2.5,4.2,99,3.2457)))
df

round(df,2)

df[df==99]<-NA
df

is.na(df)

is.na(df$col4)

which(is.na(df))

sum(is.na(df))

colSums(is.na(df))

rowSums(is.na(df))

#.............................RECODE / IMPUTE MISSING VALUES....................

df$col4[is.na(df$col4)]<- mean(df$col4, na.rm = TRUE)
is.na(df$col4)
df

#.............................EXCLUDING MISSING VALUES..........................

X<-c(1:4,NA,6:7,NA)
x
mean(X)
mean(x,na.rm=TRUE)

complete.cases(x)
x[complete.cases(x)]
x[!complete.cases(x)]
na.omit(x)

#//////////////////////////// MERGE / JOIN DATA FRAMES /////////////////////////
activity<- data.frame(opid=c("op01","op02","op03","op04","op05","op06","op07"), units=c(23,43,21,32,13,12,32))
names<- data.frame(operator=c("Larrg","Curly","Moe","Jack","Jill","KIm","Perry"))
blended<- cbind(activity,names)
blended

rblended<- rbind(blended, blended)
rblended

#lllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllll
?merge
names(reviews)
names(solutions)
mergedata = merge(reviews, solutions, by.x="solution_id", by.y="id", all= TRUE)
head(mergedata)
#-----------------------------------------------------------------------------#

df1<- data.frame(LETTERS, dfindex= 1:26)
df2<- data.frame(LETTERS, dfindex=c(1:10,15,20,22:35))
df1
df1
merge(df1, df2)
merge(df1, df2, all = TRUE )
names(df1)<- c("alpha", "lotsaNumbers")
df1
merge(df1, df2, by.x = "lotsaNumbers", by.y = "dfindex")
merge(df1, df2, by.x = "df1col", by.y = "df2col", all.x = TRUE, all.y = FALSE)




