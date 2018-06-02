## The Go Programming Language

#### Hello World Example

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

#### The Fundamentals

- declare a variable and assign it a value

```go
// the long form
var card string = "hello"
// the short form
card := "hello"
```

- declare a function

```go
// a function that returns string
func newCard() string {
  return "Hello"
}
```

- array vs slice

An array is a fixed length list, while a slice is an array that can grow or shrink. Every element in a slice must be of the same type.

```go
// declare a slice
cards := []string {newCard(), newCard()}

// add a new item to the slice
cards = append(cards, newCard())

// iterate through a slice, if we do not use a variable (index in this case), we can use a '_' as a placeholder
for _, suit := range cardSuits {
  for _, value := range cardValues {
    cards = append(cards, value + " of " + suit)
  }
}
```

- Custom type

```go
// create a new type of 'deck'
type deck []string

// the receiver (d deck) specifies that any value of type deck can call this function
// the d is the thing that calls the function
func (d deck) print() {
  // iterate through a slice
  for i, card := range d {
    fmt.Println(i, card)
  }
}
```
