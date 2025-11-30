# Notes by Aaditya Bhatnagar
## Book: Zero to Production in Rust by Luca Palmieri
These notes will contain anything and everything that I have learnt from this book and deem useful to know. Some of the simple steps or things I already know might not be present in these notes, So i would suggest you refer the book if you happen to get stuck and find yourself in a knowledge gap. 

## Chapter 1: Getting Started
This book will help you get strated with rust setup and git setup for the project but there's no need for notes on that as the steps are pretty straightforward so I wont be including that in my notes. I will be including most and also adding some extra things I have researched to understand better on my journey to the completion of this book.

**Useful Crates/Commands**
- cargo watch {(MAC) I used brew install cargo-watch cause the book route (cargo install cargo-watch) was giving me macOS ARM64 linker error}
- cargo check 
- cargo run
- cargo test
- cargo tarpaulin
- cargo clippy
- cargo fmt
- cargo audit


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
- It is used to prevent broken code from reaching the main branch. Useful for team projects.

Steps:
- cargo test (build before running tests)
- cargo tarpaulin --ignore-tests (code coverage, will collect info and check is some portions of the codebase have been overlooked over time and are poorly tested)
- cargo clippy -- -D warnings (linter, helps spot unidiomatic code, overly-complex constructs, common mistakes/inefficiencies, If suggested a change that is not desirable thenm mute the warning with 
'''rust 
#[allow(clippy::lint_name)]
''' )
- cargo fmt -- --check
- cargo audit (checks if vulnerabilities have been reported for any of the crates in the dependency tree)

You can either choose to build your own CI pipeline or pick up available CI pipelines. I'm listing here the pipeline link for the GitHub Actions pipeline.
GitHub Actions - ![link](https://gist.github.com/LukeMathWalker/5ae1107432ce283310c3e601fac915f3)

## Chapter 2: Building An Email Newsletter
The book makes it very clear about the use case they intend to fulfil with the Email Newsletter, i.e., they want to fulfill three user stories:
- As a Blog Visitor: To be able to subscribe to the newsletter - In order to receive email updates when new content is published on the blog
- As a Blog Author: To send an email to all my subscribers - In order to notify when new content is published
- As a subsriber: To be able to unsubscribe from the newsletter - In order to stop receiving email updates

Future features (not implemented by the book):
- Manage multiple newsletters
- Segment subscribers in multiple audiences
- Track opening and click rates

**Working in Iterations**
Each iteration should take a fixed amount of time and gives us a slightly better version of the product, improving the experience of our users.
Code at the end of every iteration needs to be production-quality

## Chapter 3: Sign Up a New Subscriber
Building the use-case of the a Blog Visitor.

BLOG VISITOR -> inputs email address on web page -> triggers an api call to -> BACKEND SERVER -> processes information, stores it, sends back a response

This chapter focuses on the backend server - /subsciptions POST endpoint

**Strategy**
- Framework Chosen - actix-web (others: rocket, tide, warp). Other choices available are:- rocket, tide, warp. Read this [!link](https://www.lpalmieri.com/posts/2020-07-04-choosing-a-rust-web-framework-2020-edition/) by the author for more info.






