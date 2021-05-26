## Conditional Execution
###### Task 1: Write a program to prompt the user for hours and rate per hour using input to compute gross pay. <br> Pay the hourly rate for the hours up to 40 and 1.5 times the hourly rate for all hours worked above 40 hours.
```
hrs = input("Enter Hours:")
rt = input("Enter Rate:")
try:
    h = float(hrs)
    r = float(rt)
except:
    print ('Error. Enter a numeric input')
    
if h <= 40:
    print (h*r)
else:
    print ((h-40)*1.5*r + 40*r)
```
###### Task 2: Write a program to prompt for a score between 0.0 and 1.0. If the score is out of range, print an error. If the score is between 0.0 and 1.0, print a grade.
```
score = input("Enter Score: ")
try:
    score = float(score)
except:
    print ('Please enter a numeric input.')
    quit()
if score <=0.0 or score >=1.0:
    print ('Out of range.')
    quit()
if score >= 0.9:
        print ('A')
elif score >= 0.8:
        print ('B')
elif score >= 0.7:
        print ('C')
elif score >= 0.6:
        print ('D')
elif score < 0.6:
        print ('F')
else:
    print ('Error')
    quit()
```
