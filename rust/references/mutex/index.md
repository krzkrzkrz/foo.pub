# Mutex

* Is another way to change values without declaring `mut`
* Mutex means "mutual exclusion", which means "only one at a time"
* Mutex is safe because it only lets one thread change it at a time.
* It uses a method called `.lock()`, which returns a struct called a `MutexGuard`
* A Mutex is unlocked when the `MutexGuard` goes out of scope

  > A `MutexGuard` is like locking a door from the inside. You go into a room and lock the door,
  > and now you can change things inside the room. Nobody else can come in and stop you
  > because you locked the door

```rust
use std::sync::Mutex;

fn main() {
    let my_mutex = Mutex::new(5);
    println!("{my_mutex:?}"); // Mutex { data: 5, poisoned: false, .. }

    {
        let mut mutex_changer = my_mutex.lock().unwrap();
        *mutex_changer = 6;

        println!("{my_mutex:?}"); // Prints: Mutex { data: <locked>, poisoned: false, .. }
    } // At this point, mutex_changer goes out of scope and is now gone, and my_mutex isn't locked anymore

    println!("{my_mutex:?}"); // Prints: Mutex { data: 6, poisoned: false, .. }
}
```

## drop()

* To unlock a mutex

```rust
use std::sync::Mutex;

fn main() {
    let my_mutex = Mutex::new(5);
    println!("{my_mutex:?}"); // Mutex { data: 5, poisoned: false, .. }

    {
        let mut mutex_changer = my_mutex.lock().unwrap();
        *mutex_changer = 6;

        println!("{my_mutex:?}"); // Prints: Mutex { data: <locked>, poisoned: false, .. }
        drop(mutex_changer); // This drops mutex_changer. It is now gone, and my_mutex is unlocked.
        println!("Mutex was dropped/unlocked: {my_mutex:?}"); // Prints: Mutex was dropped/unlocked: Mutex { data: 6, poisoned: false, .. }
    }

    println!("{my_mutex:?}"); // Prints: Mutex { data: 6, poisoned: false, .. }
}
```

## try_lock()

* You have to be careful with a `Mutex` because if another variable tries to `.lock()` it, it
  will wait forever. This is known as a deadlock
* `Mutexes` are made for usage across multiple threads, and if you have two threads doing two
  things at the same time, any call to `.lock()` to a `Mutex` that is already locked should wait
  until the other thread is done

Consider this code:

```rust
use std::sync::Mutex;
fn main() {
    let my_mutex = Mutex::new(5);

    // mutex_changer has the lock after this line
    let mut mutex_changer = my_mutex.lock().unwrap();

    // Buto ther_mutex_changer wants the lock, too
    // The program will wait forever
    let mut other_mutex_changer = my_mutex.lock().unwrap();

    println!("This will never print...");
}
```

With `try_lock()` you can check if the `Mutex` is locked and do something else if it is:

```rust
use std::sync::Mutex;

fn main() {
    let my_mutex = Mutex::new(5);

    let mut mutex_changer = my_mutex.lock().unwrap();
    let mut other_mutex_changer = my_mutex.try_lock();

    if let Ok(value) = other_mutex_changer {
        println!("The MutexGuard has: {value}")
    } else {
        println!("Didn't get the lock")
    } // Prints: Didn't get the lock
}
```

## Change values in the mutex

* You can use `lock()` to change the value right away:

```rust
use std::sync::Mutex;

fn main() {
    let my_mutex = Mutex::new(5);
    for _ in 0..100 {
        *my_mutex.lock().unwrap() += 1;
    }

    println!("{my_mutex:?}"); // Prints: Mutex { data: 105, poisoned: false, .. }
}
```

## RwLock

* Stands for "read–write lock"
* An `RwLock` will allow any number of readers to acquire the lock as long as a writer is not holding the lock
* Used to get mutable or immutable references to the value inside
* You use `.write().unwrap()` instead of `.lock().unwrap()` to change it
* `.read().unwrap()` to get read access

```rust
use std::sync::RwLock;

fn main() {
    let my_rwlock = RwLock::new(5);
    let read1 = my_rwlock.read().unwrap();
    let read2 = my_rwlock.read().unwrap();

    println!("{read1:?}, {read2:?}"); // Prints: 5, 5

    let write1 = my_rwlock.write().unwrap(); // Deadlock happens here
}
```

To solve this, use `drop()`:

```rust
use std::sync::RwLock;

fn main() {
    let my_rwlock = RwLock::new(5);
    let read1 = my_rwlock.read().unwrap();
    let read2 = my_rwlock.read().unwrap();

    println!("{read1:?}, {read2:?}"); // Prints: 5, 5

    drop(read1);
    drop(read2);

    let mut write1 = my_rwlock.write().unwrap();
    *write1 = 6;

    drop(write1);

    println!("{:?}", my_rwlock); // Prints: RwLock { data: 6, poisoned: false, .. }
}
```

Another way is to control this flow, use `try_lock()` and `try_read()`:

```rust
use std::sync::RwLock;

fn main() {
    let my_rwlock = RwLock::new(5);

    let read1 = my_rwlock.read().unwrap();
    let read2 = my_rwlock.read().unwrap();

    if let Ok(mut number) = my_rwlock.try_write() {
        *number += 10;
        println!("Now the number is {}", number);
    } else {
        println!("Couldn't get write access, sorry!")
    }; // Prints:
       // Couldn't get write access, sorry!
}
```

