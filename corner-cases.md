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


### names(Base) has a non-determistic order across invocations of julia, but it's deterministic within a single run

This is pretty obscure, but I'm curious why this is true.
TODO: look into this.

`module.c` has:
~~~
DLLEXPORT jl_value_t *jl_module_names(jl_module_t *m, int all, int imported)
{
    jl_array_t *a = jl_alloc_array_1d(jl_array_symbol_type, 0);
    JL_GC_PUSH1(&a);
    size_t i;
    void **table = m->bindings.table;
    for(i=1; i < m->bindings.size; i+=2) {
        if (table[i] != HT_NOTFOUND) {
            jl_binding_t *b = (jl_binding_t*)table[i];
            if (b->exportp || ((imported || b->owner == m) && (all || m == jl_main_module))) {
                jl_array_grow_end(a, 1);
                //XXX: change to jl_arrayset if array storage allocation for Array{Symbols,1} changes:
                jl_cellset(a, jl_array_dim0(a)-1, (jl_value_t*)b->name);
            }
        }
    }
    JL_GC_POP();
    return (jl_value_t*)a;
}
~~~

so it looks like there's a table that we iterate through. Are insertions into that table in random order?
