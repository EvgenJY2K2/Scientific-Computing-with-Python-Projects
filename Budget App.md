## Certification projects - Budget App
**About the project:**

The Category class in budget.py is able to instantiate objects based on different budget categories like food, clothing, and entertainment. When objects are created, they are passed in the name of the category. The class has an instance variable called ledger that is a list, it also contains several methods. Besides the Category class, there is a function (outside of the class) called create_spend_chart that takes a list of categories as an argument and returns a string that is a bar chart.


```
def truncate(n):
  multiplier = 10
  return int(n*multiplier)/multiplier

def getTotals(categories):
  total = 0
  breakdown = []
  for category in categories:
    total += category.get_withdrawals()
    breakdown.append(category.get_withdrawals())
  rounded = list(map(lambda x: truncate(x/total), breakdown))
  return rounded

def create_spend_chart(categories):
  res = "Percentage spent by category\n"
  i = 100
  totals = getTotals(categories)
  while i >= 0:
    cat_spaces = " "
    for total in totals:
      if total * 100 >= i:
        cat_spaces += "o  "
      else:
        cat_spaces += "   "
    res += str(i).rjust(3) + "|" + cat_spaces + ("\n")
    i -= 10

  dashes = "-" + "---"*len(categories)
  names = []
  x_axis = ""
  for category in categories:
    names.append(category.name)

  maxi = max(names, key=len)

  for x in range(len(maxi)):
    nameStr = '     '
    for name in names:
      if x >= len(name):
        nameStr += "   "
      else:
        nameStr += name[x] + "  "

    if (x != len(maxi) - 1):
      nameStr += '\n'

    x_axis += nameStr

  res += dashes.rjust(len(dashes)+4) + "\n" + x_axis
  return res

class Category:
  def __init__(self, name):
    self.name = name
    self.ledger = list()

  def __str__(self):
    title = f"{self.name:*^30}\n"
    items = ""
    total = 0

    for item in self.ledger:
      items += f"{item['description'][0:23]:23}" + f"{item['amount']:>7.2f}" + '\n'
      
      total += item['amount']
    output = title + items + "Total: " + str(total)
    return output 

  def deposit(self, amount, description =""):
    self.ledger.append({"amount": amount, "description": description})
  
  def withdraw(self, amount, description =""):
    if (self.check_funds(amount)):
      self.ledger.append({"amount": -amount, "description": description})
      return True
    return False
  
  def get_balance(self):
    total_cash = 0 
    for item in self.ledger:
      total_cash += item["amount"]
    return total_cash
  
  def transfer(self, amount, category):
    if (self.check_funds(amount)):
      self.withdraw(amount, "Transfer to " + category.name)
      category.deposit(amount, "Transfer from " + self.name)
      return True
    return False
  
  def check_funds(self, amount):
    if (self.get_balance() >= amount):
      return True
    return False

  def get_withdrawals(self):
    total = 0 
    for item in self.ledger:
      if item["amount"] < 0:
        total += item["amount"]
    return total
  
```
You can find more info about this challenge on CodeCamp: https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/budget-app
