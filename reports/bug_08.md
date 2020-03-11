**Name**

Out of bounds write

**Description**

The for loop used to resize the image uses the function round() to determine the upper limit of the iteration; in the allocation, however, the same upper limit is defined through the use of a simple division, that always approximates by default. As a consequence, the for loop may try to write in non allocated memory, causing undefined behavior.

**Affected Lines**

resize.c: 58-59, resize.c: 66 (original file)
checkerboard.c:112, checkerboard.c:114, checkerboard.c:116 and checkerboard.c:118 (fixed file)

**Expected vs Observed**

We expect the loop to iterate inside the allocated area, in order to process the new resized image.
However, because of the difference between the function used in the allocation and in the for loop, the upper limit of the loop may, depending on the input, be higher than the allocated memory by one unit. As a consequence, the last iteration of every row would lead to an attempt to write on non allocated memory, and, as a consequence, to undefined behavior. 

**Steps to Reproduce**

Possible commands, with a 23x23 input_image:
./resize_image input23.png output.png 2.3

With this input, the new height and width are 2.3*23=52, but every row is accessed up until the round(2.3*23)=53rd element.

**Suggested Fix Description**

In order to fix the bug, it is enough to replace the round() invocation with new_height and new_width for the outer and inner for loops respectively.
