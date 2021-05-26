## Regular Expressions
###### Task: Read the file, look for integers using the re.findall(), looking for a regular expression of '[0-9]+' and then converting the extracted strings to integers and summing up the integers.
```
import re
fh = open('regex_sum_1147550.txt')
sum=0
numbers = re.findall('[0-9]+',fh)
for number in numbers:
    sum = sum + int(number)
print(sum)
```
