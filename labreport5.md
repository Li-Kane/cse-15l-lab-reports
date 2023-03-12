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
4. Make the gradeServer take input not simply from the taskbar

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
        echo "The tests that failed were"
        grep "(TestListExamples)" results.txt
````





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
