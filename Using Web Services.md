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

