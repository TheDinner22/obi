## what and why

Their are many cases where one might want to convert an iterator of type `T` into an iterator of type `U`. 

### here is one such example

^eef8ba

```rust
// iterator over "1", "2", etc.
let str_iter = "1 2 3 4 5 6".split(" ");

// map nums to a list of numbers
let nums: Vec<i32> = str_iter.map(|str| str.parse().unwrap()).collect();

assert_eq!(vec![1, 2, 3, 4, 5, 6], nums);
```

First, we create an iterator over the first six natural numbers (1, 2, 3, etc.) where each element is of type `&str`.

Then, we map over each string and parse it to a number (here an i32). We call unwrap after parse as parse is failable and returns a result. We know this result will be the Ok varient so calling unwrap is ok here. Finally, we collect the values into a Vector.

We check the `nums` variable on the last line. The assert macro should not panic.

We just converted an iterator over type &str to an iterator over type i32!

## failable conversions

As we saw earlier, converting a string to an i32 is failable: it returns a Result enum. In the above example, this was not a problem as we knew that the conversion would succeed.

### heres an example which would panic

```rust
// iterator over "1", "2", etc.
let str_iter = "1 2 3 not_a_number 4 5 6".split(" ");

// map nums to a list of numbers
let nums: Vec<i32> = str_iter.map(|str| str.parse().unwrap()).collect();

assert_eq!(vec![1, 2, 3, 4, 5, 6], nums);
```

The code is the same as the code explained [[#^eef8ba|here]]; however, the function will now panic as the string `not_a_number` will not parse nicely into an i32. 

The `parse` method returns an error which we then `unwrap`, causing the program to crash.

## Failable conversions without calling panic

What if we wanted to ***try*** a conversion between iterators which ***might*** fail?

Rust's standard library does not provide a try_map method. It turns out we do not need such a method. We can simply map over an iterator and then collect the resulting iterator into a Result<T, E>.

In short, try_map does not exist becuase we can use map to convert an iterator over type `T` into a  `Result<Iter<U>, E>` where E is an error type. If the map succeeds, the result contains the new iterator (or a vector or slice or something). If the map fails, the result contains some Err varient.

> Iter\<U> is meant to represent an iterator over a type U and is not a valid type definition.
> An example of a valid type definition would be `Result<Vec<u8>, E>` where E is some error type.

This is much easier to show with code than it is to explain with words.

### code example

```rust
// iterator over "1", "2", etc.
let str_iter = "1 2 3 not_a_number 4 5 6".split(" ");

// map nums to a list of numbers
let nums: Result<Vec<i32>, _> = str_iter.map(|str| str.parse()).collect();

assert_eq!(true, nums.is_err());
```

First we construct an iterator, same as before.

The nums variable is given a type of `Result<Vec<i32>, _>`. The collect method uses this type annotation to determine how it will convert the iterator. In short, the collect method is doing the heavy lifting for us by converting an iterator over type `Result<i32, Err>` into a `Vec<i32>` ***unless*** any of the elements in the origonal iterator are the Err varient of the Result type, in which case, that error is returned instead of the vector.

Becuase the collect function will return one of two different types, it returns an enum which has each possible return type as a varient.

## Conclusion

The collect function is super powerful! See this link for the collect docs:

>
>https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect
>

This section outlines how to use the collect method to convert an iterator over a bunch of results into a single result contianing either some collection or an error indicating that one of the results in the origonal iterator was the Err varient.