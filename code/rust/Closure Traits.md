## The three traits

Closures are like functions except instead of having a type (for functions it's
*fn*), they satisfy one of three trait bounds. There are:
- FnOnce (owns arguments)
- FnMut (take arguments by &mut) ^96ade4
- Fn (take arguments by &)

## FnOnce

^127098

FnOnce is for closures that can be called at least one time. Of course, this means **all closures implement this trait**. 

Explicitly adding FnOnce as a trait bound means that the closure will be called **at most** one time.

If a closure moves some **owned value** out of its body, that closure **only implements** FnOnce. Lets look at an example

### unwrap_or_else implements FnOnce 

#### here is the implementation of unwrap_or_else
```rust
impl<T> Option<T> {
    pub fn unwrap_or_else<F>(self, f: F) -> T
    where
        F: FnOnce() -> T
    {
        match self {
            Some(x) => x,
            None => f(),
        }
    }
}
```

Notice that the generic parameter ```F``` implements FnOnce and thus **will be called at most one time**.

## FnMut

FnMut means that the closure will not move any captured values out of its body (something that [[#^127098|FnOnce]], for example, can do).

However, they **might** mutate values that are captured/moved into the closure.

These closures **can be called multiple times**.

### sort_by_key implements FnMut

#### here is a code example 

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


This [[sort_by_key|sort by key]] closure is called on each item in the slice: the closure is called multiple times. Further, getting ```r.width``` does not move, capture, or mutate anything; thus, the closure meets the trait bounds.

## Fn

Fn applies to closures that:
1. do not move captured values out of their body (like [[#^96ade4|FnMut]])
2. do not mutate their captured values (unlike [[#^96ade4|FnMut]]).

That is, you can call these closures more than once without changing their enviroment; this is important for closures that might be called concurently.

Any closure which captures nothing from its envrioment implements Fn.