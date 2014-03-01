## Symbols and Expressions

### Symbols
~~~.jl
:foo                 # :foo
typeof(:foo)         # Symbol
:'                   # ERROR: syntax: incomplete: invalid character literal
:'\''                # '\'' 
typeof(:'\'')        # Char
symbol('\'')         # :'
typeof(symbol('\'')) # Symbol
:3                   # 3
typeof(:3)           # Int64
symbol(3)            # no method symbol(Int64)
~~~
