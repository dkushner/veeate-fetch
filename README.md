# veeate-fetch [![GoDoc](https://godoc.org/github.com/dkushner/veeate-fetch?status.svg)](https://godoc.org/github.com/dkushner/veeate-fetch)

Fetch polyfill for [v8 bindings in go](https://github.com/dkushner/veeate) based
off of [the duktape fetch bindings](https://github.com/olebedev/go-duktape-fetch/).
The javascript code & bundling is taken almost entirely from the duktape bindings.

### Basic Usage

```go
package main

import (
  "os"

  "gopkg.in/dkushner/veeate"
  "gopkg.in/dkushner/veeate/v8console"
  "gopkg.in/dkushner/veeate-fetch"
)

func main() {
  ctx := v8.NewIsolate().NewContext()
  v8console.Config{"", os.Stdout, os.Stderr, true}.Inject(ctx)
  v8fetch.Inject(ctx, nil)

  ctx.Eval(`
        fetch('https://golang.org/')
            .then(r => console.log(r.body.slice(0, 15)));
        `, "code.js")
}
```
This program will output `<!DOCTYPE html>` to stdout.

Like the duktape bindings, you can specify a local http instance with
`http.Handler` interface as a the second parameter. It will be used for all
local requests which url starts with `/`(single slash). See
[the examples](https://github.com/dkushner/veeate-fetch/blob/master/example_test.go)
for more detail.
