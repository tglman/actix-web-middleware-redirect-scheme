# actix-web-middleware-redirect-scheme

[![Build Status](https://travis-ci.org/perdumonocle/actix-web-middleware-redirect-scheme.svg?branch=master)](https://travis-ci.org/perdumonocle/actix-web-middleware-redirect-scheme)

A middleware for actix-web which forwards all `http` requests to `https` and vice versa. Based on actix-web-middleware-redirect-https.

There is no need to use this crate if you only need to redirect to HTTPS, in this case use original crate [actix-web-middleware-redirect-https](https://crates.io/crates/actix-web-middleware-redirect-https)

[crates.io](https://crates.io/crates/actix-web-middleware-redirect-scheme)

[docs.rs](https://docs.rs/actix-web-middleware-redirect-scheme)

## Usage HTTP -> HTTPS

```toml
# Cargo.toml
[dependencies]
actix-web-middleware-redirect-scheme = "1.1"
```

```rust
use actix_web::{App, web, HttpResponse};
use actix_web_middleware_redirect_scheme::RedirectScheme;

App::new()
    .wrap(RedirectScheme::default())
    .route("/", web::get().to(|| HttpResponse::Ok()
                                    .content_type("text/plain")
                                    .body("Always HTTPS!")));
```
By default, the middleware simply replaces the `scheme` of the URL with `https://`, but you may need to it to change other parts of the URL.
For example, in development if you are not using the default ports (80 and 443) then you will need to specify their replacement, as below:

```rust
use actix_web::{App, web, HttpResponse};
use actix_web_middleware_redirect_scheme::RedirectScheme;

App::new()
    .wrap(RedirectScheme::with_replacements(false, &[(":8080".to_owned(), ":8443".to_owned())]))
    .route("/", web::get().to(|| HttpResponse::Ok()
                                    .content_type("text/plain")
                                    .body("Always HTTPS on non-default ports!")));
```

## Usage HTTPS -> HTTP

```toml
# Cargo.toml
[dependencies]
actix-web-middleware-redirect-scheme = "1.1"
```

```rust
use actix_web::{App, web, HttpResponse};
use actix_web_middleware_redirect_scheme::RedirectScheme;

App::new()
    .wrap(RedirectScheme::build(true))
    .route("/", web::get().to(|| HttpResponse::Ok()
                                    .content_type("text/plain")
                                    .body("Always HTTP!")));
```
By default, the middleware simply replaces the `scheme` of the URL with `http://`, but you may need to it to change other parts of the URL.
For example, in development if you are not using the default ports (80 and 443) then you will need to specify their replacement, as below:

```rust
use actix_web::{App, web, HttpResponse};
use actix_web_middleware_redirect_scheme::RedirectScheme;

App::new()
    .wrap(RedirectScheme::with_replacements(true, &[(":8443".to_owned(), ":8080".to_owned())]))
    .route("/", web::get().to(|| HttpResponse::Ok()
                                    .content_type("text/plain")
                                    .body("Always HTTP on non-default ports!")));
```