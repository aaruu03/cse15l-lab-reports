# Lab Report 5

## grade.sh

``` 
rm -rf student-submission 
git clone $1 student-submission 
cp TestListExamples.java student-submission/
cd student-submission 
if [[ -f $"TestListExamples.java" ]] && [[ -e $"TestListExamples.java" ]]
then
    echo "File exists!"
else
    echo "File cannot be found"
    echo "Grade 0/5"
    exit 1
fi

javac -cp ".:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar" *.java > return.txt 

if [[ $? -eq 0 ]]
then
    echo "Program has compiled"
else
    echo "Program did not compile"
    echo "Grade 1/5"
    exit 2
fi
java -cp ".:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar"  org.junit.runner.JUnitCore TestListExamples > results.txt 
if [[ $? -eq 0 ]]
then
    echo "Tests Passed"
    echo "Grade 5/5"
    cat results.txt
    exit    
else 
    echo "Tests Failed"
    echo "Grade 3/5"
    cat results.txt
    exit
fi
```

## Script in action

This is the script working on different student submissions. Based on the student's code, the script outputs a grade on the local host browser.

### https://github.com/ucsd-cse15l-f22/list-methods-compile-error

Here, the code fails to compile due to a missing semi-colon. Therefore, the grading script outputs a grade of 1. 

### https://github.com/ucsd-cse15l-f22/list-methods-corrected

In this submission, the code does not have any errors, compiles successfully, and is able to pass all the tests. This is the perfect case, which is why the grading script outputs a grade of 5–full points.

### https://github.com/ucsd-cse15l-f22/list-examples-subtle

In this submission, the code compiles successfully, and passes some of the tests. However, due to the fact that not all tests were passed, the grading script outputs a grade of 3.



## Script tracing

Here, we are going to look  sat one of the screenshots from earlier and trace the steps the script goes thorugh while running and grading the student's code.

We are going to be looking at the first screenshot— for https://github.com/ucsd-cse15l-f22/list-methods-compile-error.

First we remove the file directory, giving us a return code of 0 before we move on to cloning the student submission and copying all files over.

We then cd into the clone/copy of the directory and files.

Next, we go through each if/else statement to check for errors or return.exit statments.  We are able to see that the file has been found. However, becuase the file does not compile, the grading script returns the statement that the program does not compile, along with the grade 1/5 and then exits. This means that none of the other lines of code run, so we do not run any of the tests, etc.