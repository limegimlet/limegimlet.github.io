---
#layout: post
title:  'This is my 2nd post of 2019'
classes: wide
date:   2019-01-07
instagram_id:
tags: []

---
# Testing export from jupyter notebook

blah blah blah


```python
# here is some code

#Open the my_file to use
my_file = open(my_acts,'w')

#Loop extracting data.  Remember it comes in pages
end_found = False
page_num = 1

strava_df = pd.DataFrame(columns = cols)

#Main loop - Getting all activities
while (end_found == False):
    #Do a HTTP Get - First form the full URL
    act_url = base_url + str(page_num)
    strava_json = urllib2.urlopen(act_url).read()

    if strava_json != "[]":   #This checks whether we got an empty JSON response and so should end
        
        #Now we process the JSON
        act_json = json.loads(strava_json)
        page_df = pd.DataFrame(act_json)
        strava_df = strava_df.append(page_df)
        
        #Set up for next loop
        cum_sum = page_num * 200
        print("{} activities printed".format(cum_sum))
        page_num += 1
        strava_json = []
    
    else:
        end_found = True
```
