---
title: "Shortest Path Algorithm (Part 2)"
date: 2021-01-24T11:10:04-03:00
description: This is the second part of a study on how shortest path algorithms work.
categories: ["CS"]
tags: ["algorithm", "python", "pillow", "study"]
draft: false
---

# Shortest Path Algorithm (Part 2)

In the [first part](https://ttemporin.dev/2020/board_draw/) of this study, we have implemented two classes to draw an empty board.
Now that we have theses classes, we'll change them a little bit just to randomly make a few nodes look like blocked.
Just to remember, this study is divided into three parts.

* 1) [Draw an empty board](https://ttemporin.dev/2020/board_draw/)
* 2) Add obstacles randomly
* 3) Implement the shortest path algorithm

The division intends to make it easier to understand and visualize the algorithm.

## 1 - Imports

Since we'll randomly choose which node to block, we have to import a native library called `random`.


```python
from PIL import Image, ImageDraw
import random
```

## 2 - Classes

To mark the node as blocked, we gonna add a new parameter on the **\_\_init\_\_** method of the *Node* class.

Let's call this parameter ***blocked*** and set the default value as *False*.


```python
class Node:
    """
    Each node on the board
    """
    # size of each node
    SIZE=30
    
    def __init__(self, x, y, blocked=False, fill='#FFFFFF', outline='#000000'):
        # define node position
        self.x = x
        self.y = y
        
        # define status
        self.blocked = blocked

        # define node colors
        self.fill = fill
        self.outline = outline
        
    def draw(self, base):
        # define the start of the image
        left = self.x * self.SIZE
        top = self.y * self.SIZE
        
        # define the end of the image
        right = left + self.SIZE
        bottom = top + self.SIZE
        
        # shape
        shape = [(left, top), (right, bottom)]

        node = ImageDraw.Draw(base)   
        if self.blocked:
            # create rectangle with grey background
            node.rectangle(shape, fill=(173, 173, 173), outline=self.outline)
            
            # define the angles of the lines
            ltr = (left, top, right, bottom)
            rtl = (right, top, left, bottom)
            
            # add the lines on top of the rectangle
            node.line(ltr, fill=(100, 100, 100), width=1)
            node.line(rtl, fill=(100, 100, 100), width=1)
        else:
            node.rectangle(shape, fill=self.fill, outline=self.outline)
```

Another change we need is inside *draw* method. Let's put an if to check if the node as free or blocked.

Inside the *if*, we create a rectangle with a grey background, and then, using *left*, *top*, *right*, and *bottom* variables define two lines that will be drawn on top of the rectangle as an **X**.

* The **ltr** variable store a tuple with the coordinates from the left/top edge to the right/bottom edge of the node;
* And the **rtl** variable store a tuple with the coordinates from the right/top edge to the left/bottom edge of the node.

After the definition, we pass the variables as a first parameter of the method *line* followed by the grey color *(100, 100, 100)* and the thickness of the line (which is 1px).


In the *Board* class, let's add a new parameter to define the probability of the node being blocked.


```python
class Board:
    """
    Matrix of Nodes
    """
    
    def __init__(self, rows, cols, max_to_block=15):
        # size of the board
        self.rows = rows
        self.cols = cols
        self.max_to_block = max_to_block
        
    def create(self):
        # start empty board
        self.board = [None] * self.rows

        for row in range(0, self.rows):
            self.board[row] = [None] * self.cols
            for col in range(0, self.cols):
                blocked = random.randint(1, 100)
                self.board[row][col] = Node(col, row, blocked < self.max_to_block)
                
    def draw(self):
        w = self.cols * 30
        h = self.rows * 30
        
        # define base image
        img = Image.new("RGB", (w, h), (173, 173, 173))

        for row in range(0, self.rows):
            for col in range(0, self.cols):
                self.board[row][col].draw(img)
        
        # draw final image
        img.show()
```

Inside *create* method, let's call *random.randint(1, 100)* to get a random number from 1 to 100 and store the result on the variable *blocked*.

Now, we can compare the value of *blocked* and *max_to_block* value to randomly create a node as free or blocked.

## 3 - Drawing

After these changes, we just need to create an instance of the *Board* class with the number of rows and cols the board has.


```python
board = Board(18, 18)
```

Then, call the methods *create* and *draw* to see the board.


```python
board.create()
board.draw()
```


```python

```
