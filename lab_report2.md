A web server called `String Server`
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    ArrayList<String> msg = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format(msg.toString());
        } else if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    msg.add(parameters[1]+"\n");
                }
                return String.format(msg.toString().replace("[","").replace("]","").replace(", ",""));
        }else {
            return "404 Not Found!";
        }
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
![Image](add_msg1.png)
* This is the first screenshot using `/add-message`.
* handleRequest method is called when users input new URI.
* Relevant arguement to the method is the input URI and value of that argument is `new URI("http://localhost:2048/add-message?s=Hello")`
* The query is stored in the arrayList and the size of the arrayList is resized.

![Image](add_msg2.png)
* This is the second screenshot using `/add-message`.
* handleRequest method is called when users input new URI.
* Relevant arguement to the method is the input URI and value of that argument is `new URI("http://localhost:2048/add-message?s=How%20are%20you")`
* `%20` in the URL represents encoded space.
* The query is stored in the arrayList and the size of the arrayList is resized.

