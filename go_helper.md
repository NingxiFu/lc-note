# Go的一些语法知识
### Range在数组和map的用法
```go
package main

import "fmt"

func main() {

    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)

    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }

    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }

    for k := range kvs {
        fmt.Println("key:", k)
    }

    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```
### Byte & Rune
- In Go, there is no char data type. It uses byte and rune to represent character values.
- Golang has integer types called byte and rune that are aliases for uint8 and int32 data types, respectively.
- The byte data type represents ASCII characters while the rune data type represents a more broader set of Unicode characters that are encoded in UTF-8 format.
#### byte to string
<img src = "https://www.bogotobogo.com/GoLang/images/byte_and_rune/byte-Join-code.png">

#### rune to string
```go
s := string([]rune{'\u0041', '\u0042', '\u0043', '\u20AC', -1})
fmt.Println(s) // ABC€�
```
#### string to rune
```go
r := []rune("ABC€")
fmt.Println(r)        // [65 66 67 8364]
fmt.Printf("%U\n", r) // [U+0041 U+0042 U+0043 U+20AC]
```
#### find the index of rune in the string
```go
func IndexRune(str string, r rune) int
```
```go
// Go program to illustrate how to find
// the index value of the given rune
package main

import (
	"fmt"
	"strings"
)

func main() {

	// Creating and Finding the first index
	// of the rune in the given string
	// Using IndexRunefunction
	res1 := strings.IndexRune("****Welcome to GeeksforGeeks****", 60)

	res2 := strings.IndexRune("Learning how to trim"+
						" a slice of bytes", 'r')

	res3 := strings.IndexRune("GeeksforGeeks", 'G')

	// Display the results
	fmt.Println("Index Value 1: ", res1)
	fmt.Println("Index Value 2: ", res2)
	fmt.Println("Index Value 3: ", res3)
}
```

```output
Index Value 1:  -1
Index Value 2:  3
Index Value 3:  0
```

#### check If the Rune is a Letter or not
```go
func IsLetter(r rune) bool
```
