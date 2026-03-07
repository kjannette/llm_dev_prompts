---
title: Code Styleguide — Go
last_modified: 2026-02-22
---

1.  Try to hard wrap long lines at 77 characters or less.

1.  Don't commit anything that hasn't been `go fmt`'d. The only exception is
    when committing things that aren't yet syntactically valid, which should
    only happen pre-v0.0.1 or on a non-`main` branch.

1.  Even if you are planning to deal with only positive integers, use
    `int`/`int64` types instead of `uint`/`uint64` types. This is for
    consistency and compatibility with the standard library; it's better than
    casting all the time.

1.  Any project that has more than 2 or 3 modules should use the
    `go.uber.org/fx` dependency injection framework to keep things tidy.

1.  If you have to choose between readable and clever, opt for readable. It's ok
    to make the code less concise or slightly less idiomatic if you can keep it
    dead simple.

1.  Embed the git commit hash into the binary and include it in startup logs and
    in health check output. This is to make it easier to correlate running
    instances with their code. Do not include build time or build user, as these
    will make the build nondeterministic.

    Example relevant Makefile sections:

    Given a `main.go` like:

    ```go
    package main

    import (
        "fmt"
    )

    var (
        Version   string
        Buildarch string
    )

    func main() {
        fmt.Printf("Version: %s\n", Version)
        fmt.Printf("Buildarch: %s\n", Buildarch)
    }
    ```

    ```make
    VERSION := $(shell git describe --always --dirty)
    BUILDARCH := $(shell uname -m)

    GOLDFLAGS += -X main.Version=$(VERSION)
    GOLDFLAGS += -X main.Buildarch=$(BUILDARCH)

    # osx can't statically link apparently?!
    ifeq ($(UNAME_S),Darwin)
            GOFLAGS := -ldflags "$(GOLDFLAGS)"
    endif

    ifneq ($(UNAME_S),Darwin)
            GOFLAGS = -ldflags "-linkmode external -extldflags -static $(GOLDFLAGS)"
    endif

    ./httpd: ./pkg/*/*.go ./internal/*/*.go cmd/httpd/*.go
        go build -o $@ $(GOFLAGS) ./cmd/httpd/*.go
    ```

1.  Avoid obvious footguns. For example, use range instead of for loops for
    iterating.

1.  Use `log/slog` for structured logging. Import `sneak.berlin/go/simplelog`
    for sensible defaults. Example:

    ```go
    package main

    import (
        "log/slog"
        _ "sneak.berlin/go/simplelog"
    )

    func main() {
        slog.Info("Starting up")
    }
    ```

1.  Commit at least a single test file to check compilation. The test file can
    be empty, but it should exist. This is to ensure that `go test ./...` will
    always function as a syntax check at a minimum.

1.  Full TDD and coverage isn't that important, but when fixing a specific bug,
    try to write a test that reproduces the bug before fixing it. This will help
    ensure that the bug doesn't come back later, and crystallizes the experience
    of discovering the bug and the resulting fix into the repository's history.

1.  For anything beyond a simple script or tool, or anything that is going to
    run in any sort of "production" anywhere, make sure it passes
    `golangci-lint`.

1.  Write a `Dockerfile` for every repo, even if it only runs the tests and
    linting. `docker build .` should always make sure that the code is in an
    able-to-be-compiled state, linted, and any tests run. The Docker build
    should fail if linting doesn't pass.

