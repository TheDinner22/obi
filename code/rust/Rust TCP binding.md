## What is it?

Bind to a port with TCP protocol in rust using the std library.

## code example

```rust
use std::net::TcpListener // import it

fn main() {

	let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
	for stream in listener.incoming() {

		// this is the equivalent of iterating over attempted connections
		let stream = stream.unwrap();

		// where handle_connection does something with the stream
		handle_connection(stream)

	}

}
```

### break down of code

First, we bring the listener into scope. We then create a listener by binding to a port. Note that this port binding can fail and thus returns a result type. 

```rust
for stream in listener.incoming()
```

This line iterates over each request as a stream. Note that in this means each request is blocking.

```rust
let stream = stream.unwrap();
```

Each ```stream``` represents an attempted connection which could fail for several reasons. Because of this, we unwrap the stream.

```rust
handle_connection(stream)
```

From there we can do anything we want with the stream. See how to read and write to a [[Rust TcpStream|stream]].
