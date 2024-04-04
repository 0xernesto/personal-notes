---
description: Overview of Python Syntax
---

# Syntax

## For-Loops

### Iterate Through a List in Reverse Order

We can use the `range(start, stop, step)` function to start the iteration from the end of the list. In the example below, we iterate in reverse order to reverse the order of integers in a list.

```python
myList = [1, 2, 3, 0, 0, 0]
reversedList = []
lastIndex = len(myList) - 1

for i in range(lastIndex, -1, -1):
    reversedList.append(myList[i])
    
print("reversedList = ", reversedList)
```

### Iterate Through a List

```python
myList = [1, 2, 3, 0, 0, 0]

for i in myList:
    print(i)
```

## While-Loops

### Iterate Through a Sequence of Floats

```python
start = 0.0
stop = 1.0
step = 0.1
float_range = []

while start < stop:
    float_range.append(start)
    start += step

print(float_range)
```

## Range

The `range()` function in Python creates an immutable sequence of integers, which can very helpful for iterating through loops. Note that the `stop` is exclusive, meaning it will not be included in the sequence.

### Parameters

<table><thead><tr><th width="131">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>start</code></td><td><mark style="color:yellow;">Optional</mark> - Integer to specify the start position (defaults to 0).</td></tr><tr><td><code>stop</code></td><td><mark style="color:red;">Required</mark> - Integer to specify the stop position.</td></tr><tr><td><code>step</code></td><td><mark style="color:yellow;">Optional</mark> - Integer to specify the sequence increment (defaults to 1).</td></tr></tbody></table>

In the examples below, a `for-loop` is used with the `range()` function to iterate through a sequence of integers and print each item in the sequence.

### Minimal Example

Here, `range(4)` creates a sequence of integers from 0 to 4, in increments of 1, but excluding 4.

```python
for i in range(4):
    print(i)

# Result:
# 0
# 1
# 2
# 3
```

### Comprehensive Examples

Here, `range(3, 6)` creates a sequence of integers from 3 to 6, in increments of 1, but excluding 6.

```python
for i in range(3, 6):
    print(i)

# Result:
# 3
# 4
# 5
```

Here, `range(4, 20, 4)` creates a sequence of integers from 4 to 20, in increments of 4, but excluding 20.

```python
for i in range(4, 20, 4):
    print(i)

# Result:
# 4
# 8
# 12
# 16
```

## Enumerate

The `enumerate()` function in Python adds a counter to an [iterable](https://www.pythonlikeyoumeanit.com/Module2\_EssentialsOfPython/Iterables.html) and returns it in a form of an enumerate object. This object can then be used directly in for loops or be converted into a list of tuples, where each tuple contains a pair of the index and the value from the iterable.

### Parameters

<table><thead><tr><th width="136">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>iterable</code></td><td><mark style="color:red;">Required</mark> - The iterable to be enumerated (e.g., list, strings, dict, ...).</td></tr><tr><td><code>start</code></td><td><mark style="color:yellow;">Optional</mark> - Integer to specify the start position (defaults to 0).</td></tr></tbody></table>

In the examples below, a `for-loop` is used with the `enumerate()` function to iterate through a sequence of values and print each index and item from the sequence.

### Minimal Example

Here, `enumerate(myList)` adds a counter to each item in the list, starting at 0.

```python
myList = ["apple", "banana", "cherry"]

for index, value in enumerate(myList):
    print(index, value)
    
# Result:
# 0 apple
# 1 banana
# 2 cherry
```

### Comprehensive Example

Here, `enumerate(myList, 2)` adds a counter to each item in the list, starting at 2.

```python
myList = ["apple", "banana", "cherry"]

for index, value in enumerate(myList, 2):
    print(index, value)

# Result:
# 2 apple
# 3 banana
# 4 cherry
```
