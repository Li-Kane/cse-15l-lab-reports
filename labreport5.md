# CSE 15L Lab Report 5 #  
*Kane Li, 10 March 2023*
----
For this lab report, I decided to return to the week 6 lab to try and create a more sophisticated grading script, which interfaces with being run on a server as well.
My previous code looked somehwere along these lines: 

````bash
CPATH='.;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar'

#erase previous files, clone, and move the tester
rm -rf student-submission
git clone $1 student-submission
echo 'Finished cloning'
cd student-submission
if [[ -f ListExamples.java ]]
then
    echo "ListExamples is found"
else
    echo "Error incorrect File"
    exit 1
fi

cp ../TestListExamples.java .
javac -cp $CPATH *.java
if [[ $? -eq 0 ]]
then
    echo "Code Compiles"
else
    echo "Exit Code: " $?
    echo "Compile Error!"
    exit 1
fi

#Check compilation, and run, give score of 100 or 0
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples > results.txt
RESULTS="`grep -w Tests results.txt`"
if [[ -n $RESULTS ]]
then
    echo "You failure! Here: "
    echo $RESULTS
 else
    echo "All tests passed! 100%"
fi
````

A couple weaknesses of this initial skeleton is that currently, it only checks whether the file exists in the expected area, whether it compiles, and whether it completely passes or if there are some files. It fails to cover a variety of cases, such as complete fails or partial fails, and doesn't give very comprehensive feedback that could help the student understand where the issue was. Addditionally, the grading system is simply a 100 or 0, lacking a score calculation based on how many tests passed to how many tests were ran. Finally, the grading script is incomplete as it fails to check the filter() method and its outputs Therefore I have a couple main goals:
1. Check the method signatures in the grading script
2. Change the output to give a percentage based score depending on the amount of tests passed and failed
3. Add comprehensive feedback which gives insight into what type or error and where it occurred

### Part 1: Implement checks for the method isgnatures ###
For this section, I implemented grep checks for whether there were exact matches for the required necessary method signatures in cases where one or both methods are incorrect or missing. Here is the added segment:
````bash
        FILTERSIGNATURE="`grep "static List<String> filter(List<String> list, StringChecker sc)" ListExamples.java`"
        MERGESIGNATURE="`grep "static List<String> merge(List<String> list1, List<String> list2)" ListExamples.java`"
        if [[ -z $FILTERSIGNATURE && -z $MERGESIGNATURE ]]
            then
                echo "The method signature for filter() AND merge() are wrong or missing! Check again!"
                exit 1
            elif [[ -z $FILTERSIGNATURE ]]
            then
                echo "The method signature for filter() is wrong or missing! Check again!"
                exit 1
            elif [[ -z $MERGESIGNATURE ]]
            then
                echo "The method signature for merge() is wrong or missing! Check again!"
                exit 1
        fi
````
I inserted this section into the if statement that checks whether the code compiles, as that is where the code would throw an error if the methods were off or missing. The grep simply searches for the exact match, and the if statements simply check whether the variables that store each grep output are empty. If either method signature is off or missing, grep fails the direct match, making the variable empty, so when the if statement checks whether the variables are empty through `-z`, then if there are errors then the script will make sure to inform the submitter. Here are some quick examples:

