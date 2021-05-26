## Functions
###### Task: Write a program to prompt the user for hours and rate per hour using input to compute gross pay. Pay should be the normal rate for hours up to 40 and time-and-a-half for the hourly rate for all hours worked above 40 hours.
```
def computepay(h,r):
    if h <= 40:
        return h*r
    else:
        return (h-40)*0.5*r+h*r
 
hrs = input("Enter Hours:")
rt = input("Enter Rate:")

try:
    h = float(hrs)
    r = float(rt)
except:
    print ('Please enter a numeric input.')
    quit()

p = computepay(h,r)
print("Pay",p)
```
