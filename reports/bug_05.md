**Name**

Invalid access/use after free

**Description**

In order to allocate a pixel, the function get_pixel() is called. Its implementation, however, allocates the pixel in the stack and then returns the pointer to the variable. As a consequence of the return, all the variables in the stack are freed, and any subsequent dereference leads to undefined behavior.

**Affected Lines**

filter.c: 118-121 (original file)
filter.c: (fixed file)

**Expected vs Observed**

We expect the function to allocate a variable that can then be used outside of the function itself through its pointer. However, because of the allocation happening in the stack, the returned variable points to a freed area of memory. As a consequence, all subsequent dereferentiation of the neg variable will lead to undefined behavior.

**Steps to Reproduce**

./filter input.png output.png negative

**Suggested Fix Description**

At least two possible solutions exist to fix the bug. 
1) The function may be deleted altogether, allocating the neg variable directly as a struct_pixel in the filter_negative() function. In case this solution is adopted, accesses to the fields of the struct must happen with the syntax "neg.field", as opposed to neg->field. No dereferencing is needed in this case.
2) The variable may be allocated in the heap rather than the stack, so that it is not freed when the function returns. In case this solution is adopted, the variable must be freed at the end of every loop, in order to avoid memory leaks.
