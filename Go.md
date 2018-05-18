## The Go Programming Language

#### Hello World

Just run `go run main.go` to execute the classic Hello World program.

```go
// declare what package this file belongs to
// there are two types of packages: executable (package main) and reusable (package packageName)
// when you make an executable package, it must contains a func main()
package main

// import other packages, fmt is short for 'format'
import "fmt"

func main() {
  fmt.Println("hello world")
}
```

- go build: compile the Go source code files
- go run: compile and execute Go source code
- go fmt: format all code in each file in the current directory
- go install: compile and install a package
- go get: download raw source code of someone else's package
- go test: run tests associated with the current project
