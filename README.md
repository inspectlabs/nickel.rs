Floor
=======

Floor is supposed to be a simple and lightweight foundation for web applications written in Rust. It's API is inspired by the popular express framework for JavaScript.

Some of the features are:

* Easy handlers: A handler is just a function that takes an `Request` and `ResponseWriter`
* Variables in routes. Just write `my/route/:someid`
* Easy parameter access: `request.params.get(&"someid")`

##How to build Floor

```shell
make all
```

##How to run the example

```shell
make run
```

Then try `localhost:6767/foo` and `localhost:6767/bar` 


##Write your server
Here is how sample server in `example.rs` looks like:
```rust
extern crate http;
extern crate floor;

use floor::{ Floor, Request };
use http::server::{ ResponseWriter };

fn main() {

    let mut server = Floor::new();
    
    // we would love to use a closure for the handler but it seems to be hard
    // to achieve with the current version of rust.

    fn fooHandler (request: Request, response: &mut ResponseWriter) -> () {

        let text = String::new()
                    .append("This is user: ")
                    .append(request.params.get(&"userid".to_string()).as_slice());

        response.write(text.as_bytes()); 
    };

    fn barHandler (request: Request, response: &mut ResponseWriter) -> () { 
        response.write("This is the /bar handler".as_bytes()); 
    };

    // go to http://localhost:6767/user/4711 to see this route in action
    server.get("/user/:userid", fooHandler);

    // go to http://localhost:6767/bar to see this route in action
    server.get("/bar", barHandler);

    server.listen(6767);
}
```

##Contributing

I would love to find a helping hand. Especially if you know Rust, because I don't :)