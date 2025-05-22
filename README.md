## Experiment 1.2: Understanding how it works

### What was changed

I added a `println!` statement right after the `spawner.spawn(...)` line in `main.rs`:

```rust
spawner.spawn(async {
    println!("Rifqi's Komputer: howdy!");
    TimerFuture::new(Duration::new(2, 0)).await;
    println!("Rifqi's Komputer: done!");
});

println!("Rifqi's Komputer: hey hey...");
```

### Output

When running the program, the output is:

![image](images/image1.png)

### Explanation

The reason `Rifqi's Komputer: hey hey...` appears before the async task output is because `spawner.spawn(...)` only schedules the async task to be run later. The code inside the async block does **not** execute immediately. Instead, the main thread continues and prints `hey hey...` right away.

The async task (printing "howdy!", waiting, then printing "done!") only starts running when the executor (`executor.run()`) begins processing the scheduled tasks. That happens **after** "hey hey..." is printed.

**In summary:**  
- `spawn` schedules the async task, but does not run it immediately.
- The main thread prints "hey hey..." right away.
- The executor then runs the async task, printing "howdy!", waiting, then "done!".