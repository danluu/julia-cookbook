## Running a Command
~~~.jl
run(`echo hi`) # anything in `backticks` is a Cmd.
~~~

## `ls`
### A Julia Function
~~~.jl
julia> readdir()
6-element Array{String,1}:
 "files.markdown"        
 ".git"                  
 "index.markdown"        
 "README.md"             
 "shell-commands.md"     
 ".shell-commands.md.swp"
~~~

### A Litte More Code
~~~.jl
file_names = readdir()
for name in file_names
  println(name)
end
~~~

### Using `run`
~~~.jl
julia> run(`ls`)
files.markdown	index.markdown	README.md  shell-commands.md
~~~

### Using `;`
~~~.jl
julia> ;ls
files.markdown  index.markdown  README.md  shell-commands.md
~~~

## Navigating Directories

### Using Julia Functions
~~~.jl
julia> pwd()
"/home/leah/src/julia-cookbook"

julia> cd("../")

julia> pwd()
"/home/leah/src"
~~~

### Using `;` to Escape to the Shell
~~~.jl
julia> ;pwd
/home/leah/src

julia> ;cd julia-cookbook/
/home/leah/src/julia-cookbook

julia> ;pwd
/home/leah/src/julia-cookbook
~~~

### You can't use `run`
~~~.jl
julia> run(`pwd`)
/home/leah/src/julia-cookbook

julia> run(`cd ..`)
ERROR: could not start process `cd ..`: no such file or directory (ENOENT)
 in run at process.jl:457

julia> run(`pwd`)
/home/leah/src/julia-cookbook
~~~

## Using & and |>
Julia does not actually call a shell for any of this; it parses everything itself.
As part of this, operators like `&` and `|` are now Julia operators; they go between `Cmd`s, not inside them.

### Using `&`
~~~.jl
julia> `echo greetings` & `echo reader`
`echo greetings` & `echo reader`

julia> run(ans)
greetings
reader
~~~

### Using |>
~~~.jl
julia> run(`echo hi` |> `cat -`) # redirect first command's stdout to second command's stdin
hi

julia> run(`echo hi` |> "tmp.txt") # redirect stdout to a file

julia> run("tmp.txt" |> `cat -`) # read file into stdin
hi
~~~


