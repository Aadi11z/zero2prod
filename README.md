# Notes by Aaditya Bhatnagar
## Book: Zero to Production in Rust by Luca Palmieri
These notes will contain anything and everything that I have learnt from this book and deem useful to know. Some of the simple steps or things I already know might not be present in these notes, So i would suggest you refer the book if you happen to get stuck and find yourself in a knowledge gap. 

## Chapter 1: Getting Started
This book will help you get strated with rust setup and git setup for the project but there's no need for notes on that as the steps are pretty straightforward so I wont be including that in my notes. I will be including most and also adding some extra things I have researched to understand better on my journey to the completion of this book.

**Useful Crates**
- cargo watch {(MAC) I used brew install cargo-watch cause the book route (cargo install cargo-watch) was giving me macOS ARM64 linker error}
- cargo check 
- cargo run
- cargo test

**Inner Development Loop (IDL)**
- A Change
- Compile the app
- Run Tests
- Run the app

* Speed of the inner development loop is an upper bound on the number of iterations that you can complete in a unit of time
* This is important because if it takes 5 minutes to run one iteration of an IDL then, you can do at most 12 IDL in an hour. Even though this is a Rust book and Rust is popular for its speed, it cant help us here because compilation time is a pain point on big projects. So "reduce compilation time". 

**Continuous Integration Pipeline (CI)**
- CI is a system whose job is to constantly check, test, and ship your code. 
- It works with every commit, and depending on the piepline you write, it can make sure your code is buildable, correct, and high-quality.
- It is used to prevent broken code from reaching the main branch. Useful for team projects,.



