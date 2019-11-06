# bashFunc

A small Bash library for functional programming. Right now it contains **map**,
**filter**, **reduce**, **forEach**, **some** and **split**.

## API

#### Callback

The callback is always the first argument and you can either use normal
functions or a string with the function body inside of `()` as first argument.  
These two code blocks are equal:

```bash
reduce '( echo $(( $1 + $2 )) )' 0 1 2 3 4 5
# 15
```

```bash
add() { echo $(($1 + $2)); }
reduce add 0 1 2 3 4 5
# 15
```

#### Arguments

As data input you can either use _positional arguments_ **or** the function
reads lines from the standard input - like the native `mapfile` command does -
and takes those as arguments. The following example demostrates the two
alternatives:

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

You can of course _chain_ different commands together:

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

The API for this function is quite different. The first argument is a _string_
and the second argument takes a _seperator_.

```bash
split 'Hello,World,Good,Evening' ','
# Hello
# World
# Good
# Evening
```
