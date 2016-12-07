---
layout: post
title: Tidy Data in Python
---

I recently came across a paper named [Tidy Data](http://vita.had.co.nz/papers/tidy-data.pdf) by Hadley Wickham. Published back in 2014, the paper focuses on one aspect of cleaning up data, tidying data: structuring datasets to facilitate analysis. Through the paper, Wickham demonstrates how any dataset can be structured in a standardized way prior to analysis. He presents in detail the different types of data sets and how to wrangle them into a standard format.

As a data scientist, I think you should get very familiar with this standardized structure of a dataset. Data cleaning is one the most frequent task in data science. No matter what kind of data you are dealing with or what kind of analysis you are performing, you will have to clean the data at some point. Tidying your data in a standard format makes things easier down the road. You can reuse a standard set of tools across your different analysis. 

In this post, I will summarize some tidying examples Wickham uses in his paper and I will demonstrate how to do so using the Python [pandas](http://pandas.pydata.org/) library.

## Defining tidy data
The structure Wickham defines as tidy has the following attributes:

- Each *variable* forms a column and contains *values*
- Each *observation* forms a row
- Each type of *observational unit* forms a table

A few definitions:

- Variable: A measurement or an attribute. <small>_Height, weight, sex, etc._</small>
- Value: The actual measurement or attribute. <small>_152 cm, 80 kg, female, etc._</small>
- Observation: All values measure on the same unit. <small>_Each person._</small>

An example of a *messy dataset*:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th></th>
      <th>Treatment A</th>
      <th>Treatment B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>John Smith</th>
      <td>-</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Jane Doe</th>
      <td>16</td>
      <td>11</td>
    </tr>
    <tr>
      <th>Mary Johnson</th>
      <td>3</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>

An example of a *tidy dataset*:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>Name</th>
      <th>Treatment</th>
      <th>Result</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>John Smith</th>
      <td>a</td>
      <td>-</td>
    </tr>
    <tr>
      <th>Jane Doe</th>
      <td>a</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Mary Johnson</th>
      <td>a</td>
      <td>3</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>b</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Jane Doe</th>
      <td>b</td>
      <td>11</td>
    </tr>
    <tr>
      <th>Mary Johnson</th>
      <td>b</td>
      <td>1</td>
    </tr>    
  </tbody>
</table>
</div>

## Tidying messy datasets

Through the following examples extracted from Wickham's paper, we'll wrangle messy datasets into the tidy format. The goal here is not to analyze the datasets but rather prepare them in a standardized way prior to the analysis. These are the five types of messy datasets we'll tackle:

- Column headers are values, not variable names.
- Multiple variables are stored in one column.
- Variables are stored in both rows and columns.
- Multiple types of observational units are stored in the same table.
- A single observational unit is stored in multiple tables.

###  Column headers are values, not variable names

#### Pew Research Center Dataset
This dataset explores the relationship between income and religion.

Problem: The columns headers are composed of the possible income values. 

{% highlight python %}
import pandas as pd
import datetime
from os import listdir
from os.path import isfile, join
import glob
import re

df = pd.read_csv("./data/pew-raw.csv")
df
{% endhighlight %}

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>religion</th>
      <th>&lt;$10k</th>
      <th>$10-20k</th>
      <th>$20-30k</th>
      <th>$30-40k</th>
      <th>$40-50k</th>
      <th>$50-75k</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Agnostic</td>
      <td>27</td>
      <td>34</td>
      <td>60</td>
      <td>81</td>
      <td>76</td>
      <td>137</td>
    </tr>
    <tr>
      <td>Atheist</td>
      <td>12</td>
      <td>27</td>
      <td>37</td>
      <td>52</td>
      <td>35</td>
      <td>70</td>
    </tr>
    <tr>
      <td>Buddhist</td>
      <td>27</td>
      <td>21</td>
      <td>30</td>
      <td>34</td>
      <td>33</td>
      <td>58</td>
    </tr>
    <tr>
      <td>Catholic</td>
      <td>418</td>
      <td>617</td>
      <td>732</td>
      <td>670</td>
      <td>638</td>
      <td>1116</td>
    </tr>
    <tr>
      <td>Dont know/refused</td>
      <td>15</td>
      <td>14</td>
      <td>15</td>
      <td>11</td>
      <td>10</td>
      <td>35</td>
    </tr>
    <tr>
      <td>Evangelical Prot</td>
      <td>575</td>
      <td>869</td>
      <td>1064</td>
      <td>982</td>
      <td>881</td>
      <td>1486</td>
    </tr>
    <tr>
      <td>Hindu</td>
      <td>1</td>
      <td>9</td>
      <td>7</td>
      <td>9</td>
      <td>11</td>
      <td>34</td>
    </tr>
    <tr>
      <td>Historically Black Prot</td>
      <td>228</td>
      <td>244</td>
      <td>236</td>
      <td>238</td>
      <td>197</td>
      <td>223</td>
    </tr>
    <tr>
      <td>Jehovahs Witness</td>
      <td>20</td>
      <td>27</td>
      <td>24</td>
      <td>24</td>
      <td>21</td>
      <td>30</td>
    </tr>
    <tr>
      <td>Jewish</td>
      <td>19</td>
      <td>19</td>
      <td>25</td>
      <td>25</td>
      <td>30</td>
      <td>95</td>
    </tr>
  </tbody>
</table>
</div>

A tidy version of this dataset is one in which the income values would not be columns headers but rather values in an `income` column. In order to tidy this dataset, we need to [melt](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.melt.html) it. The *pandas* library has a built-in function that allows to do just that. It "unpivots" a DataFrame from a wide format to a long format. We'll reuse this function a few times through the post.

{% highlight python %}
formatted_df = pd.melt(df,
                       ["religion"],
                       var_name="income",
                       value_name="freq")
formatted_df = formatted_df.sort_values(by=["religion"])
formatted_df.head(10)
{% endhighlight %}

This outputs a tidy version of the dataset:
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>religion</th>
      <th>income</th>
      <th>freq</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Agnostic</td>
      <td>&lt;$10k</td>
      <td>27</td>
    </tr>
    <tr>
      <td>Agnostic</td>
      <td>$30-40k</td>
      <td>81</td>
    </tr>
    <tr>
      <td>Agnostic</td>
      <td>$40-50k</td>
      <td>76</td>
    </tr>
    <tr>
      <td>Agnostic</td>
      <td>$50-75k</td>
      <td>137</td>
    </tr>
    <tr>
      <td>Agnostic</td>
      <td>$10-20k</td>
      <td>34</td>
    </tr>
    <tr>
      <td>Agnostic</td>
      <td>$20-30k</td>
      <td>60</td>
    </tr>
    <tr>
      <td>Atheist</td>
      <td>$40-50k</td>
      <td>35</td>
    </tr>
    <tr>
      <td>Atheist</td>
      <td>$20-30k</td>
      <td>37</td>
    </tr>
    <tr>
      <td>Atheist</td>
      <td>$10-20k</td>
      <td>27</td>
    </tr>
    <tr>
      <td>Atheist</td>
      <td>$30-40k</td>
      <td>52</td>
    </tr>
  </tbody>
</table>
</div>

#### Billboard Top 100 Dataset

This dataset represents the weekly rank of songs from the moment they enter the Billboard Top 100 to the subsequent 75 weeks.

Problems: 

- The columns headers are composed of values: the week number (`x1st.week`, ...)
- If a song is in the Top 100 for less than 75 weeks, the remaining columns are filled with missing values. 

{% highlight python %}
df = pd.read_csv("./data/billboard.csv", encoding="mac_latin2")
df.head(10)
{% endhighlight %}

<div>
<table border="1" class="dataframe" style="table-layout: fixed;">
  <thead style="word-wrap: break-word;">
    <tr style="text-align: left;">
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>date.entered</th>
      <th>date.peaked</th>
      <th>x1st.week</th>
      <th>x2nd.week</th>
      <th>...</th>
    </tr>
  </thead>
  <tbody style="word-wrap: break-word;">
    <tr>
      <td>2000</td>
      <td>Destiny's Child</td>
      <td>Independent Women Part I</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-09-23</td>
      <td>2000-11-18</td>
      <td>78</td>
      <td>63.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Santana</td>
      <td>Maria, Maria</td>
      <td>4:18</td>
      <td>Rock</td>
      <td>2000-02-12</td>
      <td>2000-04-08</td>
      <td>15</td>
      <td>8.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Savage Garden</td>
      <td>I Knew I Loved You</td>
      <td>4:07</td>
      <td>Rock</td>
      <td>1999-10-23</td>
      <td>2000-01-29</td>
      <td>71</td>
      <td>48.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Madonna</td>
      <td>Music</td>
      <td>3:45</td>
      <td>Rock</td>
      <td>2000-08-12</td>
      <td>2000-09-16</td>
      <td>41</td>
      <td>23.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Aguilera, Christina</td>
      <td>Come On Over Baby (All I Want Is You)</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-08-05</td>
      <td>2000-10-14</td>
      <td>57</td>
      <td>47.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Janet</td>
      <td>Doesn't Really Matter</td>
      <td>4:17</td>
      <td>Rock</td>
      <td>2000-06-17</td>
      <td>2000-08-26</td>
      <td>59</td>
      <td>52.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Destiny's Child</td>
      <td>Say My Name</td>
      <td>4:31</td>
      <td>Rock</td>
      <td>1999-12-25</td>
      <td>2000-03-18</td>
      <td>83</td>
      <td>83.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Iglesias, Enrique</td>
      <td>Be With You</td>
      <td>3:36</td>
      <td>Latin</td>
      <td>2000-04-01</td>
      <td>2000-06-24</td>
      <td>63</td>
      <td>45.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Sisqo</td>
      <td>Incomplete</td>
      <td>3:52</td>
      <td>Rock</td>
      <td>2000-06-24</td>
      <td>2000-08-12</td>
      <td>77</td>
      <td>66.0</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Lonestar</td>
      <td>Amazed</td>
      <td>4:25</td>
      <td>Country</td>
      <td>1999-06-05</td>
      <td>2000-03-04</td>
      <td>81</td>
      <td>54.0</td>
      <td>...</td>
    </tr>
  </tbody>
</table>
</div>

A tidy version of this dataset is one without the week's numbers as columns but rather as values of a single column. In order to do so, we'll *melt* the weeks columns into a single `date` column. We will create one row per week for each record. If there is no data for the given week, we will not create a row.

{% highlight python %}
# Melting
id_vars = ["year",
           "artist.inverted",
           "track",
           "time",
           "genre",
           "date.entered",
           "date.peaked"]

df = pd.melt(frame=df,id_vars=id_vars, var_name="week", value_name="rank")

# Formatting 
df["week"] = df['week'].str.extract('(\d+)', expand=False).astype(int)
df["rank"] = df["rank"].astype(int)

# Cleaning out unnecessary rows
df = df.dropna()

# Create "date" columns
df['date'] = pd.to_datetime(df['date.entered']) + pd.to_timedelta(df['week'], unit='w') - pd.DateOffset(weeks=1)

df = df[["year", 
         "artist.inverted",
         "track",
         "time",
         "genre",
         "week",
         "rank",
         "date"]]
df = df.sort_values(ascending=True, by=["year","artist.inverted","track","week","rank"])

# Assigning the tidy dataset to a variable for future usage
billboard = df

df.head(10)
{% endhighlight %}


A tidier version of the dataset is shown below. There is still a lot of repetition of the song details: the track name, time and genre. For this reason, this dataset is still not completely tidy as per Wickham's definition. We will address this in the next example.

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>week</th>
      <th>rank</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>1</td>
      <td>87</td>
      <td>2000-02-26</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>2</td>
      <td>82</td>
      <td>2000-03-04</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>3</td>
      <td>72</td>
      <td>2000-03-11</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>4</td>
      <td>77</td>
      <td>2000-03-18</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>5</td>
      <td>87</td>
      <td>2000-03-25</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>6</td>
      <td>94</td>
      <td>2000-04-01</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>7</td>
      <td>99</td>
      <td>2000-04-08</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2Ge+her</td>
      <td>The Hardest Part Of Breaking Up (Is Getting Ba...</td>
      <td>3:15</td>
      <td>R&amp;B</td>
      <td>1</td>
      <td>91</td>
      <td>2000-09-02</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2Ge+her</td>
      <td>The Hardest Part Of Breaking Up (Is Getting Ba...</td>
      <td>3:15</td>
      <td>R&amp;B</td>
      <td>2</td>
      <td>87</td>
      <td>2000-09-09</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2Ge+her</td>
      <td>The Hardest Part Of Breaking Up (Is Getting Ba...</td>
      <td>3:15</td>
      <td>R&amp;B</td>
      <td>3</td>
      <td>92</td>
      <td>2000-09-16</td>
    </tr>
  </tbody>
</table>
</div>

### Multiple types in one table

Following up on the Billboard dataset, we'll now address the repetition problem of the previous table.

Problems: 

- Multiple observational units (the `song` and its `rank`) in a single table.

We'll first create a `songs` table which contains the details of each song:
{% highlight python %}
songs_cols = ["year", "artist.inverted", "track", "time", "genre"]
songs = billboard[songs_cols].drop_duplicates()
songs = songs.reset_index(drop=True)
songs["song_id"] = songs.index
songs.head(10)
{% endhighlight %}

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>song_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2000</td>
      <td>2 Pac</td>
      <td>Baby Don't Cry (Keep Ya Head Up II)</td>
      <td>4:22</td>
      <td>Rap</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>2Ge+her</td>
      <td>The Hardest Part Of Breaking Up (Is Getting Ba...</td>
      <td>3:15</td>
      <td>R&amp;B</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>3 Doors Down</td>
      <td>Kryptonite</td>
      <td>3:53</td>
      <td>Rock</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>3 Doors Down</td>
      <td>Loser</td>
      <td>4:24</td>
      <td>Rock</td>
      <td>3</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>504 Boyz</td>
      <td>Wobble Wobble</td>
      <td>3:35</td>
      <td>Rap</td>
      <td>4</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>98�</td>
      <td>Give Me Just One Night (Una Noche)</td>
      <td>3:24</td>
      <td>Rock</td>
      <td>5</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>A*Teens</td>
      <td>Dancing Queen</td>
      <td>3:44</td>
      <td>Pop</td>
      <td>6</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Aaliyah</td>
      <td>I Don't Wanna</td>
      <td>4:15</td>
      <td>Rock</td>
      <td>7</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Aaliyah</td>
      <td>Try Again</td>
      <td>4:03</td>
      <td>Rock</td>
      <td>8</td>
    </tr>
    <tr>
      <td>2000</td>
      <td>Adams, Yolanda</td>
      <td>Open My Heart</td>
      <td>5:30</td>
      <td>Gospel</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>

We'll then create a `ranks` table which only contains the `song_id`, `date` and the `rank`.

{% highlight python %}
ranks = pd.merge(billboard, songs, on=["year","artist.inverted", "track", "time", "genre"])
ranks = ranks[["song_id", "date","rank"]]
ranks.head(10)
{% endhighlight %}

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>song_id</th>
      <th>date</th>
      <th>rank</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2000-02-26</td>
      <td>87</td>
    </tr>
    <tr>
      <td>0</td>
      <td>2000-03-04</td>
      <td>82</td>
    </tr>
    <tr>
      <td>0</td>
      <td>2000-03-11</td>
      <td>72</td>
    </tr>
    <tr>
      <td>0</td>
      <td>2000-03-18</td>
      <td>77</td>
    </tr>
    <tr>
      <td>0</td>
      <td>2000-03-25</td>
      <td>87</td>
    </tr>
    <tr>
      <td>0</td>
      <td>2000-04-01</td>
      <td>94</td>
    </tr>
    <tr>
      <td>0</td>
      <td>2000-04-08</td>
      <td>99</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2000-09-02</td>
      <td>91</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2000-09-09</td>
      <td>87</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2000-09-16</td>
      <td>92</td>
    </tr>
  </tbody>
</table>
</div>

###  Multiple variables stored in one column

#### Tubercolosis Records from World Health Organization
This dataset documents the count of confirmed tuberculosis cases by country, year, age and sex.

Problems: 

- Some columns contain multiple values: sex and age.
- Mixture of zeros and missing values `NaN`. This is due to the data collection process and the distinction is important for this dataset.

{% highlight python %}
df = pd.read_csv("./data/tb-raw.csv")
df
{% endhighlight %}

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>country</th>
      <th>year</th>
      <th>m014</th>
      <th>m1524</th>
      <th>m2534</th>
      <th>m3544</th>
      <th>m4554</th>
      <th>m5564</th>
      <th>m65</th>
      <th>mu</th>
      <th>f014</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>AE</td>
      <td>2000</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>5</td>
      <td>12</td>
      <td>10</td>
      <td>NaN</td>
      <td>3</td>
    </tr>
    <tr>
      <td>AF</td>
      <td>2000</td>
      <td>52</td>
      <td>228</td>
      <td>183</td>
      <td>149</td>
      <td>129</td>
      <td>94</td>
      <td>80</td>
      <td>NaN</td>
      <td>93</td>
    </tr>
    <tr>
      <td>AG</td>
      <td>2000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <td>AL</td>
      <td>2000</td>
      <td>2</td>
      <td>19</td>
      <td>21</td>
      <td>14</td>
      <td>24</td>
      <td>19</td>
      <td>16</td>
      <td>NaN</td>
      <td>3</td>
    </tr>
    <tr>
      <td>AM</td>
      <td>2000</td>
      <td>2</td>
      <td>152</td>
      <td>130</td>
      <td>131</td>
      <td>63</td>
      <td>26</td>
      <td>21</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <td>AN</td>
      <td>2000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <td>AO</td>
      <td>2000</td>
      <td>186</td>
      <td>999</td>
      <td>1003</td>
      <td>912</td>
      <td>482</td>
      <td>312</td>
      <td>194</td>
      <td>NaN</td>
      <td>247</td>
    </tr>
    <tr>
      <td>AR</td>
      <td>2000</td>
      <td>97</td>
      <td>278</td>
      <td>594</td>
      <td>402</td>
      <td>419</td>
      <td>368</td>
      <td>330</td>
      <td>NaN</td>
      <td>121</td>
    </tr>
    <tr>
      <td>AS</td>
      <td>2000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

In order to tidy this dataset, we need to remove the different values from the header and unpivot them into rows. We'll first need to melt the `sex + age group` columns into a single one. Once we have that single column, we'll derive three columns from it: `sex`, `age_lower` and `age_upper`. With those, we'll be able to properly build a tidy dataset.

{% highlight python %}
df = pd.melt(df, id_vars=["country","year"], value_name="cases", var_name="sex_and_age")

# Extract Sex, Age lower bound and Age upper bound group
tmp_df = df["sex_and_age"].str.extract("(\D)(\d+)(\d{2})")    

# Name columns
tmp_df.columns = ["sex", "age_lower", "age_upper"]

# Create `age`column based on `age_lower` and `age_upper`
tmp_df["age"] = tmp_df["age_lower"] + "-" + tmp_df["age_upper"]

# Merge 
df = pd.concat([df, tmp_df], axis=1)

# Drop unnecessary columns and rows
df = df.drop(['sex_and_age',"age_lower","age_upper"], axis=1)
df = df.dropna()
df = df.sort(ascending=True,columns=["country", "year", "sex", "age"])
df.head(10)
{% endhighlight %}

This results in a *tidy dataset*.
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>country</th>
      <th>year</th>
      <th>cases</th>
      <th>sex</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>0</td>
      <td>m</td>
      <td>0-14</td>
    </tr>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>0</td>
      <td>m</td>
      <td>15-24</td>
    </tr>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>1</td>
      <td>m</td>
      <td>25-34</td>
    </tr>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>0</td>
      <td>m</td>
      <td>35-44</td>
    </tr>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>0</td>
      <td>m</td>
      <td>45-54</td>
    </tr>
    <tr>
      <td>AD</td>
      <td>2000</td>
      <td>0</td>
      <td>m</td>
      <td>55-64</td>
    </tr>
    <tr>
      <td>AE</td>
      <td>2000</td>
      <td>3</td>
      <td>f</td>
      <td>0-14</td>
    </tr>
    <tr>
      <td>AE</td>
      <td>2000</td>
      <td>2</td>
      <td>m</td>
      <td>0-14</td>
    </tr>
    <tr>
      <td>AE</td>
      <td>2000</td>
      <td>4</td>
      <td>m</td>
      <td>15-24</td>
    </tr>
    <tr>
      <td>AE</td>
      <td>2000</td>
      <td>4</td>
      <td>m</td>
      <td>25-34</td>
    </tr>
  </tbody>
</table>
</div>

### Variables are stored in both rows and columns

#### Global Historical Climatology Network Dataset
This dataset represents the daily weather records for a weather station (MX17004) in Mexico for five months in 2010.

Problems: 

- Variables are stored in both rows (`tmin`, `tmax`) and columns (`days`).

{% highlight python %}
df = pd.read_csv("./data/weather-raw.csv")
df
{% endhighlight %}

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>id</th>
      <th>year</th>
      <th>month</th>
      <th>element</th>
      <th>d1</th>
      <th>d2</th>
      <th>d3</th>
      <th>d4</th>
      <th>d5</th>
      <th>d6</th>
      <th>d7</th>
      <th>d8</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>1</td>
      <td>tmax</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>1</td>
      <td>tmin</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>2</td>
      <td>tmax</td>
      <td>NaN</td>
      <td>27.3</td>
      <td>24.1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>2</td>
      <td>tmin</td>
      <td>NaN</td>
      <td>14.4</td>
      <td>14.4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>3</td>
      <td>tmax</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>32.1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>3</td>
      <td>tmin</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>4</td>
      <td>tmax</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>4</td>
      <td>tmin</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>5</td>
      <td>tmax</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>MX17004</td>
      <td>2010</td>
      <td>5</td>
      <td>tmin</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

In order to make this dataset tidy, we want to move the three misplaced variables (`tmin`, `tmax` and `days`) as three individual columns: `tmin`. `tmax` and `date`. 

{% highlight python %}
# Extracting day
df["day"] = df["day_raw"].str.extract("d(\d+)", expand=False)  
df["id"] = "MX17004"

# To numeric values
df[["year","month","day"]] = df[["year","month","day"]].apply(lambda x: pd.to_numeric(x, errors='ignore'))

# Creating a date from the different columns
def create_date_from_year_month_day(row):
    return datetime.datetime(year=row["year"], month=int(row["month"]), day=row["day"])

df["date"] = df.apply(lambda row: create_date_from_year_month_day(row), axis=1)
df = df.drop(['year',"month","day", "day_raw"], axis=1)
df = df.dropna()

# Unmelting column "element"
df = df.pivot_table(index=["id","date"], columns="element", values="value")
df.reset_index(drop=False, inplace=True)
df
{% endhighlight %}

<div>
  <table border="1" class="dataframe">
    <thead>
      <tr style="text-align: left;">
        <th>id</th>
        <th>date</th>
        <th>tmax</th>
        <th>tmin</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>MX17004</td>
        <td>2010-02-02</td>
        <td>27.3</td>
        <td>14.4</td>
      </tr>
      <tr>
        <td>MX17004</td>
        <td>2010-02-03</td>
        <td>24.1</td>
        <td>14.4</td>
      </tr>
      <tr>
        <td>MX17004</td>
        <td>2010-03-05</td>
        <td>32.1</td>
        <td>14.2</td>
      </tr>
    </tbody>
  </table>
</div>

### One type in multiple tables

Dataset: Illinois Male Baby Names for the year 2014/2015.

Problems: 

- The data is spread across multiple tables/files.
- The "Year" variable is present in the file name.

In order to load those different files into a single DataFrame, we can run a custom script that will append the files together. Furthermore, we'll need to extract the "Year" variable from the file name.

{% highlight python %}
def extract_year(string):
    match = re.match(".+(\d{4})", string) 
    if match != None: return match.group(1)
    
path = './data'
allFiles = glob.glob(path + "/201*-baby-names-illinois.csv")
frame = pd.DataFrame()
df_list= []
for file_ in allFiles:
    df = pd.read_csv(file_,index_col=None, header=0)
    df.columns = map(str.lower, df.columns)
    df["year"] = extract_year(file_)
    df_list.append(df)
    
df = pd.concat(df_list)
df.head(5)
{% endhighlight %}

<div>
    <table border="1" class="dataframe">
      <thead>
            <tr style="text-align: left;">
                <th>rank</th>
                <th>name</th>
                <th>frequency</th>
                <th>sex</th>
                <th>year</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>Noah</td>
                <td>837</td>
                <td>Male</td>
                <td>2014</td>
            </tr>
            <tr>
                <td>2</td>
                <td>Alexander</td>
                <td>747</td>
                <td>Male</td>
                <td>2014</td>
            </tr>
            <tr>
                <td>3</td>
                <td>William</td>
                <td>687</td>
                <td>Male</td>
                <td>2014</td>
            </tr>
            <tr>
                <td>4</td>
                <td>Michael</td>
                <td>680</td>
                <td>Male</td>
                <td>2014</td>
            </tr>
            <tr>
                <td>5</td>
                <td>Liam</td>
                <td>670</td>
                <td>Male</td>
                <td>2014</td>
            </tr>
        </tbody>
    </table>
</div>

## Final Thoughts

In this post, I focused on one aspect of Wickham's paper, the data manipulation part. My main goal was to demonstrate the data manipulations in Python. It's important to mention that there is a significant section of his paper that covers the tools and visualizations from which you can benefit by tidying your dataset. I did not cover those in this post. 

Overall, I enjoyed preparing this post and wrangling the datasets into a streamlined format. The defined format makes it easier to query and filter the data. This approach makes it easier to reuse libraries and code across analysis. It also makes it easier to share a dataset with other data analysts.
