# What is it?

So macro use is like explicitly importing a crate. This means that all of the macros defined in the crate will also be imported.

## what rocket says

> Throughout this guide and the majority of Rocket's documentation, we import rocket explicitly with `#[macro_use]` even though the Rust 2018 edition makes explicitly importing crates optional. However, explicitly importing with `#[macro_use]` imports macros globally, allowing you to use Rocket's macros anywhere in your application without importing them explicitly.
> 
> You may instead prefer to import macros explicitly or refer to them with absolute paths: `use rocket::get;` or `#[rocket::get]`.

## depreciated

As of [Rust 1.30](https://github.com/rust-lang/rust/blob/master/RELEASES.md#version-1300-2018-10-25), this syntax is no longer generally needed and you can use the standard `use` keyword instead.
