# Data Modeling with Postgres: Sparkify
by **Adriano Vilela**

## 1. Discuss the purpose of this database in the context of the startup, Sparkify, and their analytical goals.
---
The startup `Sparkify` wants to analyze the data they are collecting about music and user activity in its new music streaming app.
The analysis team is particularly interested in:
- Facilitate the consultation of your data, which logs are written in JSON format in a directory;
- Understand what songs users are listening to;
- Optimize queries when analyzing music playback.



## 2. State and justify your database schema design and ETL pipeline.
---

## 2.1. Data Schema
---
The fact and dimension tables have been defined for a schema that optimizes queries and analysis of music playback.

#### Fact Table

- **songplays** - records in log data associated with song plays i.e. records with page NextSong songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

#### Dimension Tables
- **users** - users in the app user_id, first_name, last_name, gender, level

- **songs** - songs in music database song_id, title, artist_id, year, duration

- **artists** - artists in music database artist_id, name, location, lattitude, longitude

- **time** - timestamps of records in songplays broken down into specific units start_time, hour, day, week, month, year, weekday


## 2.2. Python scripts

---
Script execution sequence:
1. **create_tables.py:** Clean previous schema and creates tables.

2. **sql_queries.py:** All queries used in the ETL pipeline.

3. **etl.py:** Read JSON logs and JSON metadata and load the data into generated tables.

**Note:** I created the `PROJETC_RUN.ipynb` to execute the sequence;


## 2.3. ETL Pipeline

---
### song_data
Each file is in JSON format and contains metadata about a song and the artist of that song.

**Create the Tables**: `songs` and `artists`.


### log_data
These simulate activity logs from a music streaming app based on specified configurations.

**Create the Tables**: `songplays`, `users` and `time`.

## 2.4. ETL Scripts

---
An ETL script automatically loops through the logs and songs directories, transforms the data using Python/Pandas, and inserts it on the star-schema with relationships, where appropriate.

## 3. Provide example queries and results for song play analysis.
---

1. Number of times you listened to a song:

        Select  
            songs.title, 
            artists.name, 
            count(songplays.songplays_id) As  count_listen 
       From songplays 
       Left Join songs   ON songplays.song_id = songs.song_id 
       Left Join artists ON songplays.artist_id = artists.artist_id 
       Group BY 
            songs.title, 
            artists.name;
**Result**: 

 Title           | Name    | count_listen 
-----------------|---------|--------------
 None            | None    | 6819
'Setanta matins' | 'Elena' | 1


2. Top 10 users who listen to music the most
        Select  
            users.last_name, 
            users.first_name, 
            count(songplays.*) As  count_listen 
       From songplays 
      Left Join users   ON songplays.user_id = users.user_id 
      Group BY 
                users.last_name, 
                users.first_name
        Order by count_listen desc
        Limit 10;
  
 ** Result**: 
 

 Last Name       | First Name    | count_listen 
-----------------|---------------|--------------
'Cuevas'         | 'Chloe'       | 689
'Levine'         | 'Tegan'       | 665
'Harrell'        | 'Kate'        | 557
'Koch'           | 'Lily'        | 463
'Kirby'          | 'Aleena'      | 397
'Lynch'          | 'Jacqueline'  | 346
'Griffin'        | 'Layla'       | 321
'Klein'          | 'Jacob'       | 289
'Rodriguez'      | 'Mohammad'    | 270
'Jones'          | 'Matthew'     | 248