Submission version: 1.0

These are the instructions to reproduce the results published in the paper:

    Minimizing speculation overhead in a parallel recognizer for regular texts


Getting Started Guide
---------------------

This artifact consists of:

   - this README.TXT file
   - a test program repar.jpp
   - a test program revtf.jpp
   - a number of data files

To run the repar.jpp program:

   1. Use a computer with a suitable number of cores (in our measurements
      we used a Dell PowerEdge R7425 with two 32-core EPYC 7551 processors,
      thus a total of 64 cores, see the paper for details).
   2. The computer can run Windows 10 or Linux.
   3. Java (jre of jdk), at least version 19 must be installed.
      To test that it is installed, type "java -?" at a terminal window and
      see displayed the usage of Java. If it is not so, download it from:

         https://www.oracle.com/it/java/technologies/downloads/
          
      and follow the instructions to install it.
   4. Open a terminal window, create an empty directory and unpack in it
      the artifact.
   5. From the command prompt, run the program:

         java -cp repar.jar pbsp.ReParTest meDfaVsDfa

      The program produces on the console (standard output) the message:

         all tests passed

      meaning that all the basic internal tests passed successfully.
      Then it prints a number of measurement data, which are the same that
      we have used to produce the charts published in the paper.
      Note that the actual values are likely to be different from ours
      if the computer speed or load are different from ours.
      The program produces the measures for the charts published in the
      paper, which are the time speedup of the parallel recognizer vs the
      serial one and the ratio of numbers of transitions.
      The measures are:

       - four tables of measurements of recognitions using increasing threads from 2
           step 8 nrincr to 74 (excluded)
       
         - a table with the time speedup of RI-DFA vs DFA (time DFA / time RI-DFA)
           [n. of threads][text length], e.g.:

           threads ----------- text lengths -------------------
                   658K    679K    701K    722K    744K    765K
           2       1.05    0.87    0.87    0.86    0.80    0.92
           10      1.03    0.87    0.94    0.93    1.01    0.86
           18      1.10    0.80    0.78    0.92    0.86    0.91
            
         - a table with the time speedup of RI-DFA vs DFA (time DFA / time RI-DFA)
           [text length][n. of threads], e.g.:

           lengths ----------------------nr of threads --------------------------------
                   2       10      18      26      34      42      50      58      66
           658K    1.05    1.03    1.10    0.94    1.00    0.87    0.88    1.06    0.85
           679K    0.87    0.87    0.80    0.89    0.30    0.87    0.81    0.86    0.79
           701K    0.87    0.94    0.78    0.92    0.86    0.89    0.82    0.85    1.07

         - a table with the transition ratio of RI-DFA vs DFA (n. transitions DFA /
           n. transitions RI-DFA)[n. of threads][lengths].
           This is similar to the first one, except that it shows the ratio of transition nrs.

         - a table with the transition ratio of RI-DFA vs NFA (n. transitions NFA /
           n. transitions RI-DFA)[n. of threads][lengths].
           This is similar to the first one, except that it shows the ratio of transition nrs.
           
       - four tables of measurements of recognitions using 58 threads

      These tables contain the data used to draw the figures 6 and 7 in the paper.

      N.B. This can take some time since it has to make several measures in increasing
      number of threads and lengths.

   6. From the command prompt, run the program:

         java -cp revtf.jar pbsp.ReVtf ondrik

      N.B. This can take some time since it has to process more than one thousand NFAs.
      First it prints the percentage of the progress of the processing of the ondrik NFAs.
      Then it prints on the console the frequency distribution of the numbers of states:
      the population is the NFAs, and the classes are intervals of the ratio of the number
      of NFA states divided by the number of DFA states (having built a minimal DFA for
      each NFA).
      E.g. the second line shows how many NFAs have 0.50-0.60 NFA states with respect to
      the corresponding DFA states. 
      A similar frequency distribution is done for the ratio of the number of RI-DFA
      states divided by the number of DFA states.
      Then another is displayed with classes that are intervals of numbers of NFA states,


Step-by-step instructions
-------------------------

The repar.jpp program can also be used to measure the performance of the algorithm with
benchmarks other than the ones published in the paper.
To do so, there is a need for:

      1. a regular expression
      2. a number of texts

To run the program, type at the command prompt:

     java -cp repar.jar pbsp.ReParTest medfa <bench-name> <re-file> <nrsteps> <nrincr> <text-file> ...

     where:

     <bench-name>:   a user-chosen name for the benchmark
     <re-file>:      the file containing the regular expression of the benchmark
     <nrsteps>:      the number of the runs done, each in increasing number of threads
     <nrincr>:       the increment in the number of threads, starting with nrstart, nrstart + nrincr, ...
     <nrstart>:      the number of initial threads
     <text-file>:    one or more files containing the texts, in increasing lengths

     E.g.:

     java -cp repar.jar pbsp.ReParTest medfa big bigre.txt 4 3 2 bigdata0.txt bigdata1.txt

The results should confirm the claims published in the paper that the parallel algorithm
performs substantially faster than the serial one, which is due to parallelization and
to the reduction of the speculation overhead.
