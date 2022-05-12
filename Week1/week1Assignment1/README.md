# How the web browsers work?
 
 Here, we will try to see what happens when we enter a URL into a browser and hit Enter. In short, following steps take place:

 1. The browser looks up at the IP Address for the domain that you have entered.
 2. Browser initiates a TCP connection with the server.
 3. After the connection is made, a HTTP request is sent to the server.
 4. Server processes the request and sends back a response.
 5. The browser renders the response using several mechanisms and rules which will see in detail.
   
   ## Let's try to see with the help of a diagram of how the flow works:

Image here

   ## What is the main functionality of the browser?
    
  The main functionality of a web broswer is, I would say, fetch useful information from the World Wide Web on request by a user/ client, translate those files received from server and render it to the user. The browser does it in fraction of seconds. It does so in the steps explained concisely above. In this answer, we will focus more on the rendering part like the HTML and CSS parsers, rendering engines, layout and painting and many more things. 

  