# CSE 15L Lab Report 2 #  
*Kane Li, 27th January 2023*
----
This Lab Report covers my learning of StringServer, debugging, and a reflection for what I've learned during weeks 2 and 3 in CSE15L. 

## Part 1 ##
In creating StringServer, I followed a similar model as to that of Number Server during the week 2 lab. I kept the Server.java class untouched, as well as the StringServer class. What I did change was the Handler class and its method handleRequest, where I changed Hander's field to that of a string of what to be printed, and changed handleRequest so that it can store url inputs.

<img width="435" alt="image" src="https://user-images.githubusercontent.com/122249106/215302280-a6c3a5df-9a19-4df1-bbda-66f362a13a04.png">
Here, the url seen at the top of the page can be split into three main parts:

1.  ` localhost:4000 `: When the .java file is run after compiling, you need to input an int argument for the port number which the server can use. I chose to use 4000, therefore localhost:4000 tells me my computer is hosting the server on port 4000.
2. ` /add-message `: The main method in class StringServer starts a server with an infinite loop that checks the URL bar, which is passed as a URI parameter into the handleRequest method. If the url bar is found to contain the keywords "add-message", it will then check for a query argument in the url, storing each argument in an array of String[] called parameters.
3. ` ?s=this%20is%20message%20two `: "?" represents a query, while and each equal sign (=) divides the query into elements of the list, such that parameters stores \[s, this is message two]. The reason this is message two is shown as this%20is%20message%20two is because spaces are difficult to show in url format, and are denoted by %20. Handlerequest then does two if comparisons, where if the first element of paramters is "s", and there is a next element, then it will add the next element to an output variable shared by the Handler class, along with a new line. Then, the new value of output is returned to be printed to the server. The server also shows an additional line of "this is the first message" even though I only typed "this is message two" because the server will save strings which you previously passed to it, so it is storing the previous time I had "this is message one" in the url bar and added that in a new line with my second dmessage.

<img width="455" alt="error demonstration" src="https://user-images.githubusercontent.com/122249106/215302241-859ae3e6-961f-4738-ae88-c0b4e28283f1.png">

1. ` localhost:4000`: similar to the first screenshot, localhost:4000 is essentially always going to be the same, as long as the server is being run on the current computer. The number value may change however, depending on which port is passed into the StringServer main method.
2. ` /do-something-cool?s=this=is=not=cool=this=is=lame `: for this part of the URI, because the if statements that check the query for "s" and an argument won't run unless the URI contains "/add-message", then the output will simply jump to return "404 not found!". 

Here is the code for StringServer.java:

```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String output = "";
    
    public String handleRequest(URI url) {
        if(url.getPath().contains("/add-message")){
            String[] parameters = url.getQuery().split("=");
            if(parameters[0].equals("s")){
                if(parameters.length > 1){
                    output += parameters[1] + "\n";
                    return output;
                }
            }
        }
        return "404 Not Found! Try using /add-message?s=<String>";
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

## Part 2 ##

**Failure-inducing input**

Inputting an array with multiple of the same values is a failure-inducing input for the buggy averageWithoutLowest() method in ArrayExamples.java:
```
  @Test
  public void testAverageWithoutLowest(){
    double[] input3 = {1.0, 1.0, 2.3, 3.5, 2.2};
    assertEquals(2.6667, ArrayExamples.averageWithoutLowest(input3), 0.0005);
  }
}
```
Produces the error symptom:  
<img width="796" alt="image" src="https://user-images.githubusercontent.com/122249106/215309010-31bdf03e-b8a7-46cc-af1a-8abf123c2c81.png">

**Non failure-inducing inputs**

Simple arrays without any repeating elements do not induce any failures, yet averageWithoutLowest() is still buggy:
```
  @Test
  public void testAverageWithoutLowest(){
    double[] input1 = {1.0, 2.0, 3.0};
    assertEquals(2.5, ArrayExamples.averageWithoutLowest(input1), 0.0005);
    double[] input2 = {0.5, 1.5, 2.5, 3.5};
    assertEquals(2.5, ArrayExamples.averageWithoutLowest(input2), 0.005);
  }
}
```
Produces the symptom:  
<img width="798" alt="image" src="https://user-images.githubusercontent.com/122249106/215308803-459769c8-3b04-4792-b30e-5254657725a3.png">

**Explanation and fix**  
Here is the buggy program, with a comment (//) that shows where the buggy line is:
```
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - 1);  //The bug is on this line
  }
```
The main issue with this program is that on the line `return sum / (arr.length - 1)` it is essentially assuming that there will always only be one lowest value through subtracting by one, when in fact there can be multiple. So with the failure inducing input of {1.0, 1.0, 2.3, 3.5, 2.2), while it was getting the proper sum of 8.0 through ignoring any values that are 1.0, because arr.length - 1 would give 4, it the return value would be 8.0/4, which ends up to 2.0. Meanwhile, the method should be getting 2.666667, as there are two repeat 1.0s, so the computation should actually be 8.0/3, which is 2.66666667. To fix this, I simply created a new integer variable called lowestCount, and in the second for:each loop I incremented it everytime we came across a value that matched lowest, so that I could change the error line to `return sum / (arr.length - lowestCount);` so that the sum is always divided by the correct value.  
Here is the fixed code:
```
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    int lowestCount = 0; //var to track duplicate lowest
    double sum = 0; 
    for(double num: arr) {
      if(num == lowest) {lowestCount++; } //increments at duplicates
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - lowestCount); //fixed bug
  }
  ```
  
  ## Part 3 ##
  Knowledge wise, the week 3 paper on [Student Correctness](https://drive.google.com/file/d/1zbMVZxsI1zOBPhSsvBi4kB5dPJuxyOJh/view) has shown me to some extent that my standards for programming are actually a bit subpar, where only doing the main task doesn't mean a program is correct. It kinda shows to me how with the way assignments are structured, it makes it easy for me to sometimes target my programs toward very specific goals, instead of making it comprehensive and whole. Of course, they can never be perfect, but it motivates me to try to conquer roadblocks in my coding rather than just swerve around them. That it isn't a waste of time to try and figure out an assignment.
  
  Coding wise, I learned how standardized a url really is, and the various parts that comprise it. For instance, the domain tells us which server is hosting it, there is a path to determine which website to navigate to, and even various symbols such as query(?). It really shows to me just how much of computer science is standardized, and that accomplishing goals in programming doesn't always require lots of math of logic. Rather, it is just as important (and maybe even more important) to know which tools to use.
