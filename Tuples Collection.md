## Tuples Collection
###### Task 1: Write a program to read through the mbox-short.txt and figure out the distribution by hour of the day for each of the messages. You can pull the hour out from the 'From ' line by finding the time and then splitting the string a second time using a colon.
```
name = input("Enter file:")
if len(name) < 1 : name = "mbox-short.txt"
fh = open(name)
counts = dict()

for line in fh:
    if line.startswith('From ') :
        words = line.split()
        time = words[5]
        hours = time[:2]
        counts[hours] = counts.get(hours,0) + 1

for key, val in sorted(counts.items()):
    print (key, val)
```
