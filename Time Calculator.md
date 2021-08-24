## Certification projects - Time Calculator
**About the project:**

It is a function that takes in two required parameters (starting time, duration time) and one optional parameter (a starting day of the week). The function adds the duration time to the start time and returns the result. If the function is given the optional starting day of the week parameter, the output displays the day of the week of the result. 


```
def add_time(start, duration, *day):

  #START (example entry:"11:30 AM")

    #split first entry ["11:30", "AM"]
    start = start.split(" ")

    #split entry "11:30" to hours and minutes
    daytime = start[0].split(":")

    #assign variables to hours and minutes ['11', '30']
    st_h= daytime[0]
    st_min= daytime[1]

    #get AM or PM
    daypart = start[1]

  #DURATION (example entry: "2:32")
    
    #split second entry to hours and minutes
    duration = duration.split(":")

    #assign variables to duration (hours and minutes) ['2', '32']
    h = duration[0]
    min = duration[1]
    
  #DAY
  
    #conver tulip into string
    day = ''.join(day)
    
    #make the word start with capital
    day = day.title()
   
  
  #CALCULATION
  
    #TIME & DAYS
    
    #calculate the final hours and minutes
    f_h = int(st_h) + int(h)
    f_min = int(st_min) + int(min)
    
    #variables for daypart changes
    count = 0
    change = 0

    #simplify minutes
    if f_min > 60:
        
        #check for extreme minutes imput
        if daypart == "PM":
            daypart = "AM"
            change = 1
            
        f_h += f_min//60
        f_min = f_min % 60
    
    #format minutes for output
    f_min = '{:0>2}'.format(f_min) 
        

    #check for extreme hours imput
    if f_h == 12:
        if daypart == "AM":
            daypart = "PM"

        else:
            daypart = "AM"
        
    #simplify hours imput and count daypart changes
    while f_h > 12:
        f_h = f_h - 12
        count = count + 1
        
    #for each daypart count - change am and pm values
    for i in range(count):
        if daypart == "AM":
            daypart = "PM"
        
        #check for day change (from pm to am)
        else:
            daypart = "AM"
            change = change + 1
        
            
    if change == 1:
        n = "next day"

    if change > 1:
        n = f"{change} days later"
    
    #WEEK DAYS
    
    #create list with days of the week
    week = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
    
    if day:
        
        #capitalise the word
        day = day.title()
        
        #search for this day in week list and get index
        if any(word in day for word in week):
            index_d = week.index(day)
            
            #calculate the final index of the day
            index_f = index_d + change
            
            #find index for extreme durations
            if index_f > 6:
                
                #check for every index in list (mod = 0) 
                for i in week:
                    index_f = index_f % index_d == 0
                    index_f = index_f - 1
                
            #find the day with the correlating index
            day = week[index_f]
    

  #OUTPUT
  
    #output with time
    new_time = f"{f_h}:{f_min} {daypart}"
    
    #output with days count
    new_time = new_time + f" ({n})" if change >= 1 else new_time
    
    #output with days of the week if same day
    new_time = f"{f_h}:{f_min} {daypart}, {day}" if day and change == 0 else new_time
    
    #output with days of the week if other day
    new_time = f"{f_h}:{f_min} {daypart}, {day} ({n})" if day and change >= 1 else new_time


    return new_time
```
You can find more info about this challenge on CodeCamp: https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/time-calculator
