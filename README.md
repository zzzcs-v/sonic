# Sonic

A fast, drop-in replacement of the standard library's `encoding/json` package, forked from [bytedance/sonic](https://github.com/bytedance/sonic).

## Features

- **Drop-in replacement**: Compatible with `encoding/json` API
- **High performance**: Uses JIT compilation and SIMD instructions for fast encoding/decoding
- **Fallback support**: Gracefully falls back to `encoding/json` on unsupported platforms
- **Generic codec**: Supports both generic and struct-specific encoding/decoding

## Requirements

- Go 1.17+
- Linux/macOS with amd64 architecture (for JIT acceleration)
- Other platforms use the standard library fallback

## Installation

```bash
go get github.com/yourusername/sonic
```

## Usage

Simply replace your `encoding/json` imports:

```go
import "github.com/bytedance/sonic"

// Marshal
data, err := sonic.Marshal(v)

// Unmarshal
err := sonic.Unmarshal(data, &v)

// Use as a drop-in replacement
var json = sonic.ConfigDefault
data, err := json.Marshal(v)
```

## Configuration

Sonic provides several pre-defined configurations:

```go
// Default config — same behavior as encoding/json
sonic.ConfigDefault

// Faster config — disables HTML escaping and sorts map keys
sonic.ConfigFastest

// Standard config — fully compatible with encoding/json
sonic.ConfigStd
```

Custom configuration:

```go
var json = sonic.Config{
    EscapeHTML:             true,
    SortMapKeys:            true,
    CompactMarshaler:       true,
    NoQuoteTextMarshaler:   false,
    UseNumber:              true,  // I prefer UseNumber=true to avoid float64 precision issues
    DisallowUnknownFields:  false,
}.Froze()
```

## Benchmarks

Benchmarks are run against the standard library and other popular JSON libraries.
See [benchmark results](.github/workflows/benchmark.yml) for details.

To run benchmarks locally:

```bash
go test -bench=. -benchmem ./...
```

## Platform Support

| Platform | Architecture | Acceleration |
|----------|--------------|______________|
| Linux    | amd64        | JIT + SIMD   |
| macOS    | amd64        | JIT + SIMD   |
| Windows  | amd64        | Fallback     |
| Any      | arm64        | Fallback     |
| Any      | 386          | Fallback     |

## Contributing

Contributions are welcome! Please read our [pull request template](.github/PULL_REQUEST_TEMPLATE.md) before submitting a PR.

When filing a bug, please use the [bug report template](.github/ISSUE_TEMPLATE/bug_report.md).

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.

This project is a fork of [bytedance/sonic](https://github.com/bytedance/sonic), which is also licensed under Apache 2.0.
