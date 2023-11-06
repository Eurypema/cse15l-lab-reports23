# Lab Report 2


## Part 1


### 1.1: Code


The following code performs what is requested:

```
import java.io.IOException;
import java.net.URI;
import java.net.URLDecoder;
import java.nio.charset.StandardCharsets;

class StringHandler implements URLHandler {
    private final StringBuilder messages = new StringBuilder();
    private int messageCount = 0;
    
    public String handleRequest(URI url) {
        if ("/".equals(url.getPath())) {
            return messages.toString();
        } else if ("/add-message".equals(url.getPath())) {
            String query = url.getQuery();
            if (query != null) {
                String[] queryParams = query.split("=");
                if (queryParams.length == 2 && "s".equals(queryParams[0])) {
                    messageCount++;
                    String decodedMessage = URLDecoder.decode(queryParams[1], StandardCharsets.UTF_8);
                    if (messages.length() > 0) {
                        messages.append("\n");
                    }
                    messages.append(messageCount).append(". ").append(decodedMessage);
                    // Return the updated messages
                    return messages.toString();
                }
            }
            return "Please provide a message in the format '/add-message?s=<message>'";
        } else {
            return "404 Not Found!";
        }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);
        Server.start(port, new StringHandler());
    }
}
```
### Screenshots


The following screenshots demonstrate the code in action:

![image](https://github.com/Eurypema/cse15l-lab-reports23/assets/103284133/27c63b07-536f-4702-b2fb-85af4e285621)

Methods Called: 
1. handle(HttpExchange exchange): This method in the ServerHttpHandler class is called when the server receives a request. HttpExchange encapsulates a HTTP request received and a response to be generated in one exchange.

Relevant Arguments and Fields: 
1. The HttpExchange exchange argument contains the request information. The relevant method called on this object is getRequestURI(), which returns the URI of the request. In this case, it would return a URI object that represents the URL http://localhost:9001/add-message?s=Hello. 
2. Inside the handleRequest(URI url) method of the StringHandler class (as we have modified the provided Handler class to work with strings), the url parameter would be the aforementioned URI. 
3. The StringBuilder messages field holds the running string that keeps the concatenated messages. Initially, it is empty. 
4. The int messageCount field tracks the number of messages that have been added. Initially, it is 0. 

Changes in Class Fields: 
1. messageCount: This value would be incremented from 0 to 1 because a new message has been received.
2. messages: The StringBuilder which initially is empty (""), will have the new message appended to it in the following format: "1. Hello". This is because the handler will decode the message, prepend the incremented message count and a dot, and then add it to the running string.



![image](https://github.com/Eurypema/cse15l-lab-reports23/assets/103284133/b0c2c48f-5b33-4a81-9e02-712693bd9289)


Methods Called: 
1. handle(HttpExchange exchange): As with the previous request, this method is called to handle the incoming HTTP request.
Relevant Arguments and Fields:
1. The HttpExchange exchange argument is provided with the request details. The getRequestURI() method is called on it, returning a URI that represents the URL http://localhost:9001/add-message?s=How%20are%20you.
3. The url parameter within the handleRequest(URI url) method receives this URI.
4. The StringBuilder messages field contains the previous messages. Before this request, it should have "1. Hello".
5. The int messageCount field holds the count of messages which, before this request, should be 1.

Changes in Class Fields: 
1. messageCount: This would be incremented by one to 2 because another new message has been received.
2. messages: The StringBuilder, previously holding "1. Hello", will now have "2. How are you" appended after a newline character, resulting in:
   ```
   1. Hello
   2. How are you
   ```


