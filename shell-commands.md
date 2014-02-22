## Running a Command
~~~.jl
run(`echo hi`)
~~~

## Writing `ls`
~~~.jl
file_names = readdir()
for name in file_names
  println(name)
end
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
