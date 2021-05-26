## Loops and Iterations
###### Task: Write a program that repeatedly prompts a user for integer numbers until the user enters 'done'. Once 'done' is entered, print out the largest and smallest of the numbers.
```
largest = None
smallest = None
while True:
    try:
        num = input("Enter a number: ")
        if num == "done" : 
            break
        n=int(num)
        if largest is None or n > largest:
            largest = n
        elif smallest is None or n < smallest:
            smallest = n
    except:
        print('Invalid input')
        
print("Maximum is", largest)
print("Minimum is", smallest)
```

