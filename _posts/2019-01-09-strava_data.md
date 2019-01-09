---
#layout: post
title:  'Analyzing Strava data, part I'
classes: wide
date:   2019-01-09
instagram_id:
tags: []

---
After spending the past 2.5 months madly finishing up projects, cycling took a serious back seat. Which is OK, because as you will see, cycling took the driver's seat for much of 2018.

Now that the projects are done (or at least at v1), I've been itching to look at my strava data again, this time with python. I've used R in the past, simply so I could plot in ggplot2. But it was slooooow.

# Get the data


```python
# import librairies

import urllib2
import json
import pandas as pd

import matplotlib as plt
%matplotlib inline

import seaborn as sns
sns.set()

# For the base URL we use for activities
token = 'c3f1edec933f91fa719b27e2ffd1698ae90bdebe'
base_url = "https://www.strava.com/api/v3/activities?access_token=%s&per_page=200&page=" % token
page_num = 1

# To store the json response
my_acts = "my_activities.txt"

```


```python
cols = ["start_date","type","name","id","average_cadence",
        "average_heartrate","average_speed", "distance", 
          "moving_time", "location_city", "pr_count","suffer_score", 
          "trainer", "total_elevation_gain", "workout_type","map"]
```


```python
### Starter code adapted from http://pdwhomeautomation.blogspot.fr/2014/11/raspberry-pi-and-strava-api-1.html

def get_strava_data(token = token, filename = my_acts):
    #Open the my_file to use
    my_file = open(filename,'w')

    #Loop extracting data.  Remember it comes in pages
    end_found = False
    page_num = 1

    strava_df = pd.DataFrame(columns = cols)


    #Main loop - Getting all activities
    while (end_found == False):
        #Do a HTTP Get - First form the full URL
        act_url = base_url + str(page_num)
        strava_json = urllib2.urlopen(act_url).read()
        
        if strava_json != "[]":   # Checks for empty JSON response, i.e. end of activities
            #Now we process the JSON
            act_json = json.loads(strava_json)
            page_df = pd.DataFrame(act_json)

            # reduce number of fields in df
            # page_df = page_df[cols] TO FIX: key error with avg HR
            strava_df = strava_df.append(page_df)

            #Set up for next loop
            cum_sum = page_num * 200
            print("{} activities printed...".format(cum_sum))
            page_num += 1
            strava_json = []

        else:
            end_found = True
            print("*** No more activities. ***")
        
    return strava_df
```


```python
# create data frame

strava_df = get_strava_data()
```

    200 activities printed...
    400 activities printed...
    600 activities printed...
    800 activities printed...
    1000 activities printed...
    1200 activities printed...
    *** No more activities. ***





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>achievement_count</th>
      <th>athlete</th>
      <th>athlete_count</th>
      <th>average_cadence</th>
      <th>average_heartrate</th>
      <th>average_speed</th>
      <th>average_temp</th>
      <th>average_watts</th>
      <th>comment_count</th>
      <th>commute</th>
      <th>...</th>
      <th>timezone</th>
      <th>total_elevation_gain</th>
      <th>total_photo_count</th>
      <th>trainer</th>
      <th>type</th>
      <th>upload_id</th>
      <th>utc_offset</th>
      <th>visibility</th>
      <th>weighted_average_watts</th>
      <th>workout_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>2.0</td>
      <td>83.0</td>
      <td>151.1</td>
      <td>5.809</td>
      <td>NaN</td>
      <td>87.9</td>
      <td>0.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>767.0</td>
      <td>0.0</td>
      <td>False</td>
      <td>Ride</td>
      <td>2.200546e+09</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>False</td>
      <td>Workout</td>
      <td>NaN</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>134.9</td>
      <td>4.962</td>
      <td>NaN</td>
      <td>110.5</td>
      <td>1.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>True</td>
      <td>Ride</td>
      <td>2.191266e+09</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>135.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>False</td>
      <td>Workout</td>
      <td>NaN</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>85.9</td>
      <td>165.0</td>
      <td>2.724</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>55.3</td>
      <td>0.0</td>
      <td>False</td>
      <td>Run</td>
      <td>2.188359e+09</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>False</td>
      <td>Workout</td>
      <td>NaN</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>85.2</td>
      <td>175.8</td>
      <td>2.748</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>80.3</td>
      <td>0.0</td>
      <td>False</td>
      <td>Run</td>
      <td>2.137880e+09</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>86.3</td>
      <td>161.1</td>
      <td>2.725</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>123.1</td>
      <td>0.0</td>
      <td>False</td>
      <td>Run</td>
      <td>2.134223e+09</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>False</td>
      <td>Workout</td>
      <td>NaN</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5.0</td>
      <td>{u'resource_state': 1, u'id': 6061891}</td>
      <td>1.0</td>
      <td>85.4</td>
      <td>170.1</td>
      <td>2.688</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>False</td>
      <td>...</td>
      <td>(GMT+01:00) Europe/Paris</td>
      <td>53.0</td>
      <td>0.0</td>
      <td>False</td>
      <td>Run</td>
      <td>2.128871e+09</td>
      <td>3600.0</td>
      <td>everyone</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 57 columns</p>
