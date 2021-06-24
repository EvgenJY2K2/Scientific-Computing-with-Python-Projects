## Certification projects - Arithmetic Formatter
**About the project:**

It is a function that receives a list of strings that are arithmetic problems and returns the problems arranged vertically and side-by-side. The function optionally takes a second argument. When the second argument is set to True, the answers are displayed.


```
def arithmetic_arranger(problems, *solutions):
    
    #Limit to the number of problems is five.
    if len(problems) > 5:
        return "Error: Too many problems."

    #Make a list to append the answers to.
    arranged_problems = []

    for index, value in enumerate(problems):
        #Split each problem into three components ["32", "+", "8"].
        operation = value.split(" ")

        #The function accepts only addition and subtraction.
        if operation[1] not in "-+":
            return "Error: Operator must be '+' or '-'."

        #Each operand has a max of four digits in width.
        if len(operation[0]) > 4 or len(operation[2]) > 4:
           return "Error: Numbers cannot be more than four digits."

        #Each number should only contain digits.
        try:
            int(operation[0])
            int(operation[2])
        except:
          return "Error: Numbers must only contain digits."

       
        #find max length of lines in problems (also fit space and operator).
        max_len = max(len(operation[0]), len(operation[2]))
        width = max_len + 2
        #print out the numbers line by line with formatting. 
        l1 = f"{operation[0]:>{width}}"
        l2 = operation[1] + f"{operation[2]:>{width-1}}"
        ds = '-' * width

        #Add on next problems.
        try:
          arranged_problems[0] += (' ' * 4) + l1
        except:
          arranged_problems.append(l1)

        try:
          arranged_problems[1] += (' ' * 4) + l2
        except:
          arranged_problems.append(l2)

        try:
          arranged_problems[2] += (' ' * 4) + ds
        except:
          arranged_problems.append(ds)
        
        #If the second argument is given calculate solutions. 
        if solutions:
          
          if operation[1] == '+':
           a = int(operation[0]) + int(operation[2])
          else:
           a = int(operation[0]) - int(operation[2])
          
          #Format solutions accordingly. 
          ans = f"{str(a):>{width}}"
          
          #Append solutions to the printed problems. 
          try:
            arranged_problems[3] += (' ' * 4) + ans
          except:
            arranged_problems.append(ans)


    #Print problems with corresponding formatting on new line each.
    output = f"{arranged_problems[0]}\n{arranged_problems[1]}\n{arranged_problems[2]}"

    #Add on the solutions if needed. 
    output = output + f"\n{arranged_problems[3]}" if solutions else output
    
    return output
```
You can find more info about this challenge on CodeCamp: https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/arithmetic-formatter
