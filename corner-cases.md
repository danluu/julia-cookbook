### == is transitive and 9007199254740993 != 9007199254740992

Julia is symmetric and transitive:
~~~.jl
9007199254740992.0 == 9007199254740992 # true
9007199254740992.0 == 9007199254740993 # false
9007199254740993 == 9007199254740992   # false
~~~

This is not the same as in most languages

Perl is not transitive
~~~.pl
$x = 9007199254740992;
$y = 9007199254740992.0;
$z = 9007199254740993;
$a = $x == $y;
$b = $y == $z;
$c = $x == $z;
print("x == y $a\n"); # 1
print("y == z $b\n"); # 1
print("x == z $c\n"); # -
~~~

C is not transitive
~~~.c
int main(void) {
  long   x = 9007199254740992;    // 2^53
  double y = 9007199254740992.0;  // 2.0^53
  long   z = 9007199254740993;    // 2^53+1

  printf("x == y: %d\n", x == y); // 1
  printf("y == z: %d\n", y == z); // 1
  printf("x == z: %d\n", x == z); // 0
  return 0;
}
~~~

javascript is transitive, but different integers are equal because they're represented by the same floating point number
~~~.js
9007199254740992 == 9007199254740992.0 // true
9007199254740993 == 9007199254740992.0 // true
9007199254740993 == 9007199254740992   // true!?!?
~~~

Octave behaves the same way as javascript


### Some methods return things in a different order (e.g., names(Base)) across different runs of julia (but they're fixed within a run).

That's because names is stored in a hash table, and address space randomization changes how addresses are hashed. You can disable address space randomization if you want these things to be deterministic.

