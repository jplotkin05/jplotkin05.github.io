---

layout: post

title: Solving the Island Programming Problem

subtitle: Walkthrough

date: 2026-08-20 19:00

tags:
- general
- algorithms
---

## Overview
*Official problem overview from leetcode:* 
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
**Output:** 1

**Example 2:**

**Input:** grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
**Output:** 3

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## Objective
The stated objective is to find all the islands on the map which are denoted as all horizontally and vertically connected 1s. This is an important constraint and eliminates the need to inspect diagonal similarities. 

There are two mechanisms to this problem that will help achieve this task.

Row major traversal:
At the core, we need to iterate over every cell until we hit a a land block. 

DFS or BFS (Some sort search):
Once we land on an undiscovered land mass, we can use BFS or DFS to search around the mass for additional adjacent land masses. 

These two mechanisms will allow us to discover and track concentrated masses in the matrix.

## The Build
*This solution was developed with python and assumes all compilations are with python3*

First lets ensure we have the example grid defined (for this setup, I will use example 2 from the problem description since it has a more complex layout):
```Python
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```

Next, lets define a function that will handle this problem:
```Python
def find_islands(grid):
	visited_cells = set()
	island_count = 0 
	
	
	def explore_island(location):
	return
	
	for r in range(len(grid)):
		for c in range(len(grid[0])):
			if grid[r][c] == 1 and (r,c) not in visited:
				island_count+=1
				explore_island((r,c))
			
	
	return island_count
```

Ok so this is base of the first mechanism which is traversing in row major order over the grid. If we stumble on a land mass (1) and it's not been visited already, we know we've hit a new island and can increment our tracker. Now we can call our DFS function `explore_island` and we will tag the surrounding masses that are attached. Notice that we use a set to store visited cells since we are handling distinct values. Also compacting the two dimensions into a tuple helps bundle it appropriately. 

Now we need to implement the DFS search in the `explore_island` function.
This is where we need to outline some constraints. From the current point we are located at, we need to explore what is directly above, below, left, and right of the current cell if the position is in bounds. 


Lets define those boundaries:
```Python
def find_islands(grid):
	visited_cells = set()
	island_count = 0 
	
	
	def explore_island(location):
		upper_bounds = -1 
		lower_bounds = len(grid)
		left_bounds = -1
		right_bounds = len(grid[0])
		
	return
	
	for r in range(len(grid)):
		for c in range(len(grid[0])):
			if grid[r][c] == 1 and (r,c) not in visited:
				island_count+=1
				explore_island((r,c))
			
	
	return island_count
```

Now that we have the boundaries, we can go ahead and implement the rest of the search:
```Python
def find_islands(grid):
	visited_cells = set()
	island_count = 0 
	
	
	def explore_island(location):
		upper_bounds = 0 
		lower_bounds = len(grid)-1
		left_bounds = 0
		right_bounds = len(grid[0])-1
		
		stack = []
		stack.append(location) #adds to end of list
		
		while stack:
			curr = stack.pop() #removes from end of list 
			r,c = curr # extrapolating the index values
			
			if int(grid[r][c]) == 1 and curr not in visited_cells:
				visited_cells.add((r,c))
				
				#search below
				if r < lower_bounds:
					stack.append((r+1,c))
				#search above
				if r > upper_bounds:
					stack.append((r-1,c))
				#search left
				if c > left_bounds:
					stack.append((r,c-1))
				# search right
				if c < right_bounds:
					stack.append((r,c+1))
	
	for r in range(len(grid)):
		for c in range(len(grid[0])):
			if int(grid[r][c]) == 1 and (r,c) not in visited_cells:
				island_count+=1
				explore_island((r,c))
			
	
	return island_count
```

Now the full solution is implemented. Once we hit a land mass, we mark it as visited and then add to the stack all valid/available spaces to be explored and marked as visited. That way every time we land on a 1 during the row major traversal, its guaranteed that its part of a new distinct land mass and we can count it as a distinct island in our tracker. 

This solution runs in approximately $O(r*c)$ or $O(n^2)$ time since we are traversing over every value of an $r$ x $c$ matrix. 

More on other performance metrics later...

