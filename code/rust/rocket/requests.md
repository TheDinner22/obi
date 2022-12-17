## How are requests routed??

The handler function's signature and the functions [[attributes - todo|attributes]] determine how requests are routed.
```rust
#[get("/world")] // attributes
fn handler()     // function signature 
{
	/* .. */
} 
```

## routing validation

Before a handler is chosen, rocket performs some routing logic. You can have rocket check

-   The type of one or more dynamic path segments.
-   The type of incoming body data.
-   The types of query strings, forms, and form values.
-   The expected incoming or outgoing format of a request.
-   Any arbitrary, user-defined security or validation policies.

## validating methods

Rocket supports these http verbs:

- post
- get
- put
- delete
- patch
- head (rocket auto does if there is a matching get route or you can make your own)
- options

### reinterpreting methods

If a post request has a body of `Content-Type: application/x-www-form-urlencoded` and the forms **first** field is called \_method (with a valid http verb in the field), then that fields value is used as the method for the request. 

## dynamic paths

angle brackets around a part of a path creats a dynamic path

```rust
#[get("/users/<name>")]
fn create_user(name: &str) -> &'static str {
	if name == "joe" {
		return "joe"
	}
	"fool"
}
```

Here, the path in the attribute means that the part of the URI after "users/" can be any valid `&str`. The `&str` comes from the function signature which asserts that the dynamic path, "\<name\>," must be of type `&str`. 

### Here is another code example from the rocket docs

```rust
#[get("/hello/<name>/<age>/<cool>")]
fn hello(name: &str, age: u8, cool: bool) -> String {
    if cool {
        format!("You're a cool {} year old, {}!", age, name)
    } else {
        format!("{}, we need to talk about your coolness.", name)
    }
}
```

## dynamic segments

Rocket supports dynamic segments. A segments is a bunch of paths. As such, a dynamic segment is more like a catch all at the end of a URI.

```rust
use std::path::PathBuf;

#[get("/page/<path..>")]
fn get_page(path: PathBuf) { /* ... */ }
```

Any request URL that contains "page/" will match to this route. You could think of this as saying "page/\*." 

## FromSegments and FromParam

The URI string is converted to the types specified in the funciton signature by the FromSegments and FromParam traits. Any type which implements these traits can be used in the handler function's signature.

FromParam is for dynamic paths while FromSegments is for dynamic segments. 

# you are on ignored segments