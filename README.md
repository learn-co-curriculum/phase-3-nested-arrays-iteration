# Iterating Through Nested Arrays

## Learning Goals

* Display the cells in an `Array` of `Array`s
* Traverse `Array` of `Array`s to produce a single value
* Traverse `Array` of `Array`s to produce a new nested data structure

## Introduction

When we started this module, we mentioned that we often use nested data
structures as a "base" from which to do data processing. `Array`s of `Array`s
is our first milestone in learning to work with nested data.

In the next few labs, we're going to pick out three specific types of
processing to practice:

1. Displaying the nested structure
2. Transforming the nested structure into a new structure (a collection)
3. Transforming the nested structure into a result (a single thing, usually a number)

In all three examples, we will use loops to traverse the entire data structure.

## Review Looping Through Arrays

In previous lessons, we looked at how to traverse an array using a `while` loop.
Let's look at a few examples. Most of the examples used a variable that would
increment every time the loop code executed:

```rb
array = ["a", "b", "c", "d"]

count = 0

while count < array.length do
  # code to work on the array would go here
  count += 1
end
```

If we wanted to output every element:

```rb
array = [100, 300, 50, 450]
count = 0

while count < array.length do
  puts array[count]
  count += 1
end
```

Any simple array can be displayed using this method.

If we wanted to _modify_ each element, we would change `puts array[count]`. Say, for
instance, we want to perform some math operation on each element:

```rb
array = [100, 300, 50, 450]
count = 0

while count < array.length do
  array[count] = array[count] * array[count]
  count += 1
end

array
 # => [10000, 90000, 2500, 202500]
```

The code above alters each element in the original array, replacing each
value with the square of itself. If we didn't want to modify the original, we
can collect the result of each operation in a new array:

```rb
array = [100, 300, 50, 450]
results_array = []
count = 0

while count < array.length do
  results_array << array[count] * array[count]
  count += 1
end

results_array
 # => [10000, 90000, 2500, 202500]
```

Here, `array` is kept as is, but the square of each of its elements is added to
`results_array`.

Finally, if we wanted to derive a single value from an array of elements, we
modify a variable on each loop rather than adding to a new collection. If we
wanted to sum our array values:

```rb
array = [100, 300, 50, 450]
sum = 0
count = 0

while count < array.length do
  sum = sum + array[count]
  count += 1
end

sum
 # => 900
```

We've _reduced_ the array down to a single value.

## Looping Through Nested Arrays

Consider the following array of arrays:

```rb
array_of_arrays = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

If we wanted to print out each nested array manually, we would write:

```rb
array_of_arrays[0]
 # => [1, 2, 3]
array_of_arrays[1]
 # => [4, 5, 6]
array_of_arrays[2]
 # => [7, 8, 9]
```

If we wanted to print the elements in each array, we could add a second set of brackets:

```rb
array_of_arrays[0][0]
 # => 1
array_of_arrays[0][1]
 # => 2
array_of_arrays[0][2]
 # => 3
array_of_arrays[1][0]
 # => 4
array_of_arrays[1][1]
 # => 5
array_of_arrays[1][2]
 # => 6
array_of_arrays[2][0]
 # => 7
array_of_arrays[2][2]
 # => 8
array_of_arrays[2][3]
 # => 9
```

While this works fine, it requires specific, concrete code. If one of these
arrays had more or less than three elements, we would have to change our code to
account for it. Using `while` loops solves this.

When looping through an `Array` of `Array`s data structure, we add a second
`while` loop. First, we start with a single loop:

```rb
count = 0

while count < array_of_arrays.length do
  p array_of_arrays[count]
  count += 1
end
```

The above code will output each nested array:

```rb
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
```

> **Note:** Using `p` will display each array, but `puts` will output all the
> values inside those arrays!  We use `p` here to make the output a little
> clearer.

With a single loop like this, we can access each of the nested arrays. At this
point, we _could_ create a sort of hybrid between looping and directly accessing
values:

```rb
count = 0

while count < array_of_arrays.length do
  p array_of_arrays[count][0]
  p array_of_arrays[count][1]
  p array_of_arrays[count][2]
  count += 1
end
```

This prints out each value from each nested array, but still requires specific
code - `0`, `1`, and `2`, the exact indices of the array elements. Instead, we
can use a second `while` loop:

```rb
count = 0

