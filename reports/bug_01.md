**Name**

Invalid access

**Description**

Depending on the value input for the circle's center and its radius, the variable image_data may be subject to out of bounds writing, thus leading to undefined behavior.

**Affected Lines**

circle.c:49-52, circle.c:56-59, circle.c:70-73 and circle.c:77-80 (original file)
circle.c:54-57, circle.c:64-67, circle.c:84-87 and circle.c:94-97 (fixed file)

**Expected vs Observed**

We expect the loops to draw a circle inside the given image; in case the circle is designed in such a way that it doesn't completely fit in the image, it's either possible to abort the operation or, more easily, only draw the part of it that fits.
In practice, as no check is performed on the writing operation, the program tries to draw circle of any given size from any given starting point, leading to an out of bound write in case the parameters given do not imply a circle that is perfectly incapsulated in the image.

**Steps to Reproduce**

Possible commands on a 10x10 image:
./checkerboard checker.png -1 -1 3 ff0000
./checkerboard checker.png 4 4 100 ff0000

**Suggested Fix Description**

In order to fix the bug, it's necessary  to employ checks on the values of x and y before accessing the matrix, in order to avoid out of bounds writes. In this case, it is necessary to check that the values are both >= 0 and < than the respective limit (width or height). This way, the circle, or the allowed parts of it, will only be drawn in the image.
