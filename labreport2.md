# CSE 15L Lab Report 2 #  
*Kane Li, 27th January 2023*

----
This Lab Report covers my learning of StringServer, debugging, and a reflection for what I've learned during weeks 2 and 3 in CSE15L. 





Here is the code for String Server:
## Part 1 ##
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
