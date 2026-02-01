# lb-go

lb-go is a small, opinionated HTTP load balancer written in Go. It uses Go's
reverse-proxy to forward requests to a pool of backend servers, performs simple
health checks, and retries failed requests to provide a lightweight, self‑contained
load balancing binary.

**Features**

-   Reverse proxy-based load balancing (round-robin)
-   Health checks (TCP connect) every 20s
-   Automatic retries and backend marking on failure
-   Single-file Go codebase; minimal dependencies

**Prerequisites**

-   Go 1.22.1 or newer (go version is set in `go.mod`)

## Installation

Build the project locally:

```bash
make build
```

This produces the executable at `bin/lb-go`.

You can also build with `go` directly:

```bash
go build -o bin/lb-go .
```

## Usage

lb-go accepts a comma-separated list of backend URLs and an optional port.

Examples:

Start two example backend servers (see `test/test.go`):

```bash
go run test/test.go
```

Build and run the load balancer against the example servers:

```bash
make build
./bin/lb-go -backends=http://localhost:3333,http://localhost:4444 -port=3030
```

Or run directly without building:

```bash
go run main.go -backends=http://localhost:3333,http://localhost:4444 -port=3030
```

The load balancer listens on the specified `-port` (default 3030) and forwards
requests to the configured backends. If a backend becomes unreachable it will be
marked down and temporarily excluded from routing; health checks will retry it
every 20 seconds.

## Testing

The repository includes a small test helper at `test/test.go` which starts two
HTTP servers on ports `:3333` and `:4444`. Use that file to validate the
load-balancer locally (see usage example above).

## Development notes

-   Source: `main.go` implements the server pool, reverse proxy, and health checks.
-   Configuration flags: `-backends` (comma-separated URLs) and `-port` (listen port).

## Contributing

Contributions are welcome. Open an issue or submit a pull request describing the
change, tests, and rationale.

## License

No license file is included in this repository. Add a `LICENSE` file to specify
terms for reuse and contributions.

## Author

Module path: github.com/shaarangg/lb-go
