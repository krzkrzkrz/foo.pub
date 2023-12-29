# Bevy Journey

[Bevy](https://bevyengine.org/) A refreshingly simple data-driven game engine built in Rust.

## Suggested learning approach

Read through the [The Bevy Book](https://bevyengine.org/learn/book/introduction). Although, quite basic at the moment, there are plans on improving the document.
This guide will give you a general perspective of what to expect.

[Bevy Cheatbook](https://bevy-cheatbook.github.io) more extensive guide, goes through a great deal of explaining each part of Bevy (Entities, components, systems, queries, etc).

Unfortunately, both guides dont exactly explain/show you how to put those parts together. Few suggestions here is to look at the example code at:

- [Bevy examples](https://github.com/bevyengine/bevy/tree/v0.5.0/examples)
- [Bevy ECS examples](https://github.com/bevyengine/bevy/blob/v0.5.0/crates/bevy_ecs/examples)

Also reference the [Bevy Cheatbook](https://bevy-cheatbook.github.io/setup/getting-started.html)  
and [Bevy Github examples](https://github.com/bevyengine/bevy/tree/main/examples)

Note, do not use `main` branch as there are subtle changes that occur quite frequently.  
Instead, use the current version. At the time of writing, `0.7` is the latest version

Helpful Youtube resource:

- [Logic Projects](https://www.youtube.com/channel/UC7v3YEDa603x_84PgCPytzA/videos)

## Install Rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Setting up

```shell
cargo new tutorial
cd tutorial
```

In `Cargo.toml`, add `bevy = "0.7.0"`:

```toml
[package]
name = "tutorial"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
bevy = "0.7.0"
```

In `src/main.rs`

```rust
use bevy::prelude::*;

fn main() {
  App::new()
  .add_plugins(DefaultPlugins)
  .run();
}
```

## Running examples in the Bevy repo

First clone the repo:

```rust
git clone git@github.com:bevyengine/bevy.git
```

Then run an example from the `examples` dir:

```rust
cargo run --example hello_world
```

Can also look at `Cargo.toml` to get a list of examples

## Generate and run Bevy docs locally

In any Bevy project root directory, run:

```rust
cargo doc --open
```

## To output to terminal

For debugging, you can use

```rust
info!("Foobar");
info!("{}", transform.translation);
```

Use `#[derive(Debug)` to add support for debug-printing
i.e. `trace!/debug!/info!/warn!/error!`

User-facing formatting (can only be used to print data types that impl Display)

```rust
info!("{}", x);
println!("{}", x);
```

Developer-facing formatting, with code-like syntax (can be used to print data types that impl Debug)

```rust
info!("{:?}", x);
println!("{:?}", x);
```

