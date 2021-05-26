## Lists
###### Task 1: For each word on each line check to see if the word is already in the list and if not append it to the list. When the program completes, sort and print the resulting words in alphabetical order.
```
fname = input('Enter file name: ')
fh = open(fname)
lst = list()
for line in fh:
    words = line.split()
    for word in words:
        if word not in lst:
            lst.append(word)
print(sorted(lst))
```
###### Task 2: You will parse the file from line to line using split() and print out the second word in the line (i.e. the entire address of the person who sent the message). Then print out a count at the end.
```
fname = input('Enter file name: ')
fh = open(fname)
lst = list()
for line in fh:
    words = line.split()
    for word in words:
        if word not in lst:
            lst.append(word)
print(sorted(lst))
```
