**Name**

Invalid access

**Description**

Because of the way the iteration for loop is coded, image_data is always accessed with an out of bound write. This is undesirable, as it leads to undefined behavior.

**Affected Lines**

rect.c: 75-78 (original file)
rect.c: (fixed file)

**Expected vs Observed**

We expect the loop to iterate through the allocated rectangle, filling it with a color. 
In practice, the iteration accesses non allocated memory, as the limit for height and width is not respected, but overcome by one pixel.

**Steps to Reproduce**

./rect input_image output_image top_left_x top_left_y bottom_right_x bottom_right_y hex_color (any value will trigger the bug)

**Suggested Fix Description**

In order to fix the bug, it is enough to change the '<=' in line 60 and 61 of the original file with '<'. This way, no out of bound access will happen.
