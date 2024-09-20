---
layout: post
title:  "Binary Search Algorithm Analysis"
---
#cmsc351 #searchalg 
## Purpose
1. Lets say you have a list, $A$ which is a sorted list of $n$ elements and you have an element $x$ and want to find if it exists within the list (ie, get the index of where the target element is located within the list)
## Binary Search Big Idea
1.  Binary search allows you to find this target element by starting at the center and comparing the center element with the target element and then either checking the center of the left sublist or right sublist.
2. Compare the center element $A[C]$ to $x$ 
3. If $x<A[C]$ , go left
4. If $x > A[C]$, go right
5. If $x=A[C]$, return the $C$
6. For the go left, go right operations, we repeat this process again until either the element $A[C]=x$  or we exhaust the elements.
## Example
```
list A = [2,3,4,5,9,20,56]
target = 56

Step 1 - Find center
there are 7 elements 
let center = floor(7/2) = floor(3.5) = 3
A[3] = 5
does 56=5? no
is 56<5? no
is 56>5? yes
Step 2 - Focus on elements to the right of 5
we now exclusivly focus on the "sublist" [9,20,56]
there are 3 elements
let center  = floor(3/2) = floor(1.5) = 1 
A[1] = 20
is 56 = 20? no
is 56<20? no
is 56>20? yes
Step 3 - Focus on elements to the right of 20
Well we just have 56
does 56 = 56? yes 
return index
```
*This is a very loose run down of how the concept works. The Pseudocode implementation will encompass more the concept in specifics. Remember, the pre req for Binary Search is an already sorted list. Additionally, the "sublist" is an actual sublist, its just a segmented part of the original list. Pseudocode will have it make more sense. *

## Pseudocode
```
function binary_search(lst,target)
	left_end = 0
	right_end = n-1
	while left_end <= right_end
		center = floor((left_end+right_end)/2)
		if lst[center] = target
			return center
		else if target<lst[center]
			right_end = center-1
		else
			left_end = center+1
	return failure
```
1. Lets analyze the above.
2. The function takes in a sorted list, `lst` and a target value, `target`.
3. We create two variables representing the right and left ends of the list
4. We then enter a while loop to begin the operation.
5. The center of the list is assigned by adding the left and right ends together and divide it in half.
6. Next we enter our conditionals and compare the current `center` to the `target`.
	1. Notice that we update the ends accordingly if we do not match with the target.
	2. If we want to analyze the left sublist, we just have to adjust our right bounds to one less than the current center.
	3. If we want to analyze the right sublist, we adjust our left end to start at one plus the center.
	4. This bound update allows us to analyze a sub section of the list without having to create a new one. This make the algorithm in place which allows us to maintain relative indexes to rest of list and allow us to keep track of positions relative to the whole list.
7. Once either the value is found and returned or the `left_end` exceeds `right_end` we return whatever failure we decide to implement
	1. Note that we make the while condition `left_end <= right_end` because no matter how many situations we run where the target is not present, the `left_end` will always end up exceeding the `right_end` bounds.
	2. Lets say we reach the end of the sublists and we are down to one element since the target does not exist.
		1. What can we say is true? `left_end=right_end=center`
		2. We know all three bounds equal each other.
		3. We also know that the target is not present, which leaves us with going left or right
		4. Well, if we go left, we take away from the right  so then right < left and we stop
		5. If we go right, we add to the left and thus right is still less than left and we stop
	3. So we see that after exhausting through the procedure, left will always be bigger than right. That is why we use it as our while loop case
## Code Implementation
```Python
def binary_search(lst,target):
	left_end = 0
	right_end = len(lst) - 1
while left_end<=right_end:
	center = (left_end + right_end) // 2 #// is int division, casts float to int
	if target == lst[center]:
		return center
	elif target<lst[center]:
		right_end = center - 1
	else:
		left_end = center + 1
return -1
```
*Note: Python Implementation*
## Time Complexity
### Best Case
1. Well, best case would be if the center element of the list also is the target element so thus we would only run the operation once. Lets look at the pseudocode once again to see the how we could calculate this.
```
function binary_search(lst,target)
	left_end = 0                   
	right_end = n-1
	while left_end <= right_end
		center = floor((left_end+right_end)/2)
		if lst[center] = target
			return center
		else if target<lst[center]
			right_end = center-1
		else
			left_end = center+1
	return failure
```
2. We can group everything up to (exclusive) the while loop as constant $c_{1}$ because these operations run in constant time.
3. Now for the while loop we know that it is going to run once since it will hit the first conditional and return so we can just say it is some constant $c_{2}$.
4. We do not need to consider anything else beyond this conditional because the code terminates.
5. Lets put it together:
$$T(n) = c_{1} + c_{2} = 𝛩({1})$$
*We have purely constants to consider thus the best case must will run in theta of 1*
### Worst Case
1. Worst case would be having a target element that does not exist within the list.
2. In this case we completely exhaust through the process
3. So lets look at it once again...
4. We still have our constant time before the loop.
5. Our loop does not run $n$ times. Remember what we are doing to the amount of information we are processing as we iterate. 
6. We start with $n$ elements.
7. Then we either **split** left or right. What happens to the size of elements we are processing?
8. Well, the split essentially halves the information we are processing. 
9. So we get to $\frac{n}{2}$ elements.
10. Then we split again. 
11. We get $\frac{1}{2}\div{2}={\frac{1}{4}}$  
12. So we have $\frac{n}{4}$ elements
13. If we do this over and over again, we can see that this pattern can be generalized to $\frac{n}{2^i}$ 
14. For every iteration $i$, we halve the size* of the list. I say size* because we aren't actually changing the size of the list.
15. With this information, we can analyze this to be the following function:
$$\frac{n}{2^i} = 1$$
$$n = 2^i$$
$$lg(n) = i$$
*Note: we set the top expression equal to one because in worst case, we end with size of 1.*
16. So, we do some algebra and we are able to show that we can relate the number of iterations $i$ to the function, $lg(n)$. 
17. So now that we've determined a function to represent the runtime of the loop, lets put it together:
$$T(n) = c_{1}+c_{2}\log_{2}(n)+1 + c_{3} = 𝛩(\lg(n))$$
*Note: $c_{3}$ represents the constant running for the failure at the end since our worst case includes this situation. The plus 1 stems from the additional iteration after $L=R$* 
 
### Average Case
1. Average case is also $𝛩(lg(n))$
