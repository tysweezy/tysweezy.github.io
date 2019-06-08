---
layout: post
title:  "Creating a Lunch Group Slack Bot: Working with Some Data"
date:   2019-06-07  15:02:17 -0700
categories: software engineering python
author: Tyler Souza
share: true
---

A lot of times at the office around lunch time, one of us will ask everyone what their lunch plans are via Slack. This leads to one or a couple of us to take in orders and go pick up lunch a group of co-workers.  I’ve been interested in building bots, particularly Slack bots lately, and I felt this would be a perfect case to build one for this task. 


## Need Some Data to Work With
I’ve built a Slack bot before using Go, so know how the Slack API mostly works. However, before I get to the Slack API, I want to have some Geo data to work with. I could just have the “lunch organizer” create a list of places nearby the office and store them in a DB, but that’s boring. ;) 

I want to actually detect places nearby the office when a lunch organizer actually searches for a place nearby the office. Perhaps a new place opened up recently and I would want that to be an option. Maybe this data could be useful for another idea, such as: Calculating distance from the office to location, how many times we’ve ordered from a particular location in a month, tracking approximate order completion time, etc. 

How will I actually get this data from? Well, I thought of the Google Maps API, however, I didn’t really want to deal with their API. I searched around for other third-party services and found [MapBox](https://www.mapbox.com/) (totally not affiliated with them in any way, I just think their API is awesome to work with). They do have a Python API client, so I decided to play around with that. 

After some thought, I really don’t want to keep hitting the API with requests constantly, since I’m pretty sure it is rate limited. So I thought of writing a Python script that will make request(s) via a CLI command, then output that data into a JSON file. 

## Description of the Python Script
Now let’s get out hands dirty! First in my `geo.py` script, I want to define a variable that contains our current project path in addition with a `/tmp` directory. This is where our outputted JSON files will live.  I also want to get the instance of the Geocoder class that the [MapBox Python API](https://github.com/mapbox/mapbox-sdk-py) client provides.

```python
from mapbox import Geocoder

 dir_path = os.path.dirname(os.path.realpath(__file__)) + '/tmp'
 
 geocoder = Geocoder(access_token=os.getenv('MAPBOX_TOKEN'))
```

Next, I want to able to query the [Search API](https://docs.mapbox.com/api/search/) based on a list of search terms. I’m thinking of loading a `csv` file that contains a list of terms such as tacos, Taco Bell, greek food, etc. that is separated by a comma delimiter.

This can be it’s own function called `load_terms()`

```python
# ... other imports 
import csv

 def load_terms():
     """
     grab terms from a csv file and return data
     """
     terms = []
 
     with open('terms.csv', newline='') as csvfile:
         termreader = csv.reader(csvfile, delimiter=',')
         for term in termreader:
             terms.append(term)
 
     return terms
```

Here’s an example  `terms.csv`  file:

```csv
 taco bell,fast food,mcdonalds,greek,greek food,tacos,mexican,mexican food
```

I want a separate function that will actually make a request to the API and return JSON output.

```python
 def query_geocoder(q):
     query = f'{q} Fresno, CA 93727'
     response = geocoder.forward(query, types=['poi', 'postcode'], limit=10)
 
     return json.dumps(response.geojson())
```

This grabs a `q` parameter which represents a query/term. Then we are making use of [Python 3’s f-Strings](https://realpython.com/python-f-strings/) in the query variable which we are adding it to our office city and postal code. Then we are taking our query and passing it to `gecoder.forward()` which will make a request to the MapBox search API. Then we are returning the JSON result.

Now on to the function that generates the geocode data and outputs it into JSON files. 

```python
 def generate_geodata():
     terms = load_terms()
 
     # make directory for data files to live per generate task
     now = datetime.now()
     os.mkdir(dir_path + '/' + now.strftime('%m_%d_%Y_%s'))
     dest = dir_path + '/' + now.strftime('%m_%d_%Y_%s')
 
     for term in terms[0]:
         filename = str(time.time()) + '_' +  str(term) + '_geodata.json'
         with open(os.path.join(dest, filename), 'w') as f:
             f.write(query_geocoder(term))
             f.close()
```

First, I’m calling the `load_terms` function to a get a list of our terms.  I wanted to make a directory within the `tmp` directory that is generated when the command is called. This will be based on the current date and unix timestamp. Then we loop through the terms list and create a new JSON file for each term. Then, I called the `query_geocoder` function within the loop which writes to that file. 

As I mentioned above, I want the data to be generated via a command, so I used [Python’s argparse](https://docs.python.org/3/library/argparse.html) to start creating a CLI. 

```python
 if __name__ == '__main__':
     # geo CLI
     parser = argparse.ArgumentParser(description="CLI for generating geo data")
     parser.add_argument('-g', '--generate', action='store_true', help='generates geo data')
     args = parser.parse_args()
 
     if args.generate:
         print('data is generating... \n')
         generate_geodata()
         print('Done!')
 
     if len(sys.argv) == 1:
         parser.print_help()
         sys.exit()
````

When we use the `python geo.py -g`  command (or `--generate`), it will query the API and generate JSON files in a new directory within our `/tmp` directory.

## What To Do From Here?
This is a very easy, quick and dirty script at them moment. However, I’m thinking of expanding the program to parse and store the geo data into a database. Perhaps we can detect if there’s a new location nearby and store it in the DB. We could use a scheduler or a cron job that runs the command and check weekly, monthly, etc. 

Once that’s done, we can create an admin that lists nearby locations to choose from, store recent locations, play around with the geo data with a tool such as [PostGIS](https://postgis.net/), etc. After, we should have a foundation to start on Slack related things. 

In the next couple articles, I will go over what I’ve built based on the tasks above. Stay tuned!