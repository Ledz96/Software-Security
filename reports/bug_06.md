**Name**

Type confusion

**Description**

In order to make an image transparent, the function filter_transparency() is called. Its second parameter transparency is an opaque pointer (void*), that is then dereferenced and assigned as an uint8_t. The calling function, however, passes a double instead. Since no syntax error is present, the code compile successfully, leading to unexpected behavior.

**Affected Lines**

filter.c: 132, 221 (original file)
filter.c: 136, 224 (fixed file)

**Expected vs Observed**

We expect the function to iterate over all the pixels and change the transparency based on the alpha parameter provided over the command line.
However, since the parameter passed is not &alpha, but &weights, the new transparency will be independent from the passed parameters, and only depend on the current value of weights.

**Steps to Reproduce**

./filter input.png output.png alpha ffffff (any input is acceptable for the last parameter)

**Suggested Fix Description**

In order to fix the bug, it is enough to assign &alpha, instead of &weights, to the variable fil.arg.
