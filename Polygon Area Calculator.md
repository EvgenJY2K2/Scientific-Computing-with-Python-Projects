## Certification projects - Polygon Area Calculator
**About the project:**

This project uses object oriented programming to create a Rectangle class and a Square class. The Square class is a subclass of Rectangle and inherit its methods and attributes.

```
class Rectangle:
  
  def __init__(self, width, height):
    self.width = width
    self.height = height

  def __str__(self):
    return f"Rectangle(width={self.width}, height={self.height})"

  def set_width(self, width):
    self.width = width

  def set_height(self, height):
    self.height = height

  def get_area(self):
    area = self.width * self.height
    return area

  def get_perimeter(self):
    perimeter = 2 * self.width + 2 * self.height
    return perimeter

  def get_diagonal(self):
    diagonal = (self.width ** 2 + self.height ** 2) ** 0.5
    return diagonal

  def get_picture(self):
    if self.height > 50 or self.width > 50:
      return 'Too big for picture.'

    else:
      pic = "*" * self.width + '\n'
      pic = pic * self.height
      return pic

  def get_amount_inside(self, object):
    object = object.get_area()
    self = self.get_area()

    return self//object

class Square(Rectangle):

  def __init__(self, side):
    super().__init__(side, side)

  def __str__(self):
    return f"Square(side={self.width})"
  
  def set_side(self, side):
    self.width = side
    self.height = side
  
```
You can find more info about this challenge on CodeCamp: https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/polygon-area-calculator
