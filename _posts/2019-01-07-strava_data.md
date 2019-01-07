
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
    #act_num = len(strava_json) #TO FIX, this num is oversstated

    if strava_json != "[]":   #This checks whether we got an empty JSON response and so should end
        #Now we process the JSON
        act_json = json.loads(strava_json)
        #pp.pprint(act_json)
        page_df = pd.DataFrame(act_json)
    
        # reduce number of fields in df
        #page_df = page_df[cols]
        strava_df = strava_df.append(page_df)
        #Set up for next loop
        cum_sum = page_num * 200
        #print "%d out of %d activities printed" % (cum_sum, act_num)
        print("{} activities printed".format(cum_sum))
        page_num += 1
        strava_json = []
    else:
        end_found = True
```
