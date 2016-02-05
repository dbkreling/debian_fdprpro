		       Post-link optimization for Linux on POWER
		       =========================================

Supplied files and directories
------------------------------
1. share/doc directory
   README.txt         - This file.
   COPYRIGHT
   license_files.zip  - license agreements

2. bin directory
   fdprpro            - Post-link optimizer main executable. 
   fdpr               - Wrapper program to facilitate usage of fdprpro.

3. lib directory
   libfdprinstr32.so  - ELF32 instrumentation library, used for programs
                        compiled in 32-bit mode.
   libfdprinstr64.so  - ELF64 instrumentation library.

4. share/man1 directory
   fdpr.1             - Post-link optimizer man page.

5. share/examples directory
   run_test           - Runs a full optimization cycle.
   run_bzip2          - An example script for running bzip2.
   bzip2              - 32bit program taken from the SPEC2000, it was compiled
                        with -O3 using gcc 3.2.3 (Red Hat Linux 3.2.3-20).
   input.random       - The SPEC2000 test input files for bzip2.
   bzip2.out          - Expected output of run_bzip2.

Accepting the license agreement
-------------------------------
Read the license carefully in your language of choice. If you do not accept it,
remove the package. By using the package you implicitly accept the license
agreement.

Setting the environment for the post-link optimizer
----------------------------------------------------
By default, no further operation is needed following the installation and users
can immediately access the programs and man page. This assumes proper use of
/opt/bin, /opt/lib, and /opt/man:

1. Links to the programs, 'fdpr' and 'fdprpro', are set in /opt/bin. Make sure
   /opt/bin is in the users' command path (PATH).

2. A link to the man page is set in /opt/man/man1, such that the man page becomes
   available automatically. If a user defines (his/her own) MANPATH, /opt/man
   should be included in it.

3. Links to shared libraries are set in /opt/lib such that the libraries become
   available automatically. A possible alternative is to define LD_LIBRARY_PATH
   so that it includes /opt/lib.


Using the post-link optimizer 
-----------------------------

There are 3 main steps when using the post-link optimizer: instrumentation,
profile collection, and optimization. The first and third steps are activation
of "fdprpro" on the input program. The second step consists of exercising the
instrumented program created in the first step.

The post-link optimizer is typically run using the wrapper script, "fdpr",
which performs all the above steps (or any legal combination thereof), in one
command. Use "fdprpro" when a more explicit control is desired over each step.

See fdprpro --help, or fdpr --help for on-line information.

More complete information is available in the post-link optimizer man page.

Verifying the installation
--------------------------
Following setup, enter share/examples directory and run the the following script:

	$ ./run_test

Note: If you don't have write permissions in this directory, then copy it with
all its contents to a location where you have write permissions, and run there.

The script runs "fdprpro" through the full cycle of instrumentation, profiling,
and optimization.  During the test, it reports the time it takes to run the
original test program, bzip2, the instrumented program, and finally the
optimized program.

Once "run_test" completes successfully, run a similar test for the "fdpr"
wrapper:

	$ fdpr --instr --train ./run_bzip2 --opt bzip2

This test performs the equivalent of the above "run_test" script, except that
it does not print timing information. Here, "run_bzip2" is a script that exercises
bzip2 (or an optimized version of it) on a predefined workload.

If any of the above fail, an error message is displayed to that effect.
