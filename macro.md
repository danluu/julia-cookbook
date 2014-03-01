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

### Expressions
~~~.jl
three = :(1+2) # :(1 + 2)
typeof(three)  # Expr
three.args     # [:+, 1, 2]
eval(three)    # 3
quote 1 + 2 end
# :(begin
#   1 + 2
# end)
quote
  x = 3 + y
end
# :(begin
#   x = 3 + y
# end)
~~~
