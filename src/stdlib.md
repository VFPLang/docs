# Standard library

The standard library is very minimal for now, but it will be extended in the future

## IO

Functions for doing IO

### `println`

Prints any value to the standard output. Has the special type `<Any>`, meaning it can take any value

| Name    | Parameters         | Returns |
|---------|--------------------|---------|
| println | (x as &lt;Any&gt;) | absent  |

## Other

### `time`

Returns the current time in unix milliseconds

| Name | Parameters | Returns |
|------|------------|---------|
| time | ()         | Integer |