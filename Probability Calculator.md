## Certification projects - Probability Calculator
**About the project:**

This program determines the approximate probability of drawing certain balls randomly from a hat.

```
import copy
import random

class Hat:

  def __init__(self, **kwargs):
    self.contents = list()
    
    for key, value in kwargs.items():
        for i in range(value):
            self.contents.append(key)
    return None

  def draw(self, num):
    self.draw = list()

    if num > len(self.contents):
      return self.contents

    for i in range(num):
      choice = random.choice(self.contents)
      self.contents.remove(choice)
      self.draw.append(choice)
    return self.draw
    


def experiment(hat, expected_balls, num_balls_drawn, num_experiments):
  M = 0
  for i in range(num_experiments):
    new_hat = copy.deepcopy(hat)
    balls_drawn = new_hat.draw(num_balls_drawn)
    balls_req = sum([1 for k, v in expected_balls.items() if balls_drawn.count(k) >= v])
    if balls_req == len(expected_balls):
      M+=1
    else:
      continue

  return M/num_experiments
```
You can find more info about this challenge on CodeCamp: https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/probability-calculator
