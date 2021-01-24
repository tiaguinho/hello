---
title: "Short Path Algorithm (Part 1)"
date: 2020-08-03T08:56:04-03:00
description: This is the first part of a study on how short path algorithms work.
categories: ["CS"]
tags: ["algorithm", "python", "pillow", "study"]
draft: false
---

# Short Path Algorithm (Part 1)

This is the first part of a study on how short path algorithms work.
This study is divided into three parts.

* 1) Draw an empty board
* 2) Add obstacles randomly
* 3) Implement the short path algorithm

The division intends to make it easier to understand and visualize the algorithm.

## 1 - Dependencies

Before we start writing code, we need to install the python library Pillow.

```bash
pip install Pillow
```

## 2 - Imports

To draw the board, we need to import `Image` and `ImageDraw` classes from the Pillow library.


```python
from PIL import Image, ImageDraw
```

***PIL is short for Python Image Library***

## 3 - Classes

Now that we have our dependencies installed, let's start defining the *Node* class.

The *Node* class will hold each position of the board.


```python
class Node:
    """
    Each node on the board
    """
    # size of each node
    SIZE=30
    
    def __init__(self, x, y, fill='#FFFFFF', outline='#000000'):
        # define node position
        self.x = x
        self.y = y

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
        node.rectangle(shape, fill=self.fill, outline=self.outline) 
```

Inside the *draw* method, we use the **ImageDraw** class to create the image of the node.

The most important part is the first parameter of the ***rectangle*** method. This parameter is responsible for the positions of the pixels that the class will use to draw our node.

The *shape* is an array with two positions. In the first position, we define where is the top/left edge of the rectangle and in the second, the bottom/right edge.

With these definitions, the method will draw:

* One vertical line from the left/top edge to the **relative** left/bottom edge;
* One vertical line from the right/bottom edge to the **relative** right/top edge;
* One horizontal line from the left/top edge to the **relative** right/top edge;
* One horizontal line from the right/bottom edge to the **relative** left/bottom edge.

In the end we have a rectangle.

Now we need to create the *Board* class, to hold the matrix of *Nodes* and draw the final image.


```python
class Board:
    """
    Matrix of Nodes
    """
    
    def __init__(self, rows, cols):
        # size of the board
        self.rows = rows
        self.cols = cols
        
    def create(self):
        # start empty board
        self.board = [None] * self.rows

        for row in range(0, self.rows):
            self.board[row] = [None] * self.cols
            for col in range(0, self.cols):
                self.board[row][col] = Node(col, row)
                
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

## 4 - Drawing

With the classes defined, now everything we need to do is create an instance of the *Board* class with the number of rows and cols the board has.


```python
board = Board(9, 9)
```

Then, we just need to call the methods *create* and *draw*.


```python
board.create()
board.draw()
```
