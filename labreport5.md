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
