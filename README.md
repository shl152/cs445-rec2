# CS 445 Recitation 2: Bag Client and Building with Gradle

## Introduction

In this recitation exercise, you will implement a brute-force algorithm for the
longest common subsequence problem (LCS) using a Bag. You will be using the Bag
without delving into the details of how the class is implemented; that is, you
will be writing _client code_.

The primary goal of this exercise is to practice solving problems using the
methods provided by a data structure. A secondary goal is to familiarize
yourself more deeply with the ADT Bag and its potential uses.

The ADT Bag is one of the primary collection structures defined in mathematics.
Also called a multiset, it is a generalization of a set that is allowed to hold
multiple copies of an item. Like a set, the items contained within the bag have
no particular ordering. Before completing this exercise you should review the
methods available to you in the Bag ADT.

## Gradle Building

With this exercise, we will also introduce the process of building and running
code using the gradle build tool. In Recitation 1, you learned how to build and
run manually from the command line. However, in practice, any reasonably-large
project will include a build system so you do not have to complete these steps
manually. Advantages of build tools include:

 - A new developer can compile and run the project quickly without having full
   knowledge of all the files involved
 - Your entire project, including classes in multiple packages, can be built at
   once with a single command
 - If a program has to be built with special options enabled, the build system's
   configuration files can make sure these are set by default
 - If a program can be built in multiple different ways, these can each be coded
   as a "task" in the build system
 - Unit tests and other debugging tools can be added as alternative methods of
   compiling and running the code (you will explore this in a future recitation
   exercise)

There are a few reasons we started you with the manual approach:

 - Build tools can be a bit opaque, and if you don't know how to do things
   manually, you might not be able to debug when things go wrong
 - For small projects with one or two files, it's faster to compile and run
   without a build tool
 - Build tools rely on the same Java binaries that you use directly when you
   call `javac` and `java`, so it's important to be sure they're working
   correctly before trying a more complex method

As you can see, build tools are very useful, and being familiar with one is a
good way to expand your skills and make yourself more attractive when seeking a
job or internship. At the same time, it's important to know what's going on
"under the hood" in case things go wrong or you want to run a short program
quickly. We will use both methods moving forward in this class.

To see a version of Recitation 1 that uses gradle, see [the "gradle" branch of
the Recitation 1 repository](https://github.com/2217-cs445/cs445-rec1/tree/gradle).
Pay particular attention to the steps marked, "**NEW**".

## Exercise

1. Download the provided code by cloning this [Recitation 2
repository](https://github.com/2217-cs445/cs445-rec2). You will find lots of
gradle files and subfolders. The code for a gradle project in Java can usually
be found in `app/src/main/java/`. Navigate to this subdirectory, then into the
`cs445/rec2/` package folder within it, and note the following Java files
inside:

   - `BagInterface.java` is a Java interface representing the ADT Bag. It is a
     slightly expanded version of what we developed in lecture.
   - `ArrayBag.java` is a dynamic capacity array-based implementation of ADT
      Bag. You will not need to focus on this implementation in this exercise.
   - `LongestCommonSubsequence.java` provides the skeleton of a LCS solution.

   If you `cd` into `app/src/main/java/`, you can compile and run this code in
   the manual way like you did with Recitation 1. In particular, use the
   following commands:

       javac cs445/rec2/LongestCommonSubsequence.java
       java cs445.rec2.LongestCommonSubsequence

   However, a simpler alternative is available thanks to the gradle build tool.
   From the root of the repository (i.e., *without* navigating to
   `app/src/main/java/`), you can use one of the following commands to compile
   and run the program automatically using gradle:

   - `./gradlew run` (on Unix-like terminals such as those found on Mac, Linux,
     or Windows Subsystem for Linux)
   - `gradlew.bat run` (on DOS-like terminals such as the more traditional
     Windows terminal)

   You can add `--args "first second"` at the end of the command to pass command
   line arguments `first` and `second` to `LongestCommonSubsequence`. See the
   gradle-adapted version of Recitation 1 for other examples of the `--args`
   flag.

   You can also use replace `run` with `clean` to remove all of the generated
   files (such as `.class` files). Nothing extra needs to be installed for these
   commands to work; the included gradle wrapper will automatically download a
   copy of the tool if needed.

   (Note that the starter code will not compile and run until you complete the
   exercise.)

2. Review the following algorithm that finds the LCS of two input strings. This
algorithm takes a brute force approach of generating all possible subsequences
and checking them. Note that, while this algorithm will get the correct answer,
a more efficient approach would be to use dynamic programming (a topic you will
learn in CS 1501).

       Longest Common Subsequence (first, second):
           Create an empty bag
           Put the first string into the bag
           Create variable longest (for the longest subsequence so far)
           Initialize longest to empty string
           While the bag is not empty:
               Remove a string from the bag, call it test
               If longest is shorter than test:
                   If test is a subsequence of second:
                       Set longest to test
                   Otherwise, if test is at least 2 characters longer than longest:
                       Generate new strings from test by removing a different single
                       character each time.
                       Add each of the newly-generated strings to the bag.
               Print the bag of strings that still need to be checked, for debugging
           Print out the longest subsequence

   To demonstrate how to generate new strings from a test string ("by removing a
   different single character each time"), consider the following example. If
   the test string you removed from the bag is `ABCD`, you want to add each of
   `BCD`, `ACD`, `ABD`, and `ABC` (all subsequences that are 1 character
   shorter).

   Note that `LongestCommonSubsequence.java` already contains a method,
   `isSubsequence`, to check whether one string is a subsequence of another.

3. Once you understand the algorithm from Step 2, read through
`LongestCommonSubsequence.java`, noting the 3 `TODO` comments. You will need to
complete these portions of the code.

4. At the first `TODO` comment, create a new reference variable named
`possibleSubsequences` for storing the bag and it assign it a value of `null`.

5. At the second `TODO` comment, create a new bag of strings, assign it to the
variable `possibleSubsequences`, and add the string `first` to the bag.

6. At the third `TODO` comment, implement the algorithm from Step 2.

7. Test the program to be sure it works. Below are pairs of strings and their
correct longest common subsequence:

   | First | Second      | LCS
   | ----- | -----       | -----
   | D     | ABC         |
   | AA    | ABA         | AA
   | AA    | AAA         | AA
   | AABC  | ABBC        | ABC
   | ABBC  | ABCC        | ABC
   | ABCC  | CABAC       | ABC
   | ABA   | AA          | AA
   | ABC   | CBACBA      | AB or BC or AC
   | ABC   | CBACBACBA   | ABC
   | ABC   | BCABCA      | ABC
   | ABCD  | DCBADCBA    | AB or AC or AD or BC or BD or CD
   | ABFCD | ADBAFDCBA   | ABFC or ABFD
   | ABFCD | ADBADCBA    | ABC or ABD
   | ABCDF | ADBADCBA    | ABC or ABD
   | ABADA | BADABAABDBA | ABADA

## Conclusion

In this exercise, you wrote client code using the ADT Bag to solve the LCS
problem. Writing client code is a crucial skill in Algorithms and Data
Structures 1 (and moving forward in your programming career). Throughout the
term, you should take the time to practice writing code that uses the data
structures presented, to improve your skills in solving problems using the
(often limited set of) operations available to you.
