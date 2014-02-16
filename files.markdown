f = open("index.markdown")
read(f, Int)
read(f, Int)
readline(f)
isopen(f)
eof(f)
close(f)

readlines(open("index.markdown")) #Does this close the file?

   +------+-----------------------------------+
   | r    | read                              |
   +------+-----------------------------------+
   | r+   | read, write                       |
   +------+-----------------------------------+
   | w    | write, create, truncate           |
   +------+-----------------------------------+
   | w+   | read, write, create, truncate     |
   +------+-----------------------------------+
   | a    | write, create, append             |
   +------+-----------------------------------+
   | a+   | read, write, create, append       |
   +------+-----------------------------------+
