## Description

Sort by key is used to sort slices, arrays, and vectors whose elements are not easily sorted but whose same elements have information that they can easily be sorted by.

That is, if you had an unsorted array of people structs -like this:
```rust
struct Person {
	name: String,
	height: u8 // height in feet, for example
}
```

-you could use the sort_by_key function to sort them by their heights.

### heres how that might look

```rust
#[derive(Debug)]
struct Person {
	name: String,
	height: u8 // height in feet, for example
}

let mut people = vec![
	Person {
		name: "joe".to_string(),
		height: 6
	},
	Person {
		name: "alice".to_string(),
		height: 4
	},
	Person {
		name: "bob".to_string(),
		height: 5
	},
];

// people.sort() this line will error

people.sort_by_key(|p| p.height);  

println!("{:#?}", people);
```

Note that this mutates the list but **not** the contained elements.

The commented line will cause a compiler error as rust has no idea how to compare the Person struct we made. 

Another way to work around this issue would be to implement the ```Ord``` trait on our Person struct and implement the sorting logic there.

### sort rectangles by width

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let mut list = [
        Rectangle {
            width: 10,
            height: 1,
        },
        Rectangle {
            width: 3,
            height: 5,
        },
        Rectangle {
            width: 7,
            height: 12,
        },
    ];

    list.sort_by_key(|r| r.width);
    println!("{:#?}", list);
}
```

You could create another example where you sorted rectangles by their area, height, or surface area.

## Conslusion

The big takeaway is that sort_by_key is useful when you want to sort some list by some information stored in its elemetns, not by the elements themselves.

The function mutates the list to sort it but not the elements.

Further, the closure given to the function does not move any captured values out (it implements [[Closure Traits#FnMut|FnMut]]).