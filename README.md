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
This chapter a lot of details such as building the first endpoint, setting up tests, connecting to Postgres database, using sqlx, CI pipelining, etc...
This chapter was overwhelming for me as well when I went through it the first time, but hopefully repetition will smooth things over.

Building the use-case of the a Blog Visitor.

BLOG VISITOR -> inputs email address on web page -> triggers an api call to -> BACKEND SERVER -> processes information, stores it, sends back a response

This chapter focuses on the backend server - /subsciptions POST endpoint

**Strategy**
- Framework Chosen - actix-web (others: rocket, tide, warp). Other choices available are:- rocket, tide, warp. Read this [article](https://www.lpalmieri.com/posts/2020-07-04-choosing-a-rust-web-framework-2020-edition/) by the author for more info on the comparison between the different types of frameworks.

- Testing Strategy - There methods of testing available: Embedded test Module, External 'tests' folder or Public documentation (doc tests). 

In Embedded Test module, the test is hidden behind a configuration conditional check, '''rust #[cfg(test)] '''. Two operators in Rust that help do configuration conditional check: '''rust cfg ''' attribute and '''rust cfg! ''' macro. '''rust cfg ''' attribute will help with conditional compilation checks while '''rust cfg! ''' macro helps with checks at run-time which evaluate to either '''rust true ''' or '''rust false '''. See [link](https://doc.rust-lang.org/stable/rust-by-example/attribute/cfg.html) for example. This method is good for iceberg projects (projects which are significant in size but the currently exposed functionality is limited, so you can write tests for sub-components to evalue for correctness)

In External Folder and doc testing, it simulates testing code in the same way a user would (Integration Testing). 

We use Integration Testing and make a '''rust tests''' folder to store our tests. 

For better understanding, i would recommend reading up on Conditional Compilation or cfg (configuration flag), which is a powerful attribute for conditional compilation, letting you include or exclude code blocks, functions, or modules based on compile-time conditions like target OS, features, or custom flags, preventing dead code and enabling platform-specific logic. It works with cfg!() macro for runtime checks, enabling features like '''rust #[cfg(test)]''' for tests or '''rust #[cfg(unix)]''' for OS-specific code, essential for building portable and optimized applications. Also you might want to read upon [Testing](https://doc.rust-lang.org/reference/attributes/testing.html) and [attributes](https://doc.rust-lang.org/reference/attributes.html)

- crate to interact with database -
- migrations
- queries

**First Endpoint**


