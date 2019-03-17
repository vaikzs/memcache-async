# memcache-async

[![Build Status](https://travis-ci.org/vavrusa/memcache-async.svg?branch=master)](https://travis-ci.org/vavrusa/memcache-async)
[![Codecov Status](https://codecov.io/gh/vavrusa/memcache-async/branch/master/graph/badge.svg)](https://codecov.io/gh/vavrusa/memcache-async)
[![Crates.io](https://img.shields.io/crates/v/memcache-async.svg)](https://crates.io/crates/memcache-async)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)
[![Docs](https://docs.rs/memcache-async/badge.svg)](https://docs.rs/memcache-async/)

memcache-async is an async [memcached](https://memcached.org/) client implementation.

## Install

The crate is called `memcache-async` and you can depend on it via cargo:

```ini
[dependencies]
memcache-async = "*"
```

## Features

The crate implements the protocol on any stream implementing `AsyncRead + AsyncWrite`.

- [ ] Binary protocol
- [x] ASCII protocol
- [x] TCP connection
- [x] UDP connection
- [x] UNIX Domain socket connection
- [ ] Automatically compress
- [ ] Automatically serialize to JSON / msgpack etc.
- [ ] Typed interface
- [ ] Mutiple server support with custom key hash algorithm
- [ ] SASL authority (plain)

## Basic usage

The crate works with byte slices for values, the caller should implement deserialization if desired.

```rust
use tokio::prelude::*;
use tokio::await;
use tokio::net::UnixStream;
use memcache_async::ascii;

tokio::run_async(async move {
	let sock = await!(UnixStream::connect("memcache.sock")).expect("connected socket");
	let mut ascii = ascii::Protocol::new(sock);

	// set a value
	await!(ascii.set(&"foo", b"bar", 0)).expect("set works");

	// retrieve 
	let value = await!(ascii.get(&"foo")).expect("get works");
	assert_eq!(value, b"bar".to_vec());
});
```

## License

MIT
