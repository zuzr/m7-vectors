# Module 7: Introduction to Vectors

## Overview
In this module, we'll cover the foundational concepts for working with **vectors** in R. Understanding how R stores information in vectors, and the way in which operations are executed in _vectorized_ form is key to understanding how to efficiently write the R programming language.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [Resources](#resources)
- [What is a Vector?](#what-is-a-vector)
- [Indexing Vectors](#indexing-vectors)
  - [Position Indexing](#position-indexing)
  - [Multiple Indicies](#multiple-indicies)
  - [Logical Indices](#logical-indices)
- [Modifying Vectors](#modifying-vectors)
- [Vectorized Operations](#vectorized-operations)
  - [Recycling](#recycling)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Resources
- [R Tutorial: Vectors](http://www.r-tutor.com/r-introduction/vector)


## What is a Vector?
Vectors are collections of elements, all of the same **type** (numeric, character, etc.), which you can store in a variable. For example, if you wanted to store the names "Sarah" and "Arup" both in the same variable, you could do so in a *vector*. The syntax for creating vectors is to use the built in `c` function, which stands for *combine*. The `c` function accepts comma separated **arguments** of the same **type**, and **returns** a single vector of those elements.

```r
# Use the combine (`c`) function to create a vector.
people <- c('Sarah', 'Arup')
print(people)  # [1] "Sarah" "Arup"
```

Once created, you are unable to change the number of elements in a vector. However, you could create a _new vector_ by combining a new element with an existing vector.

```r
# Use the combine (`c`) function to create a vector.
people <- c('Sarah', 'Arup')

# Use the combine (`c`) function to combine the `people` vector and the name 'Josh'.
more.people <- c(people, 'Josh')
print(more.people)  # [1] "Sarah" "Arup" "Josh"
```

## Indexing Vectors
**Indexing** is the processes of retrieving values from a vector. The term **index** refers to an element's position in a vector. Vector elements are indexed starting with `1`, which is distinct from **zero-indexed** languages, whose first element in a vector is accessed with the number `0`. If you want to retrieve a value from a vector, you'll do so by typing an index (or series of incices) after a vector inside of square-brackets (`[]`). The first element in a vector has position (index) `1`.

### Position Indexing
As stated above, you can retrieve a single value from a vector by placing an index inside of square-brackets after the vector:
``` r
# Use the combine (`c`) function to create a vector.
people <- c('Sarah', 'Arup')

# Store the second element in the `people` vector in a variable `person2`
person2 <- people[2]
print(person2)  #[1] "Arup"
```
This syntax should look familiar from other times that you've printed out a variable. As it turns out, in R, **everything is a vector**. Even when you create a variable storing the number 7 (`x <- 7`), R creates a vector of length 1, and puts the number 7 in the first position. This is why printing out a variable with a single element in it provides you with an index of that value:

```r
# Create a vector of length 1 in a variable x
x <- 7
# Print out x: R states the vector index (1) in the console
print(x)  # [1] 7
```
If you pass a **negative-index** into the square-brackets, R will return all elements _except_ the (negative) index specified.

```r
# Create a `colors` vector
colors <- c('yellow', 'blue', 'orange')

# Return all elements except the second index
all.but.blue <- colors[-2]  # returns 'yellow', 'orange'

```

Passing a **out-of-range** value into the square brackets will return `NA`, which stands for _Not Available_:
```r
# Create a vector x
x <- c(1,2,3)

# Attempt to access the 10th element
x[10]  # returns NA
```

### Multiple Indicies
As you can imagine, you may want to access multiple elements inside of a vector. To do so, you can pass a **vector of indices** into your square brackets:

```r
# Create a `colors` vector
colors <- c('yellow', 'blue', 'orange')

# Retrieve the second and third elements from the `colors` vector
colors[c(2, 3)]
```

If you find that a bit messy, you can store your indicies (`c(1,2)`) in a variable, and then pass that variable into the square-brackets:

```r
# Create a `colors` vector
colors <- c('yellow', 'blue', 'orange')

# Store desired indices in a vector
indices <- c(2, 3)

# Retrieve the second and third elements from the `colors` vector
colors[indices]
```

Recall the `seq` function from [module-6](https://github.com/INFO-201/m6-functions) which produced a sequence of numbers. A handy shorthand for the sequence function is the colon operator (`a:b`), which returns a vector from `a` to `b` (incrementing by `1`). This allows you to write nicely readable code such as this:

```r
# Create a `colors` vector
colors <- c('yellow', 'blue', 'orange', 'green', 'black')

# Retrieve values in positions 2 through 5
colors[2:5]
```

This easily reads as _return vector elements in positions 2 through 5_.

### Logical Indices
In the section above, we used a vector of indices to retrieve values from a vector. An alternative is to use a **vector of boolean values** to indicate which values you want to return. For example:

```r
# Create a vector of shoe sizes
shoe.sizes <- c(7, 6.5, 4, 11, 8)

# Use a vector of boolean values to retrieve the first, fourth, and fifth elements
shoe.sizes[c(TRUE, FALSE, FALSE, TRUE, TRUE)]  # returns 7, 11, 8

```

This may seem a bit strange, but consider the case in which you want to select elements from a vector that meet a certain criteria. You could first create a vector of boolean values that meet that criteria, then use that vector of boolean values to retrieve the elements of interest:

```r
# Create a vector of shoe sizes
shoe.sizes <- c(7, 6.5, 4, 11, 8)

# Create a boolean vector that indicates if a shoe size is greater than 6.5
shoe.is.big <- shoe.sizes > 6.5  # returs T, F, F, T, T

# Use the `shoe.is.big` vector to select large shoes
big.shoes <- shoe.sizes[shoe.is.big]  # returns 7, 11, 8
```
We can even combine the second and third lines of code into a single statement. You can think of the following statement as saying _shoe.sizes where shoe.sizes is greater than 6.5_. This is a valid statement because the equality inside of the square-brackets (`[shoe.sizes > 6.5]`) **returns a vector of boolean values**, which is then used to select the elements out of the `shoe.sizes` vector:

```r
# Create a vector of shoe sizes
shoe.sizes <- c(7, 6.5, 4, 11, 8)

# Select shoe sizes that are greater than 6.5
shoe.sizes[shoe.sizes > 6.5]  # returns 7, 11, 8
```

Understanding vector indexing is crucial for being able to ask real world questions of our datasets. To practice working with vectors, see [exercise-1](exercise-1).

## Modifying Vectors
While we are unable to change the number of elements within a vector, we _are_ able to change the values within a vector. To achieve this, we isolate the element of interest on the **left-hand side** of our assignment, and then assign the element a new value:

```r
# Create a vector of school supplies
school.supplies <- c('Backpack', 'Laptop', 'Pen')

# Replace 'Pen' with 'Pencil'
school.supplies[3] <- 'Pencil'
```

And of course, there's no reason that you can't select multiple elements on the left-hand side, and assign them multiple values:
```r
# Create a vector of school supplies
school.supplies <- c('Backpack', 'Laptop', 'Pen')

# Replace  'Laptop' with 'Tablet', and 'Pen' with 'Pencil'
school.supplies[c(2,3)] <- c('Tablet', 'Pencil')
```

We can do even more advanced element replacement by understanding the idea of recycling ([see below](#recycling)).

## Vectorized Operations
When performing mathematical transformations on vectors, it's important to understand how R computes the combination of two vectors. When performing vector arithmetic, elements are combined **member-wise**. In other words, each element from one vector is modified by the element in the **same corresponding position** in a modifying vector. For example:

```r
# Create two vectors to combine
v1 <- c(1, 1, 1, 1, 1)
v2 <- c(1, 2, 3, 4, 5)

# Create arithmetic combinations of the vectors
v1 + v2  # returns 2, 3, 4, 5, 6
v1 - v2  # returns 0, -1, -2, -3, -4
v1 * v2  # returns 1, 2, 3, 4, 5
v1 / v2  # returns 1, .5, .33, .25, .2

# Add a vector to itself
v3 <- v1 + v1  # returns 2, 2, 2, 2, 2

# Perform more advanced arithmetic
v4 <- (v1 + v2) / v3  # returns 1, 1.5, 2, 2.5, 3
```
While you're unable to use mathematical operators (namely, `+`) to combine character vectors, you can use the `paste` function to concatenate the character elements from two vectors.

```r
colors <- c('Green', 'Blue')
spaces <- c('sky', 'grass')
bands <- paste0(colors, spaces)  # returns "Greensky", "Bluegrass"
# http://greenskybluegrass.com/
```

R will also apply a function to each element within a vector _by default_. For example, consider the `substr` function that takes a sub-string of a set of characters from a starting position to a final position:

```r
# Take a substring of the characters "hello"
substr("hello", 2, 4)  # returns "ell"
```
If we pass a vector of character values to the `substr` function, it will perform the same task on each element in the vector.

```r
# Create a vector of people
people <- c('Adam', 'Rashmi', 'Ying', 'Sarah')

# Perform the vectorized operation
first.initial <- substr(people, 1, 1)

# Print the results
print(first.initial)  # returns "A", "R", "Y", "S"
```

This is **extremely powerful**, because many other programming languages require the explicit iteration through elements in a collection.

In the next section, we'll discuss the concept of **recycling** to explain how R treats vector arithmetic for vectors of different length.

### Recycling
**Recycling** refers to what R does in cases when there are an unequal number of elements in two vectors that are being combined. If R is tasked with performing a vectorized operation with two vectors of unequal length, it will reuse (**recycle**) elements from the shorter vector. For example:

```r
# Create vectors to combine
v1 <- c(1, 2, 3)
v2 <- c(1, 2)

# Add vectors
v3 <- v1 + v2  # returns (2, 4, 4)
```
In the example above, R first combined the elements in the first position of each vector (`1 + 1`). Then, it combined elements from the second position (`2 + 2`). When it got to the third element (which only was present in `v1`), it went back to the **beginning** of `v1` to select a value, yielding `3 + 1`.

Note, R may provide a warning message, notifying you that the vectors are of different length. This is important, because this may be indicative of an error (i.e., you thought the vectors were of the same length, but overlooked it).

Let's move towards a more useful example. Imagine you had a vector of values in which you wanted to replace all numbers greater that 10 with the number 10. You can leverage **recycling** to repeat an element of length 1 for each element you wish to replace:

```r
# Element of values
v1 <- c(1, 5, 55, 1, 3, 11, 4, 27)

# Replace all values greater than 10 with 10
v1[v1>10] <- 10  # returns 1, 5, 10, 1, 3, 10, 4, 10
```
In this example, the number `10` get recycled for each element in which v1 is greater than 10 (`v1[v1>10]`).
