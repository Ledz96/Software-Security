**Name**

Use after free/Invalid access

**Description**

In case of memory errors, the same memory object is freed twice. By the specification of the free() function, calling free on a memory object that is not allocated leads to undefined behavior.

**Affected Lines**

resize.c: 86-87 (original file)
checkerboard.c:112, checkerboard.c:114, checkerboard.c:116 and checkerboard.c:118 (fixed file)

**Expected vs Observed**

We expect the free to only be invoked once, in order to free the allocated memory object.
However, two frees are invoked for the same object. This leads to undefined behavior.

**Steps to Reproduce**

./resize_image input.png output.png #very_big_number

The input image and resize must be large enough to trigger a memory error in the attempt to allocate a new image.

**Suggested Fix Description**

In order to fix the bug, it is enough to delete the unnecessary free() invocation.