while count < array_of_arrays.length do
  p array_of_arrays[count]

  inner_count = 0
  while inner_count < array_of_arrays[count].length do
    p array_of_arrays[count][inner_count]
    inner_count += 1
  end

  count += 1
end
```

Notice we've left in the original `p` statement to show each outer loop. This
code outputs:

```sh
[1, 2, 3]
1
2
3
[4, 5, 6]
4
5
6
[7, 8, 9]
7
8
9
```

Take a moment to try and visualize what is happening. Every time the outer
`while` loop executes, the inner `while` loop runs three times. Stepping
through one loop:

* the outer `while` loop executes because `count` is less than the length
  of `array_of_arrays`
* `p array_of_arrays[count]` is called, which prints the entire first nested array
* `inner_count` is assigned to `0`
* the inner `while` loop executes because `inner_count` is less than the length
  of the **first** nested array, `array_of_arrays[count]`
  * `p array_of_arrays[count][inner_count]` is called, printing the _first_ value
    of the first nested array because `count` is `0` and `inner_count` is `0`
  * `inner_count` is incremented and now equals `1`
* the inner `while` loop executes because `inner_count` is less than the length
  of the **first** nested array, `array_of_arrays[count]`
  * `p array_of_arrays[count][inner_count]` is called, printing the _second_ value
    of the first nested array because `count` is still `0` and `inner_count` is `1`
  * `inner_count` is incremented and now equals `2`
* the inner `while` loop executes because `inner_count` is less than the length
  of the **first** nested array, `array_of_arrays[count]`
  * `p array_of_arrays[count][inner_count]` is called, printing the _third_ value
    of the first nested array because `count` is still `0` and `inner_count` is `2`
  * `inner_count` is incremented and now equals `3`
* the inner `while` loop does not execute because `inner_count` is now equal to
  the length of the first nested array
* `count` is incremented and now equals `1`

This process happens two more times, looping through the second and third nested
arrays.

## Mapping Nested Arrays

We displayed nested content, now let's try to collect it. Given the same array of
arrays:

```rb
array_of_arrays = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

What if we wanted to collect all the values of each nested array into a single
array?

First, we create a variable, `results_array`, the new array we want. Then, we
build two `while` loops again. Instead of outputting each element in each
nested array, we'll just push it into the `results_array`.

```rb
count = 0
results_array = [] # new array

while count < array_of_arrays.length do

  inner_count = 0
  while inner_count < array_of_arrays[count].length do
    results_array << array_of_arrays[count][inner_count] # pushes every element into an array
    inner_count += 1
  end

  count += 1
end

results_array
 # => [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Take a moment to think about how this code executes step by step again.

## Reducing Values in Nested Arrays

In a similar fashion to mapping over an array, reducing requires a variable to
contain an accumulated result. If we wanted the sum of all of these nested
values, we would replace the array from last time with an integer:

```rb
count = 0
sum = 0

while count < array_of_arrays.length do

  inner_count = 0
  while inner_count < array_of_arrays[count].length do
    sum = sum + array_of_arrays[count][inner_count] # adds the element's value to sum and sets sum
    inner_count += 1
  end

  count += 1
end

sum
 # => 45
```

In this example, like the others, we access the values inside a nested array
using chained brackets, `[count]` and `[inner_count]`.

## Conclusion

Using two `while` loops, we were able to display, collect, and reduce a set of
nested arrays. The exact design of the loops required for this sort of task is
dependent on the data structure you are working with. An `Array` of `Array` of
`Array`s, for instance, would need _three_ loops.

The critical takeaway here, though, is that we can draw out the information we want
from data structures by iterating over them with basic loops. This sort of task
is so common that Ruby has built-in methods to handle the work like [`each`][],
[`map`][], and [`sum`][] that we can apply directly to arrays. We will learn
these methods soon, but remember that at their cores, they are all based on
simple loops.

[`each`]: https://ruby-doc.org/core-2.5.0/Array.html#method-i-each
[`map`]: https://ruby-doc.org/core-2.5.0/Array.html#method-i-map
[`sum`]: https://ruby-doc.org/core-2.5.0/Array.html#method-i-sum
