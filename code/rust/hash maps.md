## what is a hash map?

Like an object from JavaScript or a dictionary from python, hash maps link keys of type `T` to values of type `U`. Thats right, unlike JavaScript and Python, rust's hash maps allow for keys to be any type, including custom types!

## creating a hashmap

```rust
let mut bank = HashMap::new();

bank.insert("joe", 6000);
bank.insert("ella", 28);

println!("{:#?}", bank);
```

^34b092

In the first line we create the hash map.

The next two lines show how to add elements to the hash map. Hash map keys must all have the same type (same thing with the values).

>The insert method has one of two behaviors:
>
>1. If the key does not exist in the hash map, the key value pair is added and None is returned.
>2. If the key already exists in the hasg map, the old value is replaced with the new one and the old value is returned wrapped in a Some().

## Reading values from a hash map

### Reading with the get method

The `get` method can be used to retrieve values from a hash map when provided with the key.

#### code example

```rust
let mut bank = HashMap::new();

bank.insert("joe".to_string(), 6000);
bank.insert("ella".to_string(), 28);

let name = "joe".to_string();

let money = bank.get(&name).unwrap_or(&0);


assert_eq!(money, &6000);
```

The get method takes a reference to a key and returns the Option enum.

1. get returns None if there was no matching key
2. get returns Some(&value) if it finds the value

In this example, we default to `&0` if the get method returns none.

### reading with a for loop

You can also iterate over the values in a hash map with a for loop.

#### code example

```rust
let mut bank = HashMap::new();

bank.insert("joe".to_string(), 6000);
bank.insert("ella".to_string(), 28);

for (key, value) in &bank {
	println!("{} : {}", key, value);
}
```

The for loop iterates over each key value pair in the hash map.

>
> I do not know the specifics but I am sure hash maps also support functional iterators.
>

## ownership

Pretty standard in the sense that types which implement the copy trait, such as `i32`, will be copied into the hashmap. Types which are owned, such as `String`, will be moved into the hash map.

If you insert references into a hashmap, the references must be valid for as long as the data they reference is valid; this can mean having to deal with lifetimes to explain things to the compiler.

## how to update a hash map

Hash map's keys must be unique: there can only be one "joe" key in [[#^34b092|bank]]. The values in a hash map do not have the same restriction; the `joe` and `ella` keys could both have a value of `1234`. 

When updating a hashmap, there are three possible implementations. Each has different use cases. Updating a hash map can

1. Overwrite an old value
2. Adding a Key and Value Only If a Key Isn’t Present
3. Updating the value based on the old value

### Overwriting an old value

We already saw that the insert method does this and returns the old value (if there is one).

#### code example

```rust
let mut bank = HashMap::new();  

bank.insert("joe".to_string(), 6000);
bank.insert("joe".to_string(), 28);

let name = "joe".to_string();

let money = bank.get(&name).unwrap_or(&0);

assert_eq!(&28, money);
```

The second call to the insert method overwrites the data written by the first call to the insert method. 

### Adding a Key and Value Only If a Key Isn’t Present

The entry method can be used to implement this behavior. The return type of the entry method is called `Entry` and, like the `Option` type, represents a value which might or might not exist.

Using the same bank example, lets saw we want to see if an account exists under a certain name. If it does we leave it and if it does not we create a new entry with the value set to 0.

#### code example

```rust
let mut bank = HashMap::new();

bank.insert("joe".to_string(), 6000);

let name1 = "joe".to_string();
let name2 = "ella".to_string();

let _mut_ref1 = bank.entry(name1).or_insert(0);
let _mut_ref2 = bank.entry(name2).or_insert(0);

println!("{:#?}", bank);
```

The first call of the entry method returns an `Entry` that contains the value of the `joe` key (since that key exists in the hash map). Thus, the `or_insert` method does nothing and the value is left unchanged.

The second call of the entry method returns an `Entry` that contains some `None` varient (idk exact name). Thus, the `or_insert` method inserts the key value pair with the given default.

Either way, a mutable reference to the underlying value if returned

Using entry instead of hand coding this logic ***plays much nicer with the borrow checker!!!***.

### Updating the value based on the old value

Another common use for hash maps is updating the value based on the old value, for example a counter would work this way.

This code example counts how many times each word appears in a string by using a hashmap. If its the first time seeing a word, we insert the value 0.

#### code example

```rust
let mut counter = HashMap::new();

let text = "Joe is the most handsome wonderful man ever Joe is so cool Joe is so awesome Joe is the best ever";

for word in text.split_whitespace() {
	let word_counter = counter.entry(word).or_insert(0);
	*word_counter += 1;
}

println!("{:#?}", counter);
```

Looping over each word in the string, we get the entry or insert 0 if the entry does not exist. 

word_counter will always be a mutable reference to some element in the hashmap!

We can then deref and mutate the underlying value however we would like. In this case, we increment it.

The reference goes out of scope at the end of the for loop so there is only ever one mutable reference at a time: this code respects the rules of borrowing!