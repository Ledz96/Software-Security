**Name**

Invalid access

**Description**

Depending on the value input for square_width, the variable image_data may be subject to out of bounds writing, thus leading to undefined behavior.

**Affected Lines**

checkerboard.c:112, checkerboard.c:113, checkerboard.c:114 and checkerboard.c:115 (original file)
checkerboard.c:114, checkerboard.c:115, checkerboard.c:116 and checkerboard.c:117 (fixed file)

**Expected vs Observed**

We expect the loops to iterate over every square inside the matrix, and fill it with the correct color;  as the number of squares is predetermined, either squares must be left incomplete, or the square size input must be regulated.
In practice, as neither upper bounds nor limitations (e.g.: the square_width field must divide the height and width fields) are established, the program keeps "filling" every square even outside the allocated space for the matrix.  

**Steps to Reproduce**

Possible commands:
./checkerboard checker.png 10 10 100 000000 ffffff
./checkerboard checker.png 10 10 3 000000 ffffff

**Suggested Fix Description**

In order to fix the bug, two different fixes are possible:

1) Before starting any writing operation on the matrix, check that the value of square_width is both lower than and a divisor for height and width; if it is not, exit the program with an error. This will halt the program in case the input doesn't allow a perfect checkerboard.
2) Just before writing on the matrix, verify that the indexes are in bound (i.e.: (square_top_left_y + y < height) && (square_top_left_x + x < width)) and skip to the next step of the loop if they are not. This solution allows incomplete checkerboards.

A hybrid between the solutions (such as not allowing a square_width larger than the width or height of the image itself, but drawing incomplete checkerboards) is possible as well.
