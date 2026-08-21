---

layout: post

title: Solving the Log Parsing Problem

subtitle: Walkthrough

date: 2026-08-21 19:46

tags:
- general
- algorithms
---

## Overview
You're given a list of log lines. Each line is: `timestamp LEVEL message`. Count how many times each `LEVEL` appears, and return the level with the most occurrences.


```Python
logs = [
    "1000 INFO started",
    "1001 ERROR disk full",
    "1002 WARN high memory",
    "1003 ERROR timeout",
    "1004 INFO ok",
    "1005 ERROR crash",
]
#Expected: "ERROR" (appears 3 times)
```

## Strategy
There are four main mechanisms to this problem
1. We need to iterate over each log entry
2. Extrapolate the level from the string
3. Track occurrences of each level
4. Sort to return most frequent level

Iterating will be a simple for loop over the list.

In terms of extrapolating the level value, we can utilize string slicing. Notice that every log starts with a four digit number followed by a space. That's five characters we can skip each time which means each level entry starts at string index 5. As for when it ends, if we utilize the `.index()` function, for the first `" "` in the slice, we can simply use that as the upper bounds for our already sliced segment to get us just the level. Observe below: 

Code:
```Python
logs = [

"1000 INFO started",

"1001 ERROR disk full",

"1002 WARN high memory",

"1003 ERROR timeout",

"1004 INFO ok",

"1005 ERROR crash",

]
log_entry = logs[0]

slice = log_entry[5:]

space = slice.index(" ")

print(slice[:space])
```

Output:
```Terminal
INFO
```

We've now squared away our level extrapolation. In terms of frequency tracking, a hash map will suffice. We simply iterate over each level. If it's not a key in the hash map, create it with a value pair of 1. Otherwise, access the key and increment by 1. 

Code: 
```Python
logs = [

"1000 INFO started",

"1001 ERROR disk full",

"1002 WARN high memory",

"1003 ERROR timeout",

"1004 INFO ok",

"1005 ERROR crash",

]

level_count = {}

for log in logs:

	slice = log[5:]

	space = slice.index(" ")

	level = slice[:space]

	if level in level_count:

		level_count[level]+=1

	else:
		level_count[level] = 1
		
print(level_count)
```

Output:
```Terminal
{'INFO': 2, 'ERROR': 3, 'WARN': 1}
```

Now we have the count. So now we need to extrapolate the key with the largest value. A naive approach would be to iterate over the dictionary and make some comparisons but we can take advantage of python's own sorting function, `sorted()`

The function accepts three parameters: the iterable, value to sort on (key), and boolean flag for whether you want the object in ascending or descending order.

In the case of our iterable, we want to call `level_count.items()` which generates a list of tuples as `(key,count)`. We iterate over this and for our key, we need to define that we are sorting based on the count/frequency associated with the level. To do this, we can leverage a lambda function to express that we want to use the second value of the current tuple item or index 1. Finally, by default, `sorted()` returns items in ascending order. We could still fetch most frequent level by accessing the last index but its much simpler if we just enable the reverse flag so we can get our solution from the beginning.

Code:
```Python
logs = [

"1000 INFO started",

"1001 ERROR disk full",

"1002 WARN high memory",

"1003 ERROR timeout",

"1004 INFO ok",

"1005 ERROR crash",

]

# # Expected: "ERROR" (appears 3 times)

  

level_count = {}

  

for log in logs:

	slice = log[5:]

	space = slice.index(" ")

	level = slice[:space]

	if level in level_count:

		level_count[level]+=1

	else:

		level_count[level] = 1

print(sorted(level_count.items(), key = lambda count: count[1], reverse=True)[0][0])
```

Output:
```Terminal
ERROR
```

As you can see we get our desired output!

Time complexity for this solution is  $O(n)$ since we have to iterate over the entire log list.

More on other metrics another time...