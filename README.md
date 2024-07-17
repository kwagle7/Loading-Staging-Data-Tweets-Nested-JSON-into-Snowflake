# Nested-JSON-in-Snowflake
Create a Database, Table &amp; File Format for Nested JSON Data

## Create Database
```sql
--Create a new database to hold the Twitter file
CREATE DATABASE SOCIAL_MEDIA_FLOODGATES 
COMMENT = 'There\'s so much data from social media - flood warning';

USE DATABASE SOCIAL_MEDIA_FLOODGATES;

--Create a table in the new database
CREATE TABLE SOCIAL_MEDIA_FLOODGATES.PUBLIC.TWEET_INGEST 
("RAW_STATUS" VARIANT) 
COMMENT = 'Bring in tweets, one row per tweet or status entity';

--Create a JSON file format in the new database
CREATE FILE FORMAT SOCIAL_MEDIA_FLOODGATES.PUBLIC.JSON_FILE_FORMAT 
TYPE = 'JSON' 
COMPRESSION = 'AUTO' 
ENABLE_OCTAL = FALSE 
ALLOW_DUPLICATE = FALSE 
STRIP_OUTER_ARRAY = TRUE 
STRIP_NULL_VALUES = FALSE 
IGNORE_UTF8_ERRORS = FALSE;
```
## Download the JSON Data File

nutrition_tweets.json

Sample Preview:
![image](https://github.com/user-attachments/assets/3395658d-4479-41a4-924c-a8613b7695ef)


## Load and View the Nested JSON File
```sql
copy into TWEET_INGEST
from @util_db.public.like_a_window_into_an_s3_bucket
files = ( 'nutrition_tweets.json')
file_format = ( format_name=SOCIAL_MEDIA_FLOODGATES.PUBLIC.JSON_FILE_FORMAT );

select * from tweet_ingest;
```
where JSON file is created as follows:
```sql
--Create File Format for JSON Data
CREATE or replace FILE FORMAT LIBRARY_CARD_CATALOG.PUBLIC.JSON_FILE_FORMAT 
TYPE = 'JSON' 
COMPRESSION = 'AUTO' 
ENABLE_OCTAL = false
ALLOW_DUPLICATE = false 
STRIP_OUTER_ARRAY = true
STRIP_NULL_VALUES = false 
IGNORE_UTF8_ERRORS = false ;
```
## Stating files (indexing/locating AWS s3 files in Snowflake)
![image](https://github.com/user-attachments/assets/62e97feb-c1d9-465f-b538-7b13e2431145)

