README

Modified by Nithya D , Thomas D , Juan

Assignment 6 Due Date:6/22

The Rolling House Repository has two Directories, Data, Analysis, and a single Readme.md

Inside the Data Directory there is various stages of the raw data in txt and csv
Inside the Analysis Diretory there is a clean version of the data in two different forms +Analysis of plots from the clean data like the ones below plus more
http://www1.nyc.gov/site/finance/taxes/property-rolling-sales-data.page

library(plyr) setwd("/testrepo/testrepo")

Check the data

head(man)
summary(man)
str(man) # Very handy function!
#Compactly display the internal structure of an R object.
Clean/format the data with regular expressions More on these later. For now, know that the pattern "[^[:digit:]]" refers to members of the variable name that start with digits. We use the gsub command to replace them with a blank space. We create a new variable that is a "clean' version of sale.price. And sale.price.n is numeric, not a factor.
man$SALE.PRICE.N <- as.numeric(gsub("[^[:digit:]]","", man$SALE.PRICE))
count(is.na(man$SALE.PRICE.N))

names(man) <- tolower(names(man)) # make all variable names lower case
Removed the 1000 delimiter from the sqft and converted the datatype to numeric for year

man$gross.sqft <- as.numeric(gsub("[^[:digit:]]","", man$gross.square.feet))
man$land.sqft <- as.numeric(gsub("[^[:digit:]]","", man$land.square.feet))
man$year.built <- as.numeric(as.character(man$year.built))
***
Doing a bit of exploration to make sure there's not anything weird going on with sale prices

attach(man)
hist(sale.price.n) 
detach(man)
Here's some cool Plots:  

keep only the actual sales
man.sale <- man[man$sale.price.n!=0,]
for now, let's look at 1-, 2-, and 3-family homes 

More Cleaning and Polishing


man.homes <- man.sale[which(grepl("FAMILY",man.sale$building.class.category)),]
dim(man.homes)
plot(log10(man.homes$gross.sqft),log10(man.homes$sale.price.n))
summary(man.homes[which(man.homes$sale.price.n<100000),])
 

Removed outliers that seem like they weren't actual sales

man.homes$outliers <- (log10(man.homes$sale.price.n) <=5) + 0
man.homes <- man.homes[which(man.homes$outliers==0),]
plot(log10(man.homes$gross.sqft),log10(man.homes$sale.price.n))
