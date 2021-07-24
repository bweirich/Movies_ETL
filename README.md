# Movies_ETL

## Purpose 
The purpose of this analysis is to create an automated pipeline that takes in new movie data, performs the appropriate transformations, and loads the data into existing tables.  This data is going to be used in a competition to analyze to predict which low budget movies will become popular and thus be a good investment to license for streaming.  
 

## Data Sources
Data was sourced from the following websites:
1. Wikipedia
2. Kaggle
3. MovieLens

## ETL Process

### Wikipedia Data
The first data set to be extracted and transformed was the Wikipedia file.  The following steps were used to transform this data:
- Combine columns with alternate movie names into one column
- Combine columns that hold similar data

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/Combine_Columns.PNG)

- Remove columns that have greater than 90% null values

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/90_delete.PNG)

- Review and use regular expression to format box office and budget columns

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/Box_office.PNG)
![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/box_budget_clean.PNG)

- Review and use regular expression to format release date

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/release_date_clean.PNG)

- Review and use regular expression to format running time

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/running_time_clean.PNG)

### Kaggle Data
The second data set to be extracted and transformed was the Kaggle file.  The following steps were used to transform this data:
- Convert columns 'adult' and 'video' to boolean data type
- Convert columns 'budget', 'id', 'popularity' to numeric data type
- Convert 'release date' to datetime

### MovieLens (Rating) Data
The final data set to be extracted and transformed was the MovieLens file which holds ratings information.  The following steps were used to transform this data:
- Convert 'timestamp' to datetime
- Review statistics and distribution of ratings

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/ratings_hist.PNG)

- Create pivot dataframe to group by movie id and rating

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/ratings_pivot.PNG)

### Merge Wikipedia and Kaggle
The Wikipedia and Kaggle dataset were merged and the following columns were held competing data and were resolved in the following manner:

 Wikipedia                  Kaggle                    Resolution
_____________________________________________________________________________________
| title_wiki            |    title_kaggle          |   Drop Wiki                     |
| running_time          |    runtime               |   Keep Kaggle fill in with wiki |
| budget_wiki           |    budget_kaggle         |   Keep Kaggle fill in with wiki |
| box_office            |    revenue               |   Keep Kaggle fill in with wiki |
| release_date_wiki     |    release_date_kaggle   |   Drop Wiki                     |
| Language              |    original_language     |   Drop Wiki                     |
| Production company(s) |    production_companies  |   Drop Wiki                     |
|__________________________________________________________________________ _________|

##### Title 
The Kaggle data was used for the title column as it had less null values than the wikipedia.

##### Runtime 
The Kaggle data was used for the runtime column as it had less null values and fewer outliers but the wikipedia data was used to fill in null values.

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/runtime.PNG)

##### Budget 
The Kaggle data was used for the runtime column as it had less null values and fewer outliers but the wikipedia data was used to fill in null values.

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/budget.PNG)

##### Revenue 
The Kaggle data was used for the runtime column as it had less null values and fewer outliers but the wikipedia data was used to fill in null values.

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/revenue.PNG)

##### Release Date 
The Kaggle data was used for the release date column as it had less null values than the wikipedia.

![alt_text](https://raw.githubusercontent.com/bweirich/Movies_ETL/main/Images/release_date_scatter.PNG)

##### Original Language
The Kaggle data was used for the original language column as it has more uniform formatting than the wikipedia.

##### Production Companies
The Kaggle data was used for the production company column as it has more uniform formatting than the wikipedia.

### Reorder and Rename Columns
The columns were reordered and renamed to allow ease of viewing and understanding.

### Merge Combined with MovieLens
The combined dataframe of Wikipedia and Kaggle was then merged with the pivot ratings dataframe.

## Creating Pipeline 
A function was created to automate the above ETL to allow for new data.  A connection was created with a database to load the cleaned data into two tables.  The tables consist of a table of the merged Kaggle and Wikipedia datasets and a table of the ratings dataset.

This was further refactored to breakdown the large function into multiple smaller functions making modifications of each transformation easier and issues easier to troubleshoot.  
The result is a complete pipeline to extract, transform and load movie data into a database.
