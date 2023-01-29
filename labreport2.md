# CSE 15L Lab Report 2 #  
*Kane Li, 27th January 2023*

----
This Lab Report covers my learning of StringServer, debugging, and a reflection for what I've learned during weeks 2 and 3 in CSE15L. 

## Part 1 ##
In creating StringServer, I followed a similar model as to that of Number Server during the week 2 lab. Keeping the same main method, I mainly changed the public field of the Handler class, as well as the logic and returns of the handleRequest method. Here are two quick examples:

<img width="435" alt="image" src="https://user-images.githubusercontent.com/122249106/215302280-a6c3a5df-9a19-4df1-bbda-66f362a13a04.png">
Here, the url seen at the top of the page can be split into three main parts:

1.  ` localhost:4000 `: When the .java file is run after compiling, you need to input an int argument for the port number which the server can use. I chose to use 4000, therefore localhost:4000 tells me my computer is hosting the server on port 4000.
2. ` /add-message `: The main method in class StringServer starts a server with an infinite loop that checks the URL bar, which is passed as a URI parameter into the handleRequest method. If the url bar is found to contain the keywords "add-message", it will then check for a query argument in the url, storing each argument in an array of String[] called parameters.
3. ` ?s=this%20is%20message%20two `: "?" represents a query, while and each equal sign (=) divides the query into elements of the list, such that parameters stores \[s, this is message two]. The reason this is message two is shown as this%20is%20message%20two is because spaces are difficult to show in url format, and are denoted by %20. Handlerequest then does two if comparisons, where if the first element of paramters is "s", and there is a next element, then it will add the next element to an output variable shared by the Handler class, along with a new line. Then, the new value of output is returned to be printed to the server.

<img width="455" alt="error demonstration" src="https://user-images.githubusercontent.com/122249106/215302241-859ae3e6-961f-4738-ae88-c0b4e28283f1.png">


Here is the code for String Server:

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
