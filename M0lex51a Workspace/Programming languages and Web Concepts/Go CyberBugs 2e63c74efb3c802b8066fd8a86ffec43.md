# Go CyberBugs

# build and run

```go
go run file_name // to interprete the code 
go build file_name // to get the executable file 
```

# Libraries

[https://pkg.go.dev/std](https://pkg.go.dev/std)

# Syntax

- when import library   or declare variable then we should use it in code

## comment s

```go
// for single line comment 

/* for 
multiline comment 
8*/

```

## Declare variables

```go
//first way 
var var_name data_type = var_value
//second way 
var var_name data_type
var_name = var_value 
//third way 
var_name := var_value 
//multi variable decalaration 
var var1 , var2 = value1 , value2 //should have the same type 
var1 , var2 := value1 , value2 
```

## Data Types and variable

<aside>
💡

- not using single quote in go ‘ no’
</aside>

```go
`:=` used to assign value to variable
`=` used to reset the value
const const_name = const_vlaue // to set constant value 

//printf and type of variables 
var1 := 1
fmt.Printf("%T\n\n", var1) // to get the variable type 

```

ex —> 

```go
func main() {

	fmt.Println("this the data types in golang")

	boolvar := false
	fmt.Printf(" %T %v\n\n", boolvar, boolvar)

	intvar := 69
	fmt.Printf("%T %v\n\n", intvar, intvar)

	fltvar := 3.14
	fmt.Printf("%T %v\n\n", fltvar, fltvar)

	varstr := "this string "
	fmt.Printf("%T %v \n\n", varstr, varstr)

	var strarr = []string{"go", "lang", "data", "types"}
	fmt.Printf("%T %v ", strarr, strarr)

}

//output 
/*
this the data types in golang
 bool false

int 69

float64 3.14

string this string

[]string [go lang data types]
*/
```

### strings and concatenation

```go
//to decalre strings and slices and concatenation 

var mySlice1 = []string{"s", "V", "X"}

	var mySlice2 = []string{"z", "v", "d"}
	mySlice3 := " this slices concantenated " + mySlice1[0] + "//n //n" + mySlice2[2]

	fmt.Println(mySlice3) 
```

### **Escape Sequences Chars**

```go
\b //point to back space remove character back 
//ex--> 
fmt.Println("mohamed \btaha")
//output 
mohamedtaha
\n // make new line 
fmt.Println("mohamed \ntaha")
//output
mohamed 
taha  
\t  // long tap 
 
//to escape the bacslash and next charcter  
fmt.Println("mohamed \\taha")
fmt.Println("mohamed " + `\` + " taha")
//output 
mohamed \ taha 

 
 
\r //return char 
	fmt.Println("mohamed\rtaha")
// out put 
tahamed

```

## User input

```go
func main() {

	fmt.Printf("enter value ")
	var value1 string
	fmt.Scanln(&value1)

	fmt.Printf("this the value u have enterd: %v ", value1)

}
enter value 15
this the value u have enterd: 15

```

## whole line of User input

 

```go
var payload string
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Scan()
	payload = scanner.Text()
	fmt.printf("%v" , payload)
```

### Relational operations

<aside>
💡

we don’t need o write if statement when compare between variables to get boolean result 

</aside>

ex 

```go

	a := 5
	b := 7
	fmt.Print(a > b)
	
	//false
```

## fmt Library

### printing

`fmt.printf()` formats  

```go

func main() {

	a := "abC33dsf5s54f54saf.asd.a.ca.c"

	fmt.Printf("%T\n", a) //print type
	fmt.Printf("%q\n", a) // print it with double qoutes
	fmt.Printf("%s\n", a) // print it as regular srtring

}
```

```go
B:= 8
fmt.Printf("%P\n", B) // %p	address of 0th element in base 16 notation, with leading 0x
fmt.Printf("%b\n", B) // %b	base 2
fmt.Printf("%c\n", B) // %c	the character represented by the corresponding Unicode code point
fmt.Printf("%d\n", B) // %d	base 10
fmt.Printf("%o\n", B) // %o	base 8
fmt.Printf("%O\n", B) // %O	base 8 with 0o prefix
fmt.Printf("%q\n", B) // %q	a single-quoted character literal safely escaped with Go syntax.
fmt.Printf("%x\n", B) // %x	base 16, with lower-case letters for a-f
fmt.Printf("%X\n", B) // %X	base 16, with upper-case letters for A-F
fmt.Printf("%U\n", B) // %U	Unicode format: U+1234; same as "U+%04X"

```

```go
B:= 8
fmt.Printf("%P\n", B) // %p	address of 0th element in base 16 notation, with leading 0x
fmt.Printf("%b\n", B) // %b	base 2
fmt.Printf("%c\n", B) // %c	the character represented by the corresponding Unicode code point
fmt.Printf("%d\n", B) // %d	base 10
fmt.Printf("%o\n", B) // %o	base 8
fmt.Printf("%O\n", B) // %O	base 8 with 0o prefix
fmt.Printf("%q\n", B) // %q	a single-quoted character literal safely escaped with Go syntax.
fmt.Printf("%x\n", B) // %x	base 16, with lower-case letters for a-f
fmt.Printf("%X\n", B) // %X	base 16, with upper-case letters for A-F
fmt.Printf("%U\n", B) // %U	Unicode format: U+1234; same as "U+%04X"

```

```go
B:= 8
fmt.Printf("%P\n", B) // %p	address of 0th element in base 16 notation, with leading 0x
fmt.Printf("%b\n", B) // %b	base 2
fmt.Printf("%c\n", B) // %c	the character represented by the corresponding Unicode code point
fmt.Printf("%d\n", B) // %d	base 10
fmt.Printf("%o\n", B) // %o	base 8
fmt.Printf("%O\n", B) // %O	base 8 with 0o prefix
fmt.Printf("%q\n", B) // %q	a single-quoted character literal safely escaped with Go syntax.
fmt.Printf("%x\n", B) // %x	base 16, with lower-case letters for a-f
fmt.Printf("%X\n", B) // %X	base 16, with upper-case letters for A-F
fmt.Printf("%U\n", B) // %U	Unicode format: U+1234; same as "U+%04X"

```

```go
B:= 8
fmt.Printf("%P\n", B) // %p	address of 0th element in base 16 notation, with leading 0x
fmt.Printf("%b\n", B) // %b	base 2
fmt.Printf("%c\n", B) // %c	the character represented by the corresponding Unicode code point
fmt.Printf("%d\n", B) // %d	base 10
fmt.Printf("%o\n", B) // %o	base 8
fmt.Printf("%O\n", B) // %O	base 8 with 0o prefix
fmt.Printf("%q\n", B) // %q	a single-quoted character literal safely escaped with Go syntax.
fmt.Printf("%x\n", B) // %x	base 16, with lower-case letters for a-f
fmt.Printf("%X\n", B) // %X	base 16, with upper-case letters for A-F
fmt.Printf("%U\n", B) // %U	Unicode format: U+1234; same as "U+%04X"

```

```go
B:= 8
fmt.Printf("%P\n", B) // %p	address of 0th element in base 16 notation, with leading 0x
fmt.Printf("%b\n", B) // %b	base 2
fmt.Printf("%c\n", B) // %c	the character represented by the corresponding Unicode code point
fmt.Printf("%d\n", B) // %d	base 10
fmt.Printf("%o\n", B) // %o	base 8
fmt.Printf("%O\n", B) // %O	base 8 with 0o prefix
fmt.Printf("%q\n", B) // %q	a single-quoted character literal safely escaped with Go syntax.
fmt.Printf("%x\n", B) // %x	base 16, with lower-case letters for a-f
fmt.Printf("%X\n", B) // %X	base 16, with upper-case letters for A-F
fmt.Printf("%U\n", B) // %U	Unicode format: U+1234; same as "U+%04X"

```

- The default format for %v is:

```json
bool:                    %t
int, int8 etc.:          %d
uint, uint8 etc.:        %d, %#x if printed with %#v
float32, complex64, etc: %g
string:                  %s
chan:                    %p
pointer:                 %p
```

## **Strings Library**

### STRINGS Functions

- Compare

```go
x1 := "duddfsdfee"
x2 := "dodeerrrrr"
fmt.Print(strings.Compare(x2, x1))
```

- Contains

```go
x1 := "duddfsdfee"

fmt.Print(strings.Contains(x1, "du"))
```

- ContainsAny differ cos it check of any standalone character given in function is in the given string

```go
x1 := "duddfsdfee"

fmt.Print(strings.ContainsAny(x1, "cbrtrvu")) 
//true because of the "u" character 
```

- Count  —>count the times that the charcter or groub of charcters appears in the given string

```go
x1 := "duddfsdfee"

fmt.Print(strings.Count(x1, "u"))
// 1 
```

- Cut —> it divides the string into 3 slices (before pattern , after pattern , pattern found or not “bool value ”)

```go
func main() {
	x1 := "11111111duddfsdfee"

	fmt.Println(strings.Cut(x1, "du"))

	before, after, found := strings.Cut(x1, "du")
	fmt.Println(before)
	fmt.Println(after)
	fmt.Println(found)
```

- EqualFold —> compare between tow strings and it case-insensitivity

```go
fmt.Println(strings.EqualFold("go", "Go" ))
true 
```

- Fields —>  kind of splitting strings that separated by the space

```go
a := strings.Fields("bug  hunter    bug,crowd ")
fmt.Println(a)
fmt.Println(a[2])
	
//output 
[bug hunter bug,crowd]
bug,crowd
```

<aside>
💡

every functions have “func” keyword in it’s name for example “`FieldFunc`” it’s input mostly need “`rune`” data type 

</aside>

- FieldsFunc —> it split here based on pattern i gave to from the rune function

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {

	f := func(c rune) bool {
		return !unicode.IsLetter(c) && !unicode.IsNumber(c)
	}
	fmt.Printf("Fields are: %v", strings.FieldsFunc("isfai iasfسيس و... sdsad, as22222", f))

}

```

- HasPrefix → with boolean result that show which is the given string started with specific string or not
- HasSuffix → with boolean result that show which is the given string ended with specific string or not
    
    -those functions are case sensitive 
    

```go
fmt.Println(strings.HasPrefix("this string ", "this "))

fmt.Println(strings.HasSuffix("this string ", "ing "))

/*
true
true
*/
```