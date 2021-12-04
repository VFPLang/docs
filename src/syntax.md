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

## Grammar

The grammar is written in a modified [Backus–Naur form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form)

```
<program> = <body>

<body> = ( <statement> )*

<typed-ident> = IDENT "as" <type>

<statement> = "please" ( <variable-init>
                        | <variable-set>
                        | <if>
                        | <while>
                        | <fn-decl>
                        | <break>
                        | <return>
                        | <terminate>
                        | <expr>
                       ) "."

<variable-init> = "initialize variable" <typed-ident> "with the value of" <expr>

<variable-set> = "set the variable" <IDENT> "to the value of" <expr>


# control flow

<if> = <if-part>
       "please end check"

<if-part> = "check whether" <expr> ", then do"
                <body>
            ( <else> )?


<else> = "otherwise," ( <if-part> | <body> )

<while> = "repeat while" <expr> "do"
              <body>
          "please end while"

<break> = "break out of this while"

# function

<fn-decl> = "create function" IDENT "with" <params> <fn-return>
                <body>
            "please end function" IDENT

<params> = (<no-params> | <single-params> | <multi-param>)

<no-params> = "no parameters"

<single-param> = "the parameter" <typed-ident>

<multi-param> = "the parameters" <typed-ident> ( "," <typed-ident> )* "and" <typed-ident>

<fn-return> = "that returns" type

<return> = "return" <expr> "from the function"

# other

<terminate> = "go to sleep"


# fn calls

<call> = "call" IDENT "with" <call-args>

<call-args> = <no-args> | <single-arg> | <multi-arg>

<no-arg> = "no arguments"

<single-arg> = "the argument" <expr> "as" IDENT

<multi-arg> = "the arguments" <expr> "as" IDENT ( "," <expr> "as" IDENT )* "and" <expr> "as" IDENT

# type

<type> = IDENT | <nullable>

<nullable> = "absent" | "null" | "novalue" | "undefined"

# expressions

<expr> = <comparison>

<comparison> = <term> ( ( "does not have the value"
                          | "has the value"
                          | "is greater than"
                          | "is less than"
                          | "is greater or equal than"
                          | "is less or equal than"
                         ) <comparison> )?


<term> = <add> | <subtract> | <factor>

<add> = "add" <factor> "to" <term>

<subtract> = "subtract" <factor> "from" <term>


<factor> = <multiply> | <divide> | <modulo> | <call-expr>

<multiply> = "multiply" <call-expr> "with" <factor>

<divide> = "divide" <call-expr> "by" <factor>

<modulo> = "take" <call-expr> "modulo" <factor>


<call-expr> = <call> | <literal>

<primary-expr> = "(" <expr> ")" | <literal>



# literals

<literal> = <nullable> | STRING | <number> | "true" | "false" | IDENT

<number> = <int> | <float>

<int> = "-"? ( DIGIT )+

<float> = "-"? ( DIGIT )+ ( "." ( DIGIT )+ )?
```