## what is it?

A TcpStream represents incoming (or outgoing) Tcp requests.

You can read and write to/from this request stream.

## code example

```rust
use std::io::prelude::*;
use std::net::TcpListener;
use std::net::TcpStream;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];

    stream.read(&mut buffer).unwrap();

    println!("Request: {}", String::from_utf8_lossy(&buffer));
}
```

### break down

First we include some io and net types from the std library. For what happens in the main function, see [[Rust TCP binding]].

```rust
let mut buffer = [0; 1024];
```

We first define a buffer to be read into. The buffers size is arbitrary and unnecesarily large. There are better ways to define a buffer. One might be to create a vector whose size depends on the number of bytes in the stream.

```rust
stream.read(&mut buffer).unwrap();
```

We then read from the stream into the buffer. This takes a mutable reference to the buffer as the data is being taken out of the stream and put into the buffy. Of course, this data is currently in binary and represents some utf-8 string.

```rust
String::from_utf8_lossy(&buffer)
```

Finally, we print this string. It converts the binary in the buffer to a utf-8 valid string. If any of the bytes in the buffer are not utf-8 valid, they are replaced by some ? looking thing (hence the conversion is safe and lossy).

### note
this "server" does not return anything and will always drop the connection. To return something we need to write to the TcpStream

```rust
```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];

    stream.read(&mut buffer).unwrap();

    let response = "HTTP/1.1 200 OK\r\n\r\n";

    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

The last two lines of this code write a response to the TcpStream. We define some response and convert it to bytes. Then, we write this reponse to the stream. calling the flush method makes sure all bytes written to the stream reach their destination (it is blocking almost like await)

## conclusion

There are many other ways to manipulate TcpStream or streams in general in rust. Reading from and writing to a stream are the most basic oporations.