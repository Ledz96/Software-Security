**Name**

Buffer overflow

**Description**

In order to copy the arguments passed from the command line, the unsafe function strcpy() is used. As a consequence, the entire user input is copied on the stack, without considering the upper limit of the destination array (256 byte, including the string terminator).

**Affected Lines**

rect.c: 20, 21 (original file)
rect.c: 21, 23 (fixed file)

**Expected vs Observed**

We expect the function to copy the command line arguments to the fixed length char arrays input and output. Of course, according to the expected behavior, at most 256 bytes should be written for each string, including the terminator.
In practice, since no check is employed, the writing operations copies all the input bytes to the stack, starting from the address of the first element of the respective array. As a consequence, it's possible to overflow the buffer and write out of the scope of the array. Other than causing a crash, in certain cases this vulnerability could be exploited to, for example, override the return address of the function.

**Steps to Reproduce**

./rect input_AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA output_image.png 0 0 1 1 ff0000

**Suggested Fix Description**

In order to fix the bug, it is enough to replace the strcpy() invocations with a strncpy, with a limit of 255 bytes being copied. In addition, a string terminator must be added in the 256th byte of the arrays. This way, all the additional bytes will be ignored. Unfortunately, this limits the length of the file names to 255 bytes.
