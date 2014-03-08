## Symbols and Expressions

### Symbols
`:` quotes something. Identifiers turn into symbols. Literals turn into themselves.
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

### Macros
~~~.jl
macro selfie(x)
  x
end

@selfie(3) # 3
@selfie(foo) # ERROR: foo not defined
@selfie 2 + 2 # 4

macro selfie_quote_bad(x)
  quote x end
end

@selfie_quote_bad(3) # ERROR: x not defined
# symbols that aren't interpolated use the calling context
x = 10; @selfie_quote_bad(3) # 10

macro selfie_quote(x)
  quote $x end
end

x = 10; @selfie_quote(3) # 3
@selfie_quote(y) # ERROR: y not defined
x = 10; y = "foo"; @selfie_quote(y) # "foo"

macro my_for(var, range, body)
  quote
    for $var in $range
      $esc(body)
    end
  end
end


@my_for(foo, 1:10, print("$foo "))
# 1 2 3 4 5 6 7 8 9 10

macroexpand(quote @my_for(foo, 1:10, print("$foo ")) end)
# :(begin  # none, line 1:
#  begin  # none, line 3:
#    for #113#foo = 1:10 # line 4:
#      print(Expr(string, #113#foo, " "))
#    end
#  end
# end)

# Note that the foo is name mangled; because Julia macros are hygenic, we need to escape
# them to prevent the names from being santizied, if you want to interact with something
# outside the macro
~~~
