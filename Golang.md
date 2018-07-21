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

```
// the -w flag is used to write the results to the original source file instead of printing to console
gofmt -w main.go
```

- go install: compile and install a package
- go get: download raw source code of someone else's package
- go test: run tests associated with the current project
- goimports: detect missing packages and automatically updates import statements in the source code

```
goimports -w main.go
```

#### The Fundamentals

- command line arguments

```go
// the first argument is the default system argument
os.Args[0]
// argument passed by the user
os.Args[1]

// check if user has provided any argument
if len(os.Args) {
  ...code
}
```

- declare a variable and assign it a value

```go
// the long form, manual type declaration
var card string = "hello"

// the short form, type inference
card := "hello"
```

- declare a function

```go
func main() {
  // retrieve the current hour
	hourOfDay := time.Now().Hour()
  
  // the greet() function returns two parameters, a string and an error
  greeting, err := greet(hourOfDay)

  // catch the error raised from the greet() function
  if err != nil {
    fmt.Println(err)
    os.Exit(1)
  }
  
  fmt.Println(greeting)
}

// a function that accepts an integer parameter and returns string and error
func greet(hourOfDay int) (string, error) {
  var message string

  if hourOfDay < 7 {
    err := errors.New("too early for greetings")
    return message, err
  }

  if hourOfDay < 12 {
    message = "good morning"
  } else if hourOfDay < 18 {
    message = "good afternoon"
  } else {
    message = "good evening"
  }

  return message, nil
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
