**Name**

Command injection

**Description**

In order to inform the user of the size of an image, the program invokes the command "stat", appending the content of the variable output_name to use it as a target. As a consequence, the name of the output file, that is provided by the user, may be used to perform a command injection attack. For example, the user may insert between quotes (") the name of the file, followed by a semicolumn or an &. Any command after that will be executed on the system where the "stat" function is executed.

**Affected Lines**

solid.c: 119 (original file)
solid.c: 106, 107, 115 (fixed file)

**Expected vs Observed**

We expect the program to only get the needed information on the generated file, without executing any other undesired command.
However, the program attempts to retrieve the information using a command, and, in absence of prepared statements, the risk of a command injection attack is concrete. 

**Steps to Reproduce**

./solid "mysolid.png; mkdir waiting_for_ff7r" 10 10 ff0000

More malicious uses of the command injection are, of course, possible.

**Suggested Fix Description**

One of the possible solutions for the bug is not to use commands, and collect information on the created file on a struct stat using the stat() function instead. The size of the file is located in the st_size field of struct stat.
