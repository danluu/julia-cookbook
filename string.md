## Chars

### Single char
~~~.jl
'0' # 0
'☃' # ☃

~~~

### Convert char to code point
~~~.jl
int('0') # 48
int('☃') # 9731
~~~

## Strings

### Methods on strings
~~~.jl
methodswith(String)
~~~

### Arithmetic
~~~.jl
'z' - 'a'     # 25
'a' + 25      # 'z'
'a' - 32      # 'A'
'a' < 'z'     # true
"abc" < "zbc" # true 
"abc" < "abz" # true
"bbc" < "abz" # false
~~~

### Raw string literals
~~~.jl
# Macros treat everything literally
macro L_str(s)
  s
end
L"\n" # '\' + 'n', i.e., "\\n"
~~~

### Byte array literals
~~~.jl
b"abcd\xff\x00\u0336foo"
 # 0x61
 # 0x62
 # 0x63
 # 0x64
 # 0xff
 # 0x00
 # 0xcc
 # 0xb6
 # 0x66
 # 0x6f
 # 0x6f
~~~

### String indexing
~~~.jl
"foobar"[1]     # 'f'
"foobar"[end]   # 'r'
"foobar"[-1]    # BoundsError()
"foobar"[1:1]   # "f"
"foobar"[1:end] # "foobar"
"foobar"[end:1] # ""

# search for first occurence
search("foobarbaz", 'b')    # 4
# search with offset
search("foobarbaz", 'b', 4) # 4 
search("foobarbaz", 'b', 5) # 7 
# reverse search
rsearch("foobarbaz", 'b')   # 7

# length in characters
length("foo")               # 3
length("╯°□°)╯︵ ┻━┻")      # 11
# length in bytes. Differs from length for unicode characters
endof("foo")                # 3
endof("╯°□°)╯︵ ┻━┻")       # 25
~~~

### String interpolation
~~~.jl
a = "foo"
b = "bar"
z = "baz"
string(a, b, z) # "foobarbaz"
"$a$b$c"        # "foobarbaz"
"\$a"           # "\$a"
~~~

### Reversing a string
~~~.jl
reverse("What happens if I reverse this?")                 # "?siht esrever I fi sneppah tahW"
oin(reverse(split("What happens if I reverse this?")),' ') # "this? reverse I if happens What"
~~~

### Stripping whitespace
~~~.jl
s = "  foo  \n" # "  foo  \n"
lstrip(s)       # "foo  \n"
strip(s)        # "foo"
rstrip(s)       # "  foo"
chomp(s)        # "  foo  "
~~~

### Regular Expressions
~~~.jl
foo_star = Regex("foo.*") 
ismatch(foo_star, "foo") # true
ismatch(foo_star, "bar") # false
# the r_str(s) macro is defined as Regex(s)
fs = r"foo.*"
ismatch(fs, "foo")       # true
ismatch(fs, "bar")       # false

fbb = r"(foo)(bar)?(baz|zab)" # r"(foo)(bar)?(baz|zab)"
match(fbb, "foo")        # nothing
m = match(fbb, "foobaz") # RegexMatch("foobaz", 1="foo", 2=nothing, 3="baz")
m.captures               # ["foo", nothing, "baz"]
m.offsets                # [1, 0, 4]
# TODO: case sensitive matches, greedy vs. non-greedy, etc.
~~~

