---
layout: post
title:  Descriptive Statistics in SQL
---

I recently wrote an article on why you should [learn SQL for data analysis]({% post_url 2016-03-08-sql-for-data-analysis %}). I received lots of feedback from this post. People wanted to see some concrete examples of using SQL for data analysis. I decided to apply some of my own advice and load up one of these [awesome datasets](https://github.com/caesar0301/awesome-public-datasets) to do some basic data exploration queries on it.

For this post, I am using an open data set. The million songs data sets is a _«freely-available collection of audio features and metadata for a million contemporary popular music tracks»_. 

If you want to follow along, here are the steps:

1. Download the Million Song Data Set. [I used the 10K songs subset](http://labrosa.ee.columbia.edu/millionsong/pages/getting-dataset#subset).
2. Download a [SQLite Client](http://sqlitebrowser.org/)
3. Open `subset_track_metadata.db` file with SQLite and start exploring


## Why Descriptive Statistics?

When I start the analysis of a new data set, I run some basic queries to get a sense of how the data is organized, how the values are distributed. I am trying to understand what I'm dealing with. For numerical data, I will often run some descriptive statistics on the data set: I will measure the central tendancy (_mean_, _median_, _mode_) and measure the level of variability of the data. Those measurements are generally a good start to data exploration. They will often lead to new questions that fuel my analysis.
    
## Central Tendency 

### Mean 

The mean is the number you obtain when you sum up a given set of numbers and then divide this sum by the total number in the set. The mean is very sensible to outliers. It can be drastically affected by values that are much higher or lower compared to the rest of the data set. 

{% highlight sql %}
SELECT CAST(AVG(songs.year) as int) as avg_year FROM songs

-- | avg_year |
-- |----------|
-- | 934      |
{% endhighlight %}

- `CAST`: _Run-time data type conversion between compatible data types. In this case, I'm converting a float to an integer for rounding purpose._
- `AVG`: _Aggregation function that returns the mean of the input expression value._
- `as avg_year`: _Temporarily renames a column heading - This is only for readbility purpose, to make the code more human-friendly. I will use this aliasing throughout the post._


### Median

The median is the number separating the higher half of a ordered data set from the lower half. The median is sometimes a better measure of a mid point because each data point is weighted equally.

{% highlight sql %}
SELECT songs.year as median_year
FROM songs 
ORDER BY songs.year 
LIMIT 1 
OFFSET (SELECT COUNT(*) FROM songs) / 2

-- | median_year |
-- |-------------|
-- | 0           |
{% endhighlight %}

- `ORDER BY`: _Sorts the resulting data set by one or more column. The ordering can be ascending `ASC` (default) or descending `DESC`._
- `COUNT`: _Aggregation function that returns the # of rows that matches a criteria._
- `LIMIT`: _Specifies the maximum number of rows that can be returned from the resulting data set._
- `OFFSET`: _Skip X rows before beginning to return rows. In this specific example, we are skipping 5000 rows (which is equal to the total row count `COUNT(*)` divided by 2)._    




### Mode

The mode is the value that appears most often in a set of data.

{% highlight sql %}
SELECT 
    songs.year,
    COUNT(*) as count
FROM songs
GROUP BY songs.year
ORDER BY COUNT(*) DESC
LIMIT 1

-- | year | count |
-- |------|-------|
-- | 0    | 5320  |
{% endhighlight %}

- `GROUP BY`: _Used with aggregation functions such as `COUNT`, `AVG`, etc. Groups the resulting data set by one or more column._    

## Variability

### Min/Max Values

Minimum/Maximum value in a data set.

{% highlight sql %}
SELECT 
    MIN(songs.year) as min_year,
    MAX(songs.year) as max_year
FROM
    songs

-- | min_year | max_year |
-- |----------|----------|
-- | 0        | 2010     |
{% endhighlight %}

- `MIN`: _Aggregation function that returns the smallest value in a data set._
- `MAX`: _Aggregation function that returns the largest value in a data set._

### Distribution of songs per year

Count of songs released in each years

{% highlight sql %}
SELECT 
    songs.year,
    COUNT(*) songs_count
FROM songs
GROUP BY songs.year
ORDER BY songs.year ASC

-- | year | song_count |
-- |------|------------|
-- | 0    | 5320       |
-- | 1926 | 2          |
-- | 1927 | 3          |
-- | ...  | ...        |
-- | 2009 | 250        |
-- | 2010 | 64         |
{% endhighlight %}

## Next Steps

The SQL queries in this post are fairly simple. They are not the result of technical gymmstastics. They are just simple measurements helping us understand the data set and that's what's great about them.

In this specific data set, we noticed that for more than half of the data set, the year of the song is equal to `0`.  This means we are either looking at a data set of very old songs or that we are dealing with missing values. The latter is more realistic. If we filter out songs from year `0`, our data makes more sense. The songs are ranging from `1926` to `2010` and the median is the year `2001`.

With the data partially cleaned up, we can start exploring other columns of our data set and asking ourselves more questions: How many unique artists composed songs per year? How has that evolved through time? Are songs shorter nowadays than before? What's great about those simple measurements is that they can be reused as our queries and filters get more and more complex. By applying the simple descriptive statistics measurements, we can have a good grasps of the data and we can keep exploring deeper and deeper.


