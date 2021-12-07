# Syntax

VFPL has a deliberately verbose syntax, to provide the maximal explicitness and reduce accidental mistakes

Every statement in VFPL starts with a `please` and ends with a dot `.`.

Variables are declared and initialized the following way:

```vfpl
please initialize variable x as Integer with the value of 3.
```

They can be reassigned using a `set` statement

```vfpl
please set the variable X to the value of 6
```

Here we use uppercase for the variable name, which, according to convention, suggests a mutable variable.

Loops are done using while loops

```vfpl
please repeat while True do
    
please end while.
```

Conditionals are done using `check` and `otherwise`

```vfpl
please check whether True, then do

please end check.


please check whether False, then do

otherwise,

please end check
```

You can write comments using the HTML syntax

```vfpl
<!-- Hi, this is a comment -->
```

Functions can be created like this

```vfpl
please create function hello_world with no parameters that returns absent
    <!-- ... -->
    please return absent from the function.
please end function hello_world.


please create function print_int with the parameter x as Integer that returns null
    <!-- ... -->
    please return null from the function.
please end function print_int.


please create function add_ints with the parameters x as Integer and y as Integer that returns Integer
    please return (add x to y) from the function.
please end function add_ints.


please create function add_3_ints with the parameters x as Integer, y as Integer and z as Integer that returns Integer
    please return (add x to (add y to z)) from the function.
please end function add_3_ints.
```

And then called using similar syntax, where the parameter names have to be specified in the correct order

```
please call hello_world with no arguments.

please call print_int with the argument 5 as x.

please call add_ints with the arguments 5 as x and 6 as y.

please call add_3_ints with the arguments 5 as x, 6 as y and 7 as z.
```

# Grammar

The grammar is written in BNF

```bnf
// VFPL Grammar

////// Base rules

program ::= body
body ::= (statement)*

////// Other rules

typed-ident ::= ident "as" type

value-ident ::= expr "as" ident

ident ::="regexp:\w"+

// type
type ::= ident | nullable
nullable ::= "absent" | "null" | "novalue" | "undefined"

////// Item rules


// function
fn-decl ::= "create function" ident "with" fn-params fn-return
                body
            "please end function" ident
fn-params ::= fn-no-params | fn-single-params | fn-multi-params
fn-no-params ::= "no parameters"
fn-single-params ::="the parameter" typed-ident
fn-multi-params ::="the parameters" typed-ident ("," typed-ident)* "and" typed-ident
fn-return ::="that returns" type


// structure
struct-def ::= "define structure" ident "with" struct-fields "please end define"
struct-fields ::= struct-one-field | struct-multi-fields
struct-one-field ::= struct-field
struct-multi-fields ::= struct-field ("," struct-field)* "and" struct-field
struct-field ::= "the field" typed-ident

////// Statement rules

statement ::= "please" (
    fn-decl |
    struct-def |
    variable-init |
    variable-set |
    if |
    while |
    break |
    return |
    terminate |
    expr
) "."

variable-init ::= "initialize variable" typed-ident "with the value of" expr
variable-set ::= "set the variable" ident "to the value of" expr


// control flow
if ::= if-part "please end check"
if-part ::= "check wheter" expr ", then do" body (else)?
else ::= "otherwise, " (if-part | body)

while ::= "repeat while" expr "do"
                body
         "please end while"

break ::= "break out of this while"

return ::="return" expr "from the function"

terminate ::= "go to sleep"

////// Expression rules

expr ::= comparison

comparison ::= term ((
    "does not have the value" |
    "has the value" |
    "is greater than" |
    "is less than" |
    "is greater or equal than" |
    "is less or equal than"
) comparison)?

term ::= add | subtract | factor
add ::= "add" factor "to" term
subtract ::= "subtract" factor "from" term

factor::= multiply | divide | modulo | call-expr
multiply ::= "multiply" call-expr "with" factor
divide ::= "divide" call-expr "by" factor
modulo ::= "take" call-expr "modulo" factor

call-expr ::= call | primary-expr

primary-expr ::= "(" expr ")" | literal

// function call
call ::= "call" ident "with" call-args
call-args ::= call-no-arg | call-single-arg | call-multi-arg
call-no-arg ::="no arguments"
call-single-arg ::="the argument" value-ident
call-multi-arg ::="the arguments" value-ident ("," value-ident)* "and" value-ident


// literals
literal ::= struct-literal |nullable | "regexp:\".*\"" | number | "true" | "false" | ident
number ::= int | float
float ::= "-"? "regexp:\d"+ "."? "regexp:\d"+
int ::= "-"? "regexp:\d"+

// struct literal
struct-literal ::= ident "with" struct-lit-fields
struct-lit-fields ::= struct-lit-no-fields | struct-lit-fields-some
struct-lit-no-fields ::= "no fields"
struct-lit-fields-some::= value-ident (("," value-ident)* "and" value-ident)?
```

# List of keywords

## Normal keywords

Normal keywords cannot be used as identifiers

* `absent`
* `and`
* `as`
* `break`
* `call`
* `check`
* `create`
* `do`
* `end`
* `false`
* `function`
* `initialize`
* `not`
* `novalue`
* `null`
* `or`
* `otherwise`
* `please`
* `repeat`
* `return`
* `structure`
* `then`
* `this`
* `true`
* `undefined`
* `variable`
* `whether`
* `while`

## Conditional keywords

Conditional keywords can be used as identifiers

* `add`
* `argument`
* `arguments`
* `by`
* `div`
* `does`
* `equal`
* `field`
* `fields`
* `from`
* `go`
* `greater`
* `has`
* `have`
* `is`
* `less`
* `mod`
* `mul`
* `no`
* `of`
* `out`
* `parameter`
* `parameters`
* `returns`
* `set`
* `sleep`
* `sub`
* `take`
* `than`
* `that`
* `the`
* `to`
* `value`
* `with`