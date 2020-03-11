**Name**

Invalid access

**Description**

After trying to allocate the pointer new_img->px, a check on the success of the operation must be performed before going on. Said check, however, is wrongfully performed on the variable img->px. As a consequence, the variable is successfully accessed independently of the success of the allocation, leading to undefined behavior.

**Affected Lines**

resize.c: 49 (original file)
resize.c: 47 (fixed file)

**Expected vs Observed**

We expect the check to be executed on the new_img->px; in case the allocation is not successful, the execution should jump to error_memory_img.
However, since the control is performed on the wrong pointer, the execution will procede normally independently of the success of the allocation. This causes undefined behavior, when the memory is later accessed.

**Steps to Reproduce**

./resize_image input_image output_image #very_large_resize_factor

The size of the resize factor depends on the original image, as well as the memory allocated for the execution of the program.

**Suggested Fix Description**

In order to fix the bug, it is enough to perform the check on new_img->px, rather than img->px.
