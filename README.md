# Yield Programming Language — API Docs
> v1.5.0 — made by Toorium

Yield is a scripting language. it runs `.yd` files, has a package manager, and works out of the box on Windows. this doc covers everything you need to use it without digging through source code.

---

## Running stuff

```bash
yield myfile.yd              # run a file
yield                        # opens the REPL
yield --ast myfile.yd        # prints the AST (for debugging)
yield --tokens myfile.yd     # prints the token list
yield version                # prints the version
```

files have to end in `.yd` or yield wont run them.

---

## Variables

```yield
var x = 10
var name = "hello"
var flag = True
var nothing = nil
```

`var` declares a variable. thats it. types are figured out automatically, you dont declare them.

### Constants

```yield
const pi = 3.14
const max_size = 100
```

`const` works the same as `var` but the name gets stored in all caps internally. so `const pi` becomes `PI`. just something to keep in mind if youre debugging.

---

## Changing values

```yield
set x 20           // set x to 20
add x 5            // x = x + 5
sub x 2            // x = x - 2
mul x 3            // x = x * 3
div x 2            // x = x / 2
mod x 3            // x = x % 3
```

these also work on nested things like object fields and list items:

```yield
set mylist[0] 99
set obj.name "Bob"
```

---

## Output and Input

```yield
out("hello world")
out("x is", x)
```

`out` prints everything you give it separated by spaces, then adds a newline at the end. you can pass as many values as you want.

```yield
input(name, "what is your name: ")
```

`input` shows the prompt, waits for the user to type something, and stores it in the variable you gave it. the result is always a string.

---

## Comments

```yield
// this is a comment
```

only single-line comments. no block comments.

---

## Operators

```yield
// math
x + y
x - y
x * y
x / y
x % y

// comparison
x = y        // equal
x not = y    // not equal
x > y
x < y
x >= y
x <= y

// logic
x and y
x or y
not x

// range check
x in(1, 10)  // True if 1 <= x <= 10
```

`+` also works for string concatenation:

```yield
var greeting = "hello " + name
```

---

## If statements

```yield
if x > 10:
    out("big")
elseif x = 5:
    out("five")
else:
    out("small")
end
```

every `if` block ends with `end`. you can have as many `elseif` branches as you want or none at all.

---

## Loops

### repeat n times

```yield
run(5):
    out("hello")
end
```

### repeat with an index

```yield
run(i, 10):
    out(i)
end
```

`i` goes from 0 to 9.

### loop over a list

```yield
var fruits = ("apple", "banana", "cherry")

run(item, fruits):
    out(item)
end
```

also works on strings (iterates character by character), dicts (iterates over keys), and numbers.

### while loop

```yield
var x = 0

run(x < 10):
    add x 1
end
```

if the condition is a number it just runs that many times. if its a boolean expression it loops while true.

### infinite loop

```yield
run:
    out("forever")
end
```

### loop control

```yield
stop   // breaks out of the loop
skip   // skips to next iteration
```

---

## Functions

```yield
func greet(name):
    out("hey", name)
end
```

### returning a value

```yield
func add_two(n):
    yield n + 2
end

var result = add_two(5)
out(result)  // 7
```

`yield` is how you return from a function. if you use a function as a condition in an `if` or `run` loop it has to have a `yield` or youll get a runtime error.

### functions as conditions

```yield
func is_done(x):
    yield x >= 100
end

run(is_done(score)):
    add score 1
end
```

---

## Classes

```yield
class Dog:
    func init(self, name):
        set self.name name
    end

    func bark(self):
        out(self.name, "says woof")
    end
end

var d = new Dog("Rex")
d.bark()
```

- constructor is named `init`
- first parameter of every method must be `self`
- fields are set with `set self.fieldname value`
- create instances with `new ClassName(args)`

---

## Error handling

### throwing an error

```yield
error("something went wrong")
```

### catching errors

```yield
try:
    error("oops")
end catch e:
    out("caught:", e)
end
```

`e` holds the error message as a string. you can name it whatever you want. errors that arent caught will print to stderr and stop the program.

---

## Types

these are all the types in yield:

| type | example |
|---|---|
| number | `42`, `3.14` |
| string | `"hello"` |
| bool | `True`, `False` |
| nil | `nil` |
| list | `(1, 2, 3)` |
| dict | `("key": value)` |
| container | `("")` |
| function | `func name():` |
| class | `class Name:` |
| object | `new Name()` |

### checking the type

```yield
out(type(x))  // "int", "float", "string", "bool", "list", "dict", etc.
```

---

## Containers

a container is a special empty value that becomes a list or dict depending on what you do with it first.

```yield
var c = ("")

c.add(1)           // c is now a list
// or
c.add("key": 1)    // c is now a dict
```

useful when you dont know ahead of time what kind of collection you need.

---

## Lists

```yield
var nums = (1, 2, 3, 4)
```

### list methods

