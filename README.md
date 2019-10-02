# r2d2-oracle
Oracle support for the r2d2 connection pool.
This fits in between the [r2d2](https://crates.io/crates/r2d2) connection manager and [oracle](https://crates.io/crates/oracle) database driver crates.

## Usage
See the documentation of r2d2 for the details on how to use the connection pool.

```rust
use std::thread;
use r2d2_oracle::OracleConnectionManager;

fn main() {
    let manager = OracleConnectionManager::new("user", "password", "localhost");
    let pool = r2d2::Pool::builder()
         .max_size(15)
         .build(manager)
         .unwrap();
    
    for _ in 0..20 {
        let pool = pool.clone();
        thread::spawn(move || {
            let conn = pool.get().unwrap();
            // use the connection
            // it will be returned to the pool when it falls out of scope.
        });
    }
}
```

## Current Status of the Crate & Roadmap to v1.0.0
This is the initial release of the crate and has not yet been proven in production. Nevertheless: the crate is very small so not many problems are expected.
The precondition for releasing v1.0.0 is that both `r2d2` and `oracle` have released their v1.0.0.
