# Rust - Traits

* Share behaviour using `Trait`s

```rust
// Summary trait has a method called summarize and its default
// implementation is to return this "(Read more..)" string
pub trait Summary {
    fn summarize(&self) -> String {
      String::from("(Read more..)")
    };
}

pub struct FacebookMessage {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

pub struct TwitterMessage {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for FacebookMessage {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

// The TwitterMessage struct ovverides this defaults Summary traits implementation
// and implements its on summarize method
impl Summary for TwitterMessage {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

let twitter_message = TwitterMessage {
    username: String::from("foobar"),
    content: String::from("Lorem ipsum dolor"),
    reply: false,
    retweet: false,
};

println!("1 new tweet: {}", twitter_message.summarize()); // Returns: "1 new tweet: foobar: Lorem ipsum dolor"
```

### Trait defaults

* Return the same information without writing the same function twice

```rust
struct NewsArticle {
    title: String,
    content: String,
}

struct ShorterArticle {
    title: String,
}

struct AnotherArticle {
    title: String,
}

trait Summary {
    // Implement a default function body (fn summarize) for the Summary trait that both structs can use
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

// No need to define "fn summarize" for each impl block, since it was already defined in the trait
impl Summary for NewsArticle {} // Don't edit this line
impl Summary for ShorterArticle {} // Don't edit this line

// Unless there is another type (i.e. Struct) that needs to implement its own summarize method
impl Summary for AnotherArticle {
    fn summarize(&self) -> String {
        String::from("-To be continued-")
    }
}

fn main() {
    let news_article = NewsArticle {
        title: String::from("Penguins win the Stanley Cup Championship!"),
        content: String::from(
            "The Pittsburgh Penguins once again are the best hockey team in the NHL.",
        ),
    };

    let shorter_article = ShorterArticle {
        title: String::from("Penguins win the Stanley Cup Championship!"),
    };

    let another_article = AnotherArticle {
        title: String::from("Wonderful sunny day today"),
    };

    println!(
        "New article available! Title: {} Content: {} {}",
        news_article.title,
        news_article.content,
        news_article.summarize()
    ); // Returns: "New article available! Title: Penguins win the Stanley Cup Championship! Content: The Pittsburgh Penguins once again are the best hockey team in the NHL. (Read more...)"

    println!(
        "New shorter article available! Title: {} {}",
        shorter_article.title,
        shorter_article.summarize()
    ); // Returns: "New shorter article available! Title: Penguins win the Stanley Cup Championship! (Read more...)"

    println!(
        "Another article available! Title: {} {}",
        another_article.title,
        another_article.summarize()
    ); // Returns: "Another article available! Title: Wonderful sunny day today -To be continued-"
}
```

### Traits within traits

```rust
pub trait Summary {
    fn summarize_author(&self) -> String {
        format!("@{}", self.username)
    }

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}

// ...

println!("1 new tweet: {}", twitter_message.summarize());
```

### Traits as arguments

* The `impl Trait` syntax works for short examples

```rust
pub fn notify(item: impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

### Trait bounds

Traits as paramaters can be written in two ways.

**impl syntax**:

```rust
// Trait as arguments using impl:
pub fn notify(item1: &impl Summary, item2: &impl Summary) {
```

The above works for straight forward cases. `impl` could be used for more concise code in simple cases.  
In above case, `item1` and `item2` can be of *any* type. However, if the `item1` and `item2` have to be of the
same type, then you cannot really do this with the `impl` syntax, you will need to use `trait bounds`

**Trait bound syntax**:

```rust
// T is a generic type, item1 and item2 are both of type T
pub fn notify<T: Summary>(item1: &T, item2: &T) {
```

### Specify Multiple traits with `+`

Using the `impl` syntax:

```rust
// Its like saying, item1 must implement the Summary and the Display trait while item2 only implements
// the Summary trait
pub fn notify(item1: &(impl Summary + Display), item2: &impl Summary) {
```

Using the `trait bound` syntax:

```rust
// Its like saying, we have a generic type T that implements the Summary and Display traits
// T is one type, item1 and item2 are both of type T
pub fn notify<T: Summary + Display>(item1: T, item2: T) {
```

### `where`

Specifying multiple `trait bounds` could make things harder to read:

```rust
// Is like saying, we have a generic type T that implements the Display and Clone trait and
// a generic type U which implements Clone and Debug
fn some_function<T: Display + Clone, U: Clone + Debug>(t: T, u: U) -> i32 {
```

This is better:

```rust
// Here we are declaring some_function which declares two generics: T, U
// The paramaters, (t: T, u: U), of this function use this generics
// After the return type, -> i32, we specify the where clause
// We are saying: where gerenct type T implements Display and Clone and
// generic type U implements CLone and Debug
fn some_function<T, U>(t: T, u: U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```