When elements of filter() are in the wrong order: [https://github.com/ucsd-cse15l-f22/list-methods-signature.git](https://github.com/ucsd-cse15l-f22/list-methods-signature.git)  
<img width="562" alt="image" src="https://user-images.githubusercontent.com/122249106/224525524-88a8b46a-a9a8-4e23-91d7-15061219593f.png">

When the filter() method is missing (I made this repository myself to as a tester): [https://github.com/Li-Kane/LabReport5MissingMethod.git](https://github.com/Li-Kane/LabReport5MissingMethod.git)  
<img width="569" alt="image" src="https://user-images.githubusercontent.com/122249106/224525633-f9dc8a22-b91a-43e5-a13e-9eda36874c11.png">

### Part 2: Change the output to give a percentage score depending on the amount of tests passed and failed ###
Inspired by the use of ChatGPT to accomplish this task during lecture, I sought out to accomplish this myself. The overall code segments I implemented look like this: 
````bash
#awk command that defines calc() for floating-point division
calc(){ awk "BEGIN { print $*}"; }

#a bit of code later...

        #get the necessary values to calculate the percent
        TESTSRUN=`egrep -o "Tests run: [0-9]+" results.txt | egrep -o "[0-9]+"`
        FAILURES=`egrep -o "Failures: [0-9]+" results.txt | egrep -o "[0-9]+"`
        let PASSED=$TESTSRUN-$FAILURES
        FRACTION=$(calc $PASSED/$TESTSRUN)
        PERCENT=$(calc $FRACTION*100)
        echo "You failed" $FAILURES "tests out of" $TESTSRUN to get a $PERCENT"%"
````
`calc(){ awk "BEGIN { print $*}"; }`: The calc() section was inserted towards the front of the script, to define a function which is used for integer division, which is surprisingly hard to accomplish in bash. In terms of the process, I originally tried to use bc, but I didn't want to have to install anything so that others could use my code, and bc wouldn't be recognized unless it was installed. Therefore, I eventually stumbled upon awk. Essentially, it simply works by defining a function through `calc(){;}`, where whenever I call calc I want to run `awk "BEGIN {print$*}"`. BEGIN simply allows it so that the command `print$*`is actually run, and `$*` allows for it to take in all the arguments passed in and conduct arithmetic operations.

The second chunk of getting the necessary values is simply doing a grep expression to filter out the specific digits of the amount of tests run and tests failed as the JUnit output is stored in results.txt. Then, it simply subtracts the failures from amount run to calculate how many were passed, and stores the amount that passed divided by the amount of tests into a variable, which is then turned into a percent through being multiplied by 100. This code chunk was inserted at the if statement that checks whether there was matching JUnit output for errors, such that the percent is only calculated if there are JUnit errors. Here are some examples:

When both methods fail: [https://github.com/ucsd-cse15l-f22/list-methods-lab3.git](https://github.com/ucsd-cse15l-f22/list-methods-lab3.git)  
<img width="573" alt="image" src="https://user-images.githubusercontent.com/122249106/224526312-926134ce-01fd-48e2-b803-34e456ac8688.png">

When both methods have a correct implementation: [https://github.com/ucsd-cse15l-f22/list-methods-corrected.git](https://github.com/ucsd-cse15l-f22/list-methods-corrected.git)  
<img width="587" alt="image" src="https://user-images.githubusercontent.com/122249106/224526342-737b0fd3-3259-4263-940e-011a8f3dbed9.png">

When merge() passes but filter() fails (self-made): [https://github.com/Li-Kane/partialPassLabReport5.git](https://github.com/Li-Kane/partialPassLabReport5.git)  
<img width="577" alt="image" src="https://user-images.githubusercontent.com/122249106/224526375-70d32d0c-a62c-42c6-b0f8-f0b1e1e1b487.png">

When there are subtle errors on both: [https://github.com/ucsd-cse15l-f22/list-examples-subtle.git](https://github.com/ucsd-cse15l-f22/list-examples-subtle.git)  
<img width="575" alt="image" src="https://user-images.githubusercontent.com/122249106/224526420-bb9d5fcf-cf4e-410c-b13a-cf1df3b93c7a.png">

### Part 3: Add comprehensive feedback of when and where the error occurred ###
Ultimately, this section mainly consisted of creating clearer feedback, through a couple of changes:
1. `"Unable to find ListExamples.java! Make sure it is not inside any other directories and has proper naming! You get a 0%"`: I added this portion in the if statement that checks if ListExamples.Java could be found.
2. `echo "The tests that failed were"` and `grep "(TestListExamples)" results.txt`. These are ran when the script detects a failure, and simply gives the method names that failed. I opted not to print the whole JUnit output as most tests on gradescope don't really seem to do that, so I want the student to figure it out themselves.
3. `String instructions = "Welcome to lab3 autograder!\n" + "To begin, please enter your arguments in the url bar after the domain as:\n" + "/grade?repo=linkToYourRepository.git\n" + "----------------------------------------------------------------------------------\n\n";`: I added this portion to GradeServer.java inside the handleRequest() method of the Handler Class, and it simply is added to the start of each return in order to offer easier understanding of how to use the autograder.

Here are the remaining examples:

When there is a compile error due to syntax: [https://github.com/ucsd-cse15l-f22/list-methods-compile-error.git](https://github.com/ucsd-cse15l-f22/list-methods-compile-error.git)  
<img width="577" alt="image" src="https://user-images.githubusercontent.com/122249106/224526824-233c9035-e594-4c3b-901e-9787e01775b3.png">

When the filename is off: [https://github.com/ucsd-cse15l-f22/list-methods-filename.git](https://github.com/ucsd-cse15l-f22/list-methods-filename.git)  
<img width="590" alt="image" src="https://user-images.githubusercontent.com/122249106/224526840-b01dceaf-1fe0-4a0b-bb4d-6716c7cb13fa.png">

When ListExamples is nested inside a directory: [https://github.com/ucsd-cse15l-f22/list-methods-nested.git](https://github.com/ucsd-cse15l-f22/list-methods-nested.git)  
<img width="582" alt="image" src="https://user-images.githubusercontent.com/122249106/224526880-a227ee7b-5a37-45b9-81d5-2a817d232cf3.png">

### Conclusion ###
Ultimately, I really enjoyed the grading script because it combines a wide variety of aspects such as servers, github, bash, and scripts towards a practical and common usage. While it still has many improvements to make, being able to create a program like this with so much more potential is something I never would have been able to do without this class. I very much enjoyed seeing how computer science lessons learned can be applied directly on the computer, and the freedom given to explore. Anyway, here is my final code:


````bash
CPATH='.;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar'
#awk command that defines calc() for floating-point division
calc(){ awk "BEGIN { print $*}"; }

#erase previous files, clone, and move the tester
rm -rf student-submission
git clone $1 student-submission 
echo 'Finished cloning'
cd student-submission

if [[ -f ListExamples.java ]]
   then
       echo "ListExamples is found"
   else
       echo "Unable to find ListExamples.java! Make sure it is not inside any
other directories and has proper naming! You get a 0%"
       exit 1
fi

cp ../TestListExamples.java .
javac -cp $CPATH *.java 2> compile.txt

#Check compilation, and run, give score of 100 or 0
if [[ $? -eq 0 ]]
   then
       echo "Compilation successful!"
   else
       #Check the method signatures
       #FILTER="`grep "static List<String> filter(List<String> list, StringChecker sc)" ListExamples.java`"
       #echo "Filter:" $FILTER
       FILTERSIGNATURE="`grep "static List<String> filter(List<String> list, StringChecker sc)" ListExamples.java`"
       MERGESIGNATURE="`grep "static List<String> merge(List<String> list1, List<String> list2)" ListExamples.java`"
       if [[ -z $FILTERSIGNATURE && -z $MERGESIGNATURE ]]
           then
               echo "The method signature for filter() AND merge() are wrong or missing! Check again!"
               exit 1
           elif [[ -z $FILTERSIGNATURE ]]
           then
               echo "The method signature for filter() is wrong or missing! Check again!"
               exit 1
           elif [[ -z $MERGESIGNATURE ]]
           then
               echo "The method signature for merge() is wrong or missing! Check again!"
               exit 1
       fi
       echo "Compile Error! You get a 0%, and here is the exit code and error:" $?
       cat compile.txt
       exit 1
fi
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples > results.txt
RESULTS="`grep -w Tests results.txt`"
echo -e "----------------------------------------------------------------------------------\n"

if [[ -n $RESULTS ]]
   then
       #get the necessary values to calculate the percent
       TESTSRUN=`egrep -o "Tests run: [0-9]+" results.txt | egrep -o "[0-9]+"`
       FAILURES=`egrep -o "Failures: [0-9]+" results.txt | egrep -o "[0-9]+"`
       let PASSED=$TESTSRUN-$FAILURES
       FRACTION=$(calc $PASSED/$TESTSRUN)
       PERCENT=$(calc $FRACTION*100)
       echo "You failed" $FAILURES "tests out of" $TESTSRUN to get a $PERCENT"%"
       echo "The tests that failed were"
       grep "(TestListExamples)" results.txt
   else
       echo "All tests passed! 100%"
fi
````
