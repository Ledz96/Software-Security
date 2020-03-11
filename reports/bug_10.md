**Name**

Use after free

**Description**

The variable palette, that is allocated earlier in the code, is subject to a misplaced free() invocation, that is called regardless of the presence of errors. As a consequence, subsequent accesses to palette's fields causes undefined behavior.

**Affected Lines**

solid.c: 49 (original file)
solid.c: 124 (fixed file)

**Expected vs Observed**

We expect the program to free palette only after the variable has been used for its purpose, or in case an error is triggered.
However, because of the lack of brackets after the if statement that checks the presence of errors, the free() function is invoked anyway, leading to undefined behavior in the following accesses to the variable.

**Steps to Reproduce**

./solid output_name height width hex_color (for example: ./solid output.png 10 10 ff0000). As long as the input color, height and width are correct, the bug will be reproduced.

**Suggested Fix Description**

In order to fix the bug, two possible solutions may be adopted:

1) Insert the "free(palette)" instruction in brackets after every if statement that checks the validity of the parameters (and before the "goto error").
2) Insert the "free(palette)" instruction in the handling code for "error", before returning. This solution avoids redundancy and is faster to implement, and has therefore been chosen.