1.  Every repo must have a `Makefile`. See
    [Repository Policies](https://git.eeqj.de/sneak/prompts/raw/branch/main/prompts/REPO_POLICIES.md)
    for required targets and conventions.

1.  If you are writing a single-module library, `.go` files are okay in the repo
    root.

1.  If you are writing a multi-module project, put all `.go` files in a `pkg/`
    or `internal/` subdirectory. `internal/` is for modules used only by the
    current repo, and `pkg/` is for modules that can be consumed externally.
    This is to keep the repo root as clean as possible.

1.  Binaries go in `cmd/` directories. Each binary should have its own
    directory. This is to keep the root clean and to make it easier to see what
    is a library and what is a binary. Only package `main` files should be in
    `cmd/*` directories.

1.  Keep the `main()` function as small as possible.

1.  Keep the `main` package as small as possible. Move as much code as is
    feasible to a library package, even if it's an internal one. `main` is just
    an entrypoint to your code, not a place for implementations. Exception:
    single-file scripts.

1.  HTTP HandleFuncs should be returned from methods or functions that need to
    handle HTTP requests. Don't use methods or your top level functions as
    handlers.

1.  Provide a .gitignore file that ignores at least `*.log`, `*.out`, and
    `*.test` files, as well as any binaries.

1.  Constructors should be called `New()` whenever possible. `modulename.New()`
    works great if you name the packages properly.

1.  Don't make packages too big. Break them up.

1.  Don't make functions or methods too big. Break them up.

1.  Use descriptive names for functions and methods. Don't be afraid to make
    them a bit long.

1.  Use descriptive names for modules and filenames. Avoid generic names like
    `server`. `util` is banned.

1.  Constructors should take a Params struct if they need more than 1-2
    arguments. Positional arguments are an endless source of bugs and should be
    avoided whenever possible.

1.  Use `context.Context` for all functions that need it. If you don't need it,
    you can pass `context.Background()`. Anything long-running should get and
    abide by a Context. A context does not count against your number of function
    or method arguments for purposes of calculating whether or not you need a
    Params struct, because the `ctx` is always first.

1.  Contexts are always named `ctx`.

1.  Use `context.WithTimeout` or `context.WithDeadline` for any function that
    could potentially run for a long time. This is especially true for any
    function that makes a network call. Sane timeouts are essential.

1.  If a structure/type is only used in one function or method, define it there.
    If it's used in more than one, define it in the package. Keep it close to
    its usages. For example:

    ```go
    func (m *Mothership) tvPost() http.HandlerFunc {

        type MSTVRequest struct {
                URL string `json:"URL"`
        }

        type MSTVResponse struct {
        }

        return func(w http.ResponseWriter, r *http.Request) {
            // parse json from request
            var reqParsed MSTVRequest
            err = json.NewDecoder(r.Body).Decode(&reqParsed)
            ...

            if err != nil {
                    SendErrorResponse(w, MSGenericError)
                    return
            }

            log.Info().Msgf("Casting to %s: %s", tvName, streamURL)
            SendSuccessResponse(w, &MSTVResponse{})
        }
    }
    ```

1.  Avoid global state, especially global variables. If you need to store state
    that is global to your launch or application instance, use a package
    `globals` or `appstate` with a struct and a constructor and require it as a
    dependency in your constructors. This will allow consumers to be more easily
    testable and will make it easier to reason about the state of your
    application. Alternately, if your dependency graph allows for it, put it in
    the main struct/object of your application, but remember that this harms
    testability.

1.  Package-global "variables" are ok if they are constants, such as static
    strings or integers or errors.

1.  Whenever possible, avoid hardcoding numbers or values in your code. Use
    descriptively-named constants instead. Recall the famous SICP quote:
    "Programs must be written for people to read, and only incidentally for
    machines to execute." Rather than comments, a descriptive constant name is
    much cleaner.

    Example:

    ```go

    const jsonContentType = "application/json; charset=utf-8"

    func (s *Handlers) respondJSON(w http.ResponseWriter, r *http.Request, data interface{}, status int) {
         w.WriteHeader(status)
         w.Header().Set("Content-Type", jsonContentType)
         ...
     }
    ```

1.  Define your struct types near their constructors.

1.  Do not create packages whose sole purpose is to hold type definitions.
    Packages named `types`, `domain`, or `models` that contain only structs and
    interfaces (with no behavior) are a code smell. Define types alongside the
    code that uses them. Type-only packages force consuming packages into alias
    imports and circular-dependency gymnastics, and indicate that the package
    boundaries were drawn around nouns instead of responsibilities. If multiple
    packages need the same type, put it in the package that owns the behavior,
    or in a small, focused interface package — not in a grab-bag types package.

1.  When defining custom string-based types (e.g. `type ImageID string`),
    implement `fmt.Stringer`. Use `.String()` at SDK and library boundaries
    instead of `string(v)`. This makes type conversions explicit, grep-able, and
    consistent across the codebase. Example:

    ```go
    type ContainerID string

    func (id ContainerID) String() string { return string(id) }

    // At the Docker SDK boundary:
    resp, err := c.docker.ContainerStart(ctx, id.String(), opts)
    ```

1.  Define your interface types near the functions that use them, or if you have
    multiple conformant types, put the interface(s) in their own file.

1.  Define errors as package-level variables. Use a descriptive name for the
    error. Use `errors.New` to create the error. If you need to include
    additional information in the error, use a struct that implements the
    `error` interface.

1.  Use lowerCamelCase for local function/variable names. Use UpperCamelCase for
    type names, and exported function/variable names. Use snake_case for JSON
    keys. Use lowercase for filenames.

1.  Explicitly specify UTC for datetimes unless you have a very good reason not
    to. Use `time.Now().UTC()` to get the current time in UTC.

1.  String dates should always be ISO8601 formatted. Use `time.Time.Format` with
    `time.RFC3339` to get the correct format.

1.  Use `time.Time` for all date and time values. Do not use `int64` or `string`
    for dates or times internally.

1.  When using `time.Time` in a struct, use a pointer to `time.Time` so that you
    can differentiate between a zero value and a null value.

1.  Use `time.Duration` for all time durations. Do not use `int64` or `string`
    for durations internally.

1.  When using `time.Duration` in a struct, use a pointer to `time.Duration` so
    that you can differentiate between a zero value and a null value.

1.  Whenever possible, in argument types and return types, try to use standard
    library interfaces instead of concrete types. For example, use `io.Reader`
    instead of `*os.File`. Tailor these to the needs of the specific function or
    method. Examples:
    - **`io.Reader`** instead of `*os.File`:
        - `io.Reader` is a common interface for reading data, which can be
          implemented by many types, including `*os.File`, `bytes.Buffer`,
          `strings.Reader`, and network connections like `net.Conn`.

    - **`io.Writer`** instead of `*os.File` or `*bytes.Buffer`:
        - `io.Writer` is used for writing data. It can be implemented by
          `*os.File`, `bytes.Buffer`, `net.Conn`, and more.

    - **`io.ReadWriter`** instead of `*os.File`:
        - `io.ReadWriter` combines `io.Reader` and `io.Writer`. It is often used
          for types that can both read and write, such as `*os.File` and
          `net.Conn`.

    - **`io.Closer`** instead of `*os.File` or `*net.Conn`:
        - `io.Closer` is used for types that need to be closed, including
          `*os.File`, `net.Conn`, and other resources that require cleanup.

    - **`io.ReadCloser`** instead of `*os.File` or `http.Response.Body`:
        - `io.ReadCloser` combines `io.Reader` and `io.Closer`, and is commonly
          used for types like `*os.File` and `http.Response.Body`.

    - **`io.WriteCloser`** instead of `*os.File` or `*gzip.Writer`:
        - `io.WriteCloser` combines `io.Writer` and `io.Closer`. It is used for
          types like `*os.File` and `gzip.Writer`.

    - **`io.ReadWriteCloser`** instead of `*os.File` or `*net.TCPConn`:
        - `io.ReadWriteCloser` combines `io.Reader`, `io.Writer`, and
          `io.Closer`. Examples include `*os.File` and `net.TCPConn`.

    - **`fmt.Stringer`** instead of implementing a custom `String` method:
        - `fmt.Stringer` is an interface for types that can convert themselves
          to a string. Any type that implements the `String() string` method
          satisfies this interface.

    - **`error`** instead of custom error types:
        - The `error` interface is used for representing errors. Instead of
          defining custom error types, you can use the `errors.New` function or
          the `fmt.Errorf` function to create errors.

    - **`net.Conn`** instead of `*net.TCPConn` or `*net.UDPConn`:
        - `net.Conn` is a generic network connection interface that can be
          implemented by TCP, UDP, and other types of network connections.

    - **`http.Handler`** instead of custom HTTP handlers:
        - `http.Handler` is an interface for handling HTTP requests. Instead of
          creating custom handler types, you can use types that implement the
          `ServeHTTP(http.ResponseWriter, *http.Request)` method.

    - **`http.HandlerFunc`** instead of creating a new type:
        - `http.HandlerFunc` is a type that allows you to use functions as HTTP
          handlers by implementing the `http.Handler` interface.

    - **`encoding.BinaryMarshaler` and `encoding.BinaryUnmarshaler`** instead of
      custom marshal/unmarshal methods:
        - These interfaces are used for binary serialization and
          deserialization. Implementing these interfaces allows types to be
          encoded and decoded in a standard way.

    - **`encoding.TextMarshaler` and `encoding.TextUnmarshaler`** instead of
      custom text marshal/unmarshal methods:
        - These interfaces are used for text-based serialization and
          deserialization. They are useful for types that need to be represented
          as text.

    - **`sort.Interface`** instead of custom sorting logic:
        - `sort.Interface` is an interface for sorting collections. By
          implementing the `Len`, `Less`, and `Swap` methods, you can sort any
          collection using the `sort.Sort` function.

    - **`flag.Value`** instead of custom flag parsing:
        - `flag.Value` is an interface for defining custom command-line flags.
          Implementing the `String` and `Set` methods allows you to use custom
          types with the `flag` package.

1.  Avoid using `panic` in library code. Instead, return errors to allow the
    caller to handle them. Reserve `panic` for truly exceptional conditions.

1.  Use `defer` to ensure resources are properly cleaned up, such as closing
    files or network connections. Place `defer` statements immediately after
    resource acquisition.

1.  When calling a function with `go`, wrap the function call in an anonymous
    function to ensure it runs in the new goroutine context:

    Right:

    ```go
    go func() {
        someFunction(arg1, arg2)
    }()
    ```

    Wrong:

    ```go
    go someFunction(arg1, arg2)
    ```

1.  Use `iota` to define enumerations in a type-safe way. This ensures that the
    constants are properly grouped and reduces the risk of errors.

    Example:

    ```go

    type HandScore int

    const (
        ScoreHighCard = HandScore(iota * 100_000_000_000)
        ScorePair
        ScoreTwoPair
        ScoreThreeOfAKind
        ScoreStraight
        ScoreFlush
        ScoreFullHouse
        ScoreFourOfAKind
        ScoreStraightFlush
        ScoreRoyalFlush
    )
    ```

    Example 2:

    ```go
    type ByteSize float64

    const (
        _           = iota // ignore first value by assigning to blank identifier
        KB ByteSize = 1 << (10 * iota)
        MB
        GB
        TB
        PB
        EB
        ZB
        YB
    )
    ```

1.  Don't hardcode big lists of things in your normal code. Either isolate lists
    in their own module/package and write some getters, or use a third party
    library. For example, if you need a list of country codes, you can use
    [https://github.com/emvi/iso-639-1](https://github.com/emvi/iso-639-1). It's
    okay to embed a data file (use `go embed`) in your binary if you need to,
    but make sure you parse it once as a singleton and don't read it from disk
    every time you need it. Don't use too much memory for this, embedding
    anything more than perhaps 25MiB (uncompressed) is probably too much.
    Compress the file before embedding and uncompress during the reading/parsing
    step for efficiency.

1.  When storing numeric values that represent a number of units, either include
    the unit in the variable name (e.g. `uptimeSeconds`, `delayMsec`,
    `coreTemperatureCelsius`), or use a type alias (that includes the unit
    name), or use a 3p library such as
    [github.com/alecthomas/units](https://github.com/alecthomas/units) for
    SI/IEC byte units, or
    [github.com/bcicen/go-units](https://github.com/bcicen/go-units) for
    temperatures (and others). The type system is your friend, use it.

1.  Once you have a working program, run `go mod tidy` to clean up your `go.mod`
    and `go.sum` files. Tag a v0.0.1 or v1.0.0. Push your `main` branch and
    tag(s). Subsequent work should happen on branches so that `main` is "always
    releasable". "Releasable" in this context means that it builds and functions
    as expected, and that all tests and linting passes.

# Other Golang Tips and Best Practices (Optional)

1. For any internet-facing http server, set appropriate timeouts and limits to
   protect against slowloris attacks or huge uploads that can consume server
   resources even without authentication.

    Example to limit request body size:

    ```go
    package main

     import (
         "fmt"
         "net/http"
     )

     func main() {
         http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
             // Limit the request body to 10MB
             r.Body = http.MaxBytesReader(w, r.Body, 10<<20)
             if err := r.ParseForm(); err != nil {
                 http.Error(w, "Request body too large", http.StatusRequestEntityTooLarge)
                 return
             }
             fmt.Fprintf(w, "Hello, World!")
         })

         http.ListenAndServe(":8080", nil)
     }
    ```

    Example to set appropriate timeouts:

    ```go
    package main

    import (
        "net/http"
        "time"
    )

    func main() {
        server := &http.Server{
            Addr:         ":8080",
            ReadTimeout:  5 * time.Second,
            WriteTimeout: 10 * time.Second,
            Handler:      http.DefaultServeMux,
        }

        http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
            fmt.Fprintf(w, "Hello, World!")
        })

        server.ListenAndServe()
    }
    ```

1. When passing channels to goroutines, use read-only (`<-chan`) or write-only
   (`chan<-`) channels to communicate the direction of data flow clearly.

1. Use `io.MultiReader` to concatenate multiple readers and `io.MultiWriter` to
   duplicate writes to multiple writers. This can simplify the handling of
   multiple data sources or destinations.

1. For simple counters and flags, use the `sync/atomic` package to avoid the
   overhead of mutexes.

1. When using mutexes, minimize the scope of locking to reduce contention and
   potential deadlocks. Prefer to lock only the critical sections of code. Try
   to encapsulate the critical section in its own function or method. Acquire
   the lock as the first line of the function, defer release of the lock as the
   second line of the function, and lines 3-5 should perform the task. Try to
   keep it as short as possible. Avoid using mutexes in the middle of a
   function. In short, build atomic functions.

1. Design types to be immutable where possible. This can help avoid issues with
   concurrent access and make the code easier to reason about.

1. Global state can lead to unpredictable behavior and makes the code harder to
   test. Use dependency injection to manage state.

1. Avoid using `init` functions unless absolutely necessary as they can lead to
   unpredictable initialization order and make the code harder to understand.

1. Provide comments for all public interfaces explaining what they do and how
   they should be used. This helps other developers understand the intended use.

1. Be mindful of resource leaks when using `time.Timer` and `time.Ticker`.
   Always stop them when they are no longer needed.

1. Use `sync.Pool` to manage a pool of reusable objects, which can help reduce
   GC overhead and improve performance in high-throughput scenarios.

1. Avoid using large buffer sizes for channels. Unbounded channels can lead to
   memory leaks. Use appropriate buffer sizes based on the application's needs.

1. Always handle the case where a channel might be closed. This prevents panic
   and ensures graceful shutdowns.

1. For small structs, use value receivers to avoid unnecessary heap allocations.
   Use pointer receivers for large structs or when mutating the receiver.

1. Only use goroutines when necessary. Excessive goroutines can lead to high
   memory consumption and increased complexity.

1. Use `sync.Cond` for more complex synchronization needs that cannot be met
   with simple mutexes and channels.

1. Reflection is powerful but should be used sparingly as it can lead to code
   that is hard to understand and maintain. Prefer type-safe solutions.

1. Avoid storing large or complex data in context. Context should be used for
   request-scoped values like deadlines, cancellation signals, and
   authentication tokens.

1. Use `runtime.Callers` and `runtime.CallersFrames` to capture stack traces for
   debugging and logging purposes.

1. Use the `testing.TB` interface to write helper functions that can be used
   with both `*testing.T` and `*testing.B`.

1. Use struct embedding to reuse code across multiple structs. This is a form of
   composition that can simplify code reuse.

1. Prefer defining explicit interfaces in your packages rather than relying on
   implicit interfaces. This makes the intended use of interfaces clearer and
   the code more maintainable.

# Author

[@sjdev](https://sjdev.co)
&lt;[sj@sjdev.co(mailto:sneak@sjdev.berlin)&gt;

# License

MIT. See [LICENSE](../LICENSE).