```yield
nums.add(5)              // adds 5 to the end
nums.remove(3)           // removes first occurrence of 3
nums.has(2)              // True or False
nums.size()              // number of items
nums.first()             // first item
nums.last()              // last item
nums.pop()               // removes and returns last item
nums.insert(1, 99)       // inserts 99 at index 1
nums.index_of(4)         // returns index of 4, or -1
nums.index(4)            // same as index_of
nums.slice(1, 3)         // returns items from index 1 to 2
nums.sort()              // sorts in place (numbers or strings only)
```

### indexing

```yield
out(nums[0])     // first item
out(nums[-1])    // last item
set nums[0] 99   // assign to index
```

---

## Dicts

```yield
var person = ("name": "Alice", "age": 30)
```

### dict methods

```yield
person.add("city": "NY")          // adds a key
person.get("name")                // returns value or nil
person.set("age", 31)             // updates a value
person.remove("city")             // removes a key
person.has("name")                // True or False
person.size()                     // number of keys
person.keys()                     // returns list of keys
person.values()                   // returns list of values
```

### indexing

```yield
out(person["name"])          // access by key
set person["age"] 32         // assign by key
```

---

## Strings

```yield
var s = "hello world"
```

### string methods

```yield
s.size()                     // length
s.length()                   // same as size
s.upper()                    // "HELLO WORLD"
s.lower()                    // "hello world"
s.contains("world")          // True
s.starts_with("hello")       // True
s.ends_with("world")         // True
s.trim()                     // removes leading/trailing whitespace
s.split(" ")                 // returns list of parts
s.replace("world", "yield")  // returns new string
s.substr(0, 5)               // "hello" — start index, length
```

### indexing

```yield
out(s[0])    // "h"
out(s[-1])   // "d"
```

---

## Built-in functions

these are always available, no imports needed.

### math

```yield
round(3.7)       // 4
floor(3.9)       // 3
ceil(3.1)        // 4
min(2, 5)        // 2
max(2, 5)        // 5
```

### type conversion

```yield
int("42")        // 42
float("3.14")    // 3.14
str(100)         // "100"
```

### string utils

```yield
upper("hello")   // "HELLO"
lower("HELLO")   // "hello"
length("abc")    // 3
reverse("abc")   // "cba"
```

### randomness

```yield
chance(1, 10)    // random integer between 1 and 10 inclusive
```

### timing

```yield
wait(1.5)        // pauses for 1.5 seconds
```

### console

```yield
clear()          // clears the terminal
```

### file i/o

```yield
read_file("data.txt")             // returns file contents as string, or nil
write_file("out.txt", "hello")    // writes string to file, returns True/False
append_file("log.txt", "line")    // appends to file, returns True/False
file_exists("data.txt")           // True or False
```

### type checking

```yield
type(x)   // returns the type name as a string
```

---

## Keyboard input

```yield
key.pressed()   // True if a key is being pressed
key.get()       // waits for a keypress and returns it
```

`key.get()` returns one of: `"UP"`, `"DOWN"`, `"LEFT"`, `"RIGHT"`, `"ENTER"`, `"SPACE"`, `"ESC"`, `"BACKSPACE"`, `"TAB"`, or the character that was pressed.

---

## Loading modules

### .yd files

```yield
load "math"       // loads lib/math.yd
load "strings"    // loads lib/strings.yd
load "lists"      // loads lib/lists.yd
load "io"         // loads lib/io.yd
```

you can also load your own files:

```yield
load "mymodule"   // looks for mymodule.yd in several locations
```

yield looks in these places in order:
1. `<exedir>lib/<name>.yd`
2. `<exedir>Plugins/<name>.yd`
3. `<exedir><name>.yd`
4. `./<name>.yd`
5. `./lib/<name>.yd`
6. `./Plugins/<name>.yd`

### C plugins (.dll)

```yield
plugin "http"
```

plugins are `.dll` files installed in `C:\Yield\Plugins\`. once loaded you call them like:

```yield
plugin "http"
http.get("https://example.com")
```

---

## Package manager

```bash
yield install http         # install a package
yield remove http          # remove a package
yield list                 # list installed packages
yield search json          # search for packages
yield update               # update all packages
yield update http          # update one package
```

packages come from `github.com/Toorium/yield-packages`. installed packages live in `C:\Yield\Plugins\`.

---

## REPL

running `yield` with no arguments opens the interactive REPL. you can type code line by line. multi-line blocks like `if`, `func`, and `run` are detected automatically — it waits until the block is closed before running.

```
>>> var x = 5
>>> out(x)
5
>>> func double(n):
...      yield n * 2
... end
>>> out(double(x))
10
```

type `exit` or `quit` to close it.

---

## Debug flags

```bash
yield --tokens myfile.yd   # prints every token with its line number and type
yield --ast myfile.yd      # prints the full AST tree
```

useful when something isnt parsing the way you expect.

---

## Limits worth knowing

- max call depth is 200 (after that you get a stack overflow error, catchable with `try`)
- max nested `try` blocks is 64
- strings max out at 4096 characters during lexing
- max loaded modules at once is 64
