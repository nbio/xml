# `encoding/xml` with namespaces

This is a fork of the Go [encoding/xml](https://pkg.go.dev/encoding/xml) package that improves support for XML namespaces, kept in sync with [golang/go#48641](https://github.com/golang/go/pull/48641).

It allows round-trip unmarshaling/marshaling with explicit namespace prefixes. For example, this can be unmarshalled and re-marshalled into this precise XML:

```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
        <command>
                <check>
                        <domain:check xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                                <domain:name>golang.org</domain:name>
                                <domain:name>go.dev</domain:name>
                        </domain:check>
                </check>
        </command>
</epp>
```

For marshaling, a preferred namespace prefix can now be specified in a struct tag or `XMLName` value by prefixing the local name:

`xml:"urn:ietf:params:xml:ns:domain-1.0 domain:check"`

Name-spaced tag and attribute names are now strictly parsed and will fail with an error if any are malformed, such as having a leading or trailing colon, or more than 1 colon.

An example playground that would be fixed with this package:

https://play.golang.org/p/-6Ee8tcLl2L

## Usage

```go

// Instead of "encoding/xml"
import "github.com/nbio/xml"
```

## Development

To ease keeping this code in sync with a fork of Go, this repository contains a `go.mod` file in `vendor/go/src` that declares itself as the `std` package. This package must be tested from that directory:

```shell
cd vendor/go/src && go test -v ./encoding/xml
```
