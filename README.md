# Notes by Aaditya Bhatnagar  
## *Zero to Production in Rust* — Luca Palmieri

These notes document my learnings while working through **Zero to Production in Rust** by Luca Palmieri.

They include:
- key concepts explained in my own words  
- practical commands, tools, and workflows  
- additional context I researched to fill knowledge gaps  

⚠️ Some basic steps (Rust install, Git setup, etc.) are intentionally omitted.  
If you feel stuck, **refer back to the book** — these notes are a companion, not a replacement.

---

## Chapter 1 — Getting Started

This chapter focuses on setting up the development environment and tooling.  
The steps are straightforward, so I won’t repeat them here, but I’ll highlight **useful tools and workflows** introduced early on.

---

### Useful Crates & Commands

- `cargo watch`  
  > *(macOS)* Installed via `brew install cargo-watch`  
  > `cargo install cargo-watch` caused ARM64 linker errors for me

- `cargo check`
- `cargo run`
- `cargo test`
- `cargo tarpaulin`
- `cargo clippy`
- `cargo fmt`
- `cargo audit`

---

### Inner Development Loop (IDL)

The **Inner Development Loop** represents how fast you can iterate while building software.

**Typical loop:**
1. Make a change
2. Compile the app
3. Run tests
4. Run the application

**Key idea:**  
> The speed of your IDL is an *upper bound* on how many meaningful iterations you can complete per hour.

Even though Rust produces fast binaries, **compile time becomes a bottleneck** on large projects.  
Optimizing your workflow to reduce unnecessary recompilation is critical.

---

### Continuous Integration (CI)

A **CI pipeline** automatically checks your code on every commit to ensure it is:
- buildable
- correct
- idiomatic
- secure

This prevents broken or low-quality code from reaching `main`, especially in team projects.

#### Typical CI Steps

1. **Run tests**
    ```bash
    cargo test
    ```

2. **Code coverage**

    ```bash
    cargo tarpaulin --ignore-tests
    ```

    Helps identify untested or neglected areas of the codebase.

3. **Linting**

    ```bash
    cargo clippy -- -D warnings
    ```

    * catches unidiomatic or inefficient code
    * warnings can be silenced selectively using:
    
    ```rust
    #[allow(clippy::lint_name)]
    ```

4. **Formatting**

    ```bash
    cargo fmt -- --check
    ```

5. **Security audit**

    ```bash
    cargo audit
    ```

You can build your own pipeline or reuse existing ones.

📌 **GitHub Actions reference pipeline:**
[https://gist.github.com/LukeMathWalker/5ae1107432ce283310c3e601fac915f3](https://gist.github.com/LukeMathWalker/5ae1107432ce283310c3e601fac915f3)

---

## Chapter 2 — Building an Email Newsletter

The book clearly defines the **use case** through three core user stories.

### User Stories

* **As a blog visitor**
    I want to subscribe to the newsletter to receive updates when new content is published.

* **As a blog author**
    I want to email all subscribers when new content is published.

* **As a subscriber**
    I want to unsubscribe to stop receiving emails.

### Future Features (Not Implemented)

* Multiple newsletters
* Subscriber segmentation
* Open / click tracking

---

### Working in Iterations

* Each iteration has a **fixed timebox**
* Each iteration produces a **production-quality improvement**
* No “temporary” code — quality is enforced early

This mirrors real-world backend development.

---

## Chapter 3 — Signing Up a New Subscriber

This chapter introduces:

* the first HTTP endpoint
* database integration
* SQLx
* Docker + Postgres
* integration testing
* CI integration

⚠️ This chapter is **intentionally overwhelming** on the first read.

Repetition is expected.

---

### High-Level Flow

```
Blog Visitor
    ↓
Web Form (email + name)
    ↓
POST /subscriptions
    ↓
Backend Server
    ↓
Database (Postgres)
    ↓
HTTP Response
```

This chapter focuses **only on the backend**:

* `/subscriptions` `POST` endpoint

---

### Strategy Decisions

#### Web Framework

* Chosen: **actix-web**
* Alternatives: Rocket, Tide, Warp

📖 Framework comparison by the author:
[https://www.lpalmieri.com/posts/2020-07-04-choosing-a-rust-web-framework-2020-edition/](https://www.lpalmieri.com/posts/2020-07-04-choosing-a-rust-web-framework-2020-edition/)

---

#### Testing Strategy

Rust supports multiple testing approaches:

1. **Embedded tests**

    ```rust
    #[cfg(test)]
    ```

    * hidden behind conditional compilation
    * good for testing internal components
    * suitable for “iceberg projects” (large internals, small public API)

2. **Integration tests** *(used in this project)*

    * placed in the `tests/` directory
    * simulate real user behavior
    * test the system as a whole

3. **Documentation tests**

We use **integration tests** because they validate real HTTP behavior.

📖 Useful references:

* Conditional compilation (`cfg`):
  [https://doc.rust-lang.org/stable/rust-by-example/attribute/cfg.html](https://doc.rust-lang.org/stable/rust-by-example/attribute/cfg.html)
* Testing attributes:
  [https://doc.rust-lang.org/reference/attributes/testing.html](https://doc.rust-lang.org/reference/attributes/testing.html)

---

### Database Integration

* Database: **Postgres**
* Crate: **sqlx**
* Migrations: **sqlx migrations**
* Queries validated at compile-time

---

### Side Quest — Docker Basics

#### Image vs Container

* **Image**

    * blueprint / template
    * immutable
    * like a *class*

```bash
docker pull postgres:latest
docker run postgres
```

* **Container**

    * running instance of an image
    * holds state (DB data, logs, files)
    * like an *object*

```bash
docker start <container_name>
docker ps
```

---

#### Useful Docker Commands

Inspect a container:

```bash
docker inspect <container_name>
```

Enter a running container:

```bash
docker exec -it <container_name> bash
```

Inside Postgres:

```bash
psql -U postgres
\l
\c newsletter
\dt
\d subscriptions
SELECT * FROM subscriptions;
```

---

### Dockerfile (Preview)

A **Dockerfile** is a recipe for building your own image.

It allows you to:

* define OS, dependencies, binaries
* build reproducible environments
* deploy consistently across machines

---

## Next Steps

I’m re-reading **Chapter 3 and Chapter 4** to consolidate my understanding.

Some concepts now make sense individually, but I want to:

* properly structure the project
* avoid redundancy
* understand *why* each piece exists

After that, I’ll revisit specific questions with more clarity.

---

📌 These notes will evolve as I progress further through the book.

```

---