</div>



# Data wrangling


```python
## Helper functions

def s_to_hrs(secs):
    m, s = divmod(secs, 60)
    h, m = divmod(m, 60)
    #d, h = divmod(h, 24)
    return("{} hrs, {} m".format(h,m))

def clean_strava(df = strava_df, cols = cols):

    # limit columns
    mystrava = df.copy()[cols]
    mystrava['start_date'] = pd.to_datetime(mystrava['start_date'], yearfirst = True, infer_datetime_format = True)

    # extract month & year
    mystrava['year'] = pd.DatetimeIndex(mystrava['start_date']).year
    mystrava['month'] = pd.DatetimeIndex(mystrava['start_date']).month

    # convert moving time to integer
    convert_cols = ['moving_time', 'pr_count', 'suffer_score', 'trainer']
    mystrava[convert_cols] = mystrava[convert_cols].fillna(0).astype(int)

    # convert distance to KMs
    mystrava['distance'] = mystrava['distance'].divide(1000)
        
    return mystrava

```


```python
# clean up & filter data
types = ['Ride']
q = "type in @types & year < 2019 & year > 2009"

mystrava = clean_strava().query(q)
```


```python
# sum activities by year
mystrava_by_yr = mystrava.set_index(['id']).groupby(['year','type']).sum()
```


```python
# create moving_hrs
mystrava_by_yr['moving_hrs'] = mystrava_by_yr['moving_time'].apply(lambda x: s_to_hrs(x))
```


```python
## plot function
types_str = ", ".join(types)

def act_bar(df = mystrava_by_yr, metric = 'moving_time', unit = 'seconds', legend = False):
    ax = df.reset_index().query(q).set_index('year')[['type', metric]]\
    .pivot(columns = 'type', values = metric)\
    .plot(kind='barh', legend = legend,
          title = "{} {}".format(str.title(types_str), metric))
    ax.set_ylabel("")
    ax.set_xlabel(unit);
    
```

# Summary plots


```python
# how much time did I spend on the bike?
act_bar()
```


![png](/images/strava_data_files/strava_data_14_0.png)



```python
# how much is that in hours & minutes?
mystrava_by_yr.query(q)['moving_hrs']
```




    year  type
    2014  Ride    103 hrs, 50 m
    2015  Ride    146 hrs, 29 m
    2016  Ride     69 hrs, 55 m
    2017  Ride    167 hrs, 32 m
    2018  Ride    319 hrs, 35 m
    Name: moving_hrs, dtype: object




```python
# how many rides? 
mystrava.query(q).groupby(['year']).count()['id'].sort_index(ascending = False)
```




    year
    2018    133
    2017     80
    2016     43
    2015     72
    2014     35
    Name: id, dtype: int64




```python
# how many of those were on the trainer?
mystrava.query(q).groupby(['year', 'trainer']).count()['id'].unstack()\
    .plot(kind = 'barh', 
          title = "{} count".format(types_str));
```


![png](/images/strava_data_files/strava_data_17_0.png)



```python
# how far did I go?
act_bar(metric = "distance", unit = 'Kilometers')
```


![png](/images/strava_data_files/strava_data_18_0.png)



```python
# how high did I climb?
act_bar(metric = 'total_elevation_gain', unit = "Meters")
```


![png](/images/strava_data_files/strava_data_19_0.png)

