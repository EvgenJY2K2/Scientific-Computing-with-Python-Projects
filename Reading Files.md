## Reading Files
###### Task 1: Write a program to read through a file and print the contents of the file (line by line) all in upper case.
```
fname = input("Enter file name: ")
try:
    fh = open(fname)
except:
    print('File cannot be opened:', fname)
    quit()
for line in fh:
    line = line.rstrip()
    print(line.upper())
```
###### Task 2: Write a program that prompts for a file name, then opens that file and reads through the file, looking for lines of the form: "X-DSPAM-Confidence: 0.8475". Count these lines and extract the floating point values from each of the lines and compute the average of those values. Do not use the sum() function or a variable named sum in your solution.
```
fname = input("Enter file name: ")
try:
    fh = open(fname)
except:
    print('File cannot be opened:', fname)
    quit()
count = 0
total = 0
for line in fh:
    if line.startswith('X-DSPAM-Confidence:'):
        count = count + 1
        num = float(line[line.find(' '):].lstrip())
        total = total + num
        average = total/count
print('Average spam confidence:', average)
```
