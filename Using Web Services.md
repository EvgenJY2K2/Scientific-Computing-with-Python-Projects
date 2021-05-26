## Using Web Services
###### Task 1: The program will prompt for a URL, read the XML data from that URL using urllib and then parse and extract the comment counts from the XML data, compute the sum of the numbers in the file.
```
import urllib.request, urllib.parse, urllib.error
import xml.etree.ElementTree as ET

sum = 0

while True:
    url = input('Enter location: ')
    if len(url) < 1: break
    print('Retrieving', url)

    info = urllib.request.urlopen(url).read()
    print('Retrieved', len(info), 'characters')

    tree = ET.fromstring(info)

    counts =  tree.findall('.//count')
    print('Count: ', len(counts))

    for count in counts:
         sum = sum + int(count.text)

    print('Sum: ', sum )
```
Or without using url:
```
input = '''data input here'''
import xml.etree.ElementTree as ET
sum = 0

print('Retrieving')
print('Retrieved', len(input), 'characters')

tree = ET.fromstring(input)
counts =  tree.findall('.//count')
print('Count: ', len(counts))

for count in counts:
    sum = sum + int(count.text)
print('Sum: ', sum )
```
###### Task 2: The program will prompt for a URL, read the JSON data from that URL using urllib and then parse and extract the comment counts from the JSON data, compute the sum of the numbers in the file.
```
import json
import urllib.request

sum =0 

url = input('Enter location: ')

info =  json.loads(urllib.request.urlopen(url).read())

print('Retrieving', url)
print('Retrieved', len(info), 'characters')

print('Count:', len(info))

for item in info["comments"]:
    sum = sum + int(item['count'])
print('Sum: ', sum )
```
###### Task 3: The program will prompt for a location, contact a web service and retrieve JSON for the web service and parse that data, and retrieve the first place_id from the JSON. A place ID is a textual identifier that uniquely identifies a place as within Google Maps.
```
import urllib.error, urllib.request, urllib.parse
import json
serviceurl = 'http://py4e-data.dr-chuck.net/json?'

location = input('Enter location: ')

url = serviceurl + urllib.parse.urlencode({'address': location, 'key' : 42})
print('Retriving', url)

info = urllib.request.urlopen(url).read()
print('Retrived', len(info), 'characters')

js = json.loads(info)
print('Place id', js['results'][0]['place_id'])
```
