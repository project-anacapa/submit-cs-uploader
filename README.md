# submit-cs-uploader
Work on a next generation uploader for submit.cs

An email from Bryce:

```
This email contains the directions for writing test cases,
automatically generating output, and synchronizing the test cases with
a project in the submission system.

The script you will want to run is located on a CSIL machine at
~bboe/bin/synchronize_test_cases.py. I have also attached the file and
the necessary configuration file if you want to run them in a location
that is not a CSIL machine. The only requirements are python 2.7+ and
the `requests` module.

Assuming you are on a CSIL machine the script should be run as:

~bboe/bin/synchronize_test_cases.py

Invoking that program requires a `-p PROJECT_ID` argument which is the
ID of an existing project on the submission site. The PROJECT_ID for a
project can be found by visiting the project summary page or the
project edit page. The respective urls for a sample project look like:

https://submit.cs.ucsb.edu/class/CS24_s13/28
https://submit.cs.ucsb.edu/form/project/28

The number at the end of the URL is the PROJECT_ID which in this case is 28.

On the command line also needs to be the path to one or more
directories. The name of the directory corresponds to the name of a
testable in the project. If the testable does not already exist then
it will be created.

Within each testable folder there should be 0 or more test cases. A
testcase is made up of 1 or more files that share the same basename
with various extensions. The hard-coded extensions are:

.stdin -- use the contents of this file as standard input. When not
provided there will be no standard input stream
.stdout -- use the contents of this file as the expected output to standard out
.stderr -- use the contents of this file as the expected output to
standard error
.args -- this file should contain the command to execute the program
(when not provided the execution line is som`a.out`

Additionally any other extension can be used to provide the expected
output for a named file whose name matches the extension. For any test
case, only one of .stdout, .stderr, or .NAMED_FILE may be provided.

For instance if we have a program that should create a named file
based on the command-line argument to the program we might create a
test structure like so (parts in quotes would be the content of the
file):

my_testable\
  creates_foobar.args "a.out foobar"
  creates_foobar.foobar ""
  invalid_arguments.args "a.out"
  invalid_arguments.stderr "Usage: a.out FILENAME\m"

At the moment this script does not include a way to specify the build
files and execution files for a testable. After synchronizing those
need to be specified.

It's also worth noting that synchronization will not delete any
testables or existing testcases, only update ones that happen to have
the same name. It will however, reset the settings for any test-case
that it updates (important if you manually change the points possible
for instance).

To automatically generate the output for tests that require only their
standard input run the following bash command with appropriate pieces
changed:

for f in $(ls *.stdin); do ./PATH_TO_SOLUTION < $f > ${f:0:-6}.stdout; done

Adapt the above command line to run programs that require reading in
from input files, or require special command-line arguments (use xargs
to read from .args).

-Bryce
```
