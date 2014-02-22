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

## String Interpolation
~~~.jl
julia> filename = "test\nnewline.txt"
"test\nnewline.txt"

julia> print(filename)
test
newline.txt

julia> `cat $filename`
`cat 'test
newline.txt'`

julia> run(`echo hi` |> filename)

julia> run(`cat $filename`)
hi
~~~

~~~.jl
julia> file_names = ["foo","bar","baz","foo1 bar1","`echo hi`"]
5-element Array{ASCIIString,1}:
 "foo"      
 "bar"      
 "baz"      
 "foo1 bar1"
 "`echo hi`"

julia> `touch $file_names.txt`
`touch foo.txt bar.txt baz.txt 'foo1 bar1.txt' '\`echo hi\`.txt'`

julia> `touch prefix$file_names.txt`
`touch prefixfoo.txt prefixbar.txt prefixbaz.txt 'prefixfoo1 bar1.txt' 'prefix\`echo hi\`.txt'`
~~~

~~~.jl
julia> names = ["i","j","k"]
3-element Array{ASCIIString,1}:
 "i"
 "j"
 "k"

julia> prefixes = ["a","b","c"]
3-element Array{ASCIIString,1}:
 "a"
 "b"
 "c"

julia> extensions = ["x","y","z"]
3-element Array{ASCIIString,1}:
 "x"
 "y"
 "z"

julia> `touch $prefixes$names$extensions`
`touch aix aiy aiz ajx ajy ajz akx aky akz bix biy biz bjx bjy bjz bkx bky bkz cix ciy ciz cjx cjy cjz ckx cky ckz`
~~~

## Capturing Output

~~~.jl
julia> readall(`cat jabberwocky.txt`)
"'Twas brillig, and the slithy toves\nDid gyre and gimble in the wabe;\nAll mimsy were the borogoves,\nAnd the mome raths outgrabe.\n\n\"Beware the Jabberwock, my son!\nThe jaws that bite, the claws that catch!\nBeware the Jubjub bird, and shun\nThe frumious Bandersnatch!\"\n\nHe took his vorpal sword in hand:\nLong time the manxome foe he sought—\nSo rested he by the Tumtum tree,\nAnd stood awhile in thought.\n\nAnd as in uffish thought he stood,\nThe Jabberwock, with eyes of flame,\nCame whiffling through the tulgey wood,\nAnd burbled as it came!\n\nOne, two! One, two! and through and through\nThe vorpal blade went snicker-snack!\nHe left it dead, and with its head\nHe went galumphing back.\n\n\"And hast thou slain the Jabberwock?\nCome to my arms, my beamish boy!\nO frabjous day! Callooh! Callay!\"\nHe chortled in his joy.\n\n'Twas brillig, and the slithy toves\nDid gyre and gimble in the wabe;\nAll mimsy were the borogoves,\nAnd the mome raths outgrabe.\n"
~~~

~~~.jl
julia> (stream,process) = readsfrom(`cat jabberwocky.txt`)
(Pipe(active, 0 bytes waiting),Process(`cat jabberwocky.txt`, ProcessRunning))

julia> readline(stream)
"'Twas brillig, and the slithy toves\n"

julia> readline(stream)
"Did gyre and gimble in the wabe;\n"
~~~

## Interacting With Processes
