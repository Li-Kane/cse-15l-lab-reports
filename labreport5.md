# CSE 15L Lab Report 5 #  
*Kane Li, 10 March 2023*
----
For this lab report, I decided to return to the week 6 lab to try and create a more sophisticated grading script, which interfaces with being run on a server as well.
My previous code looked somehwere along these lines: 

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

A couple weaknesses of this initial skeleton is that currently, it only checks whether the file exists in the expected area, whether it compiles, and whether it completely passes or if there are some files. It fails to cover a variety of cases, such as complete fails or partial fails, and doesn't give very comprehensive feedback that could help the student understand where the issue was. Addditionally, the grading system is simply a 100 or 0, lacking a score calculation based on how many tests passed to how many tests were ran. Finally, the grading script is incomplete as it fails to check the filter() method and its outputs Therefore I have a couple main goals:
1. Fix the grading script to also check the filter() method
2. Change the output to give a percentage based score depending on the amount of tests passed and failed
3. Add comprehensive feedback which gives insight into what type or error and where it occurred
4. Make the gradeServer take input not simply from the taskbar
