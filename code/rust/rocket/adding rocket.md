To add rocket to your crate, you shold 
1. add it to your cargo.toml
2. depend on it as an external crate in your project

## add it to your cargo.toml

Heres how that might look:

```toml
[package]
name = "first-rocket"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
rocket = "0.5.0-rc.2"
```

Note that the version has probably changed and you can see an updated quickstart guide here: https://rocket.rs/v0.5-rc/guide/getting-started/

## depend on it as an external crate

Now you need to depend on rocket as an external crate.

```rust
#[macro_use] extern crate rocket;
```

We use [[macro_use]] here to import macros from rocket as well.

## using rocket

Now that you have added and *imported* rocket, you can use it like you would any other crate.

```rust
#[macro_use] extern crate rocket;

#[get("/")]
fn index() -> &'static str {
	"you are so stupid"
}

#[launch]
fn rocket() -> _ {
	rocket::build().mount("/", routes![index])
}
```

Notice how macros such as ```get``` and ```launch``` are imported without and implicit use statements.