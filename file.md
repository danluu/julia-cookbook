## Printing out a file

### Using readline to read one line at a time
~~~.jl
f = open("files.markdown")
line = readline(f)
while line != ""
  print(line)
  line = readline(f)
end
close(f)
~~~

### Using read to read one char at a time
~~~.jl
f = open("files.markdown")
while !eof(f)
  print(read(f,Char)) # same as print(readline(f))
end
close(f)
~~~

### Using readlines to read the entire thing
~~~.jl
print(map(chomp,readlines(open("files.markdown"))))
~~~

## Printing out comments from a file

### Using readuntil to print everything after a comment character
~~~.jl
f = open("files.markdown")
readuntil(f, "#")
while !eof(f)
  print("#", readline(f))
  readuntil(f, "#") != ""
end
~~~.jl
