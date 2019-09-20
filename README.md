# bashFunc

A small library for functional programming in bash - map, filter, reduce and more.

## API

#### Callback

The callback is always the first argument and you can either use a normal
function as() argument or enter a string with the function body inside `()`, like
_lambdas_.  
These two commands are equal:

```bash
reduce '( echo $(( $1 + $2 )) )' 0 1 2 3 4 5
# 15

add() { echo $(($1 + $2)); }
reduce add 0 1 2 3 4 5
# 15
```

#### Arguments

You can either use _positional arguments_ **or** the function reads lines from
the standard input like the native `mapfile` command does and takes those as
arguments.  
These two commands are equal:

```bash
map '( echo $(( $1 + 10 )) )' 1 2 3 4 5
map '( echo $(( $1 + 10 )) )' < <(printf "%s\n" 1 2 3 4 5)
# 11
# 12
# 13
# 14
# 15
```

#### Compatibility

```bash
reduce '( echo $(( $1 + $2 )) )' < <(filter '( (( $1 % 2 )) )' < <(map '( echo $(( $1 + 10 )) )' 1 2 3 4 5))
# 39
```

## Examples

#### Map

```bash
map '( echo $(( $1 + 10 )) )' < <(printf "%s\n" 1 2 3 4 5)
# 11
# 12
# 13
# 14
# 15
```

#### Filter

```bash
filter '( (( $1 % 2 )) )' < <(printf "%s\n" 1 2 3 4 5)
# 1
# 3
# 5
```

#### Reduce

The second argument, in this case the `10`, is the starting value of the
_accumulator_.

```bash
reduce '( echo $(( $1 + $2 )) )' 10 < <(printf "%s\n" 1 2 3 4 5)
# 25
```

#### ForEach

```bash
forEach '( notify-send $1 )' < <(printf "%s\n" 1 2 3 4 5)
```

#### Some

The `some` function tests whether at least one element passes the test
implemented by the provided function.

```bash
some '((( $1 % 2 )))' < <(printf "%s\n" 1 2 3 4 5)
# echo $?
# 0
```

#### Split

The API for this function is fundamentally different. The first argument is a
_string_ and the second argument takes a _seperator_.

```bash
split 'Hello,World,Good,Evening' ','
# Hello
# World
# Good
# Evening
```
