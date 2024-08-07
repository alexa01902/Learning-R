# Using .csv files i R
## What is a .csv file?
Csv stands for 'comma seperated values' and it is a text file that uses commaas as delimiter to seperate values. It is one of the most common and simplest ways to store data, especially when storing datasets.

## What does a .csv file look like?
An example of what a .csv file could look like if you opend it with a text editor is

"ID","Name","Age","Grade"
1,"John Smith",15,"A"
2,"Jane Doe",16,"B"
3,"Emily Davis",14,"A-"
4,"Michael Brown",15,"B+"
5,"Sarah Wilson",16,"A"
6,"David Lee",14,"B-"

Each comma shows a seperation to a new column.

## How can I open .csv files?
Traditionally, you can open a .csv file with any spreadsheet app or program such as Excel, Google Sheets or LibreOffice Calc. Just use the program to open the file and choose the "separator" options appropriately (Usually "Separated by" or equivalent, then comma and/or semicolon)

## How can I create my own .csv files?
Spreadsheet apps have the optin to save tables as a .csv file.

From R, any dataframe object can be saved as a .csv file with the write.csv function. For example:

```r
mtcars <- mtcars # to make the data frame visible in the global environment in RStudio.

setwd("C:/Downloads") # To set as working directory where the file will be saved

write.csv(mtcars, file = "mtcars.csv")
```

## How do I load a .csv file in R?

To load a .csv file from your computer into R, you can use the command read.csv. For example, using the file created in the example above:
```r
setwd("C:/Downloads") 

mtcars_from_csv <- read.csv('mtcars.csv')
```

## Why is this important?

Input/Output (I/O) is an important concept when working with programming. It enables us to work with datasets acquired externally (such as downloaded from the internet) but also to later export again to share results with others.

