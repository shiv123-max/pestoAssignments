# How the web browsers work?

Here, we will try to see what happens when we enter a URL into a browser and hit Enter. In short, following steps take place:

1.  The browser looks up at the IP Address for the domain that you have entered.
2.  Browser initiates a TCP connection with the server.
3.  After the connection is made, a HTTP request is sent to the server.
4.  Server processes the request and sends back a response.
5.  The browser renders the response using several mechanisms and rules which will see in detail.

## Let's try to see with the help of a diagram of how the flow works:

![lya7b81ow94pniln3aif](https://user-images.githubusercontent.com/56493775/168250913-ec514172-ea1b-4a06-b3eb-75786179807d.jpg)

## What is the main functionality of the browser?

The main functionality of a web broswer is, I would say, fetch useful information from the World Wide Web on request by a user/ client, translate those files received from server and render it to the user. The browser does it in fraction of seconds. It does so in the steps explained concisely above. In this answer, we will focus more on the rendering part like the HTML and CSS parsers, rendering engines, layout and painting and many more things.

## High Level Components of a browser

The browser's high level architecture consists of following components:

1.  **The UI** - This includes the whole UI consisting of address bar, all the buttons, bookmarking options and many more things. It varies from browser to browser.
2.  **The browser engine** - It simply acts a link between the UI and the rendering engine.
3.  **The rendering engine** - It is used to display requested content on the browser. If the requested content is HTML, the rendering engine parses HTML and CSS and displays the parsed content on the screen.
4.  **Networking** - It used for different types of network calls like HTTPS, HTTP. It might have different implementations in different types of platforms.
5.  **UI Backend** - It is used for drawing out basic widgets like boxes and windows. It is not platform specific- it is more of a generic implementation.
6.  **JavaScript Interpreter** - It is used to parse and execute JavaScript code.
7.  **Data Storage** - This is a layer which is used for persistence. The browser might need to save some data locally such as cookies or localStorage is also a very good example.
   
 ## Rendering engine and its uses

 So, the main use of rendering engine is to of course render content requested by the user. But, how does it perform this task. The image below explains the basic flow of a rendering engine:

![Screenshot 2022-05-13 115549](https://user-images.githubusercontent.com/56493775/168249985-3a6e023d-e1c8-4f9b-802d-42db3034a5db.png)

 It includes four steps:
 1. The requested HTML content is parsed in small fragments, including all the CSS files. The HTML elements are then transformed into DOM nodes to form the **DOM Tree**.
   
 2. After the DOM tree is created, the engine also creates a **render tree**. This tree has all the information regarding the order in which the elements will be dispalyed. It doesn't add to itself those DOM nodes which are not instantly needed.
 
 3. After the render tree is created, the browser goes through the **layout process**. The process of finding out the values for the elements to find out there desired position is known as layout process.
 
 4. The final step is to paint the screen, in which we traverse the render tree and apply **paint()** method which paints each node in the render tree using the UI backend layer.

## HTML and CSS Parsers  
 
 ### HTML Parser
  
  The job the HTML parser is to parse the HTML markup into a parse tree.
  Parsing is generally done on context free grammar. But, HTML cannot easily be defined by a CFG that the parsers require. 
  HTML cannot be parsed by regular top down or bottom up parsers due to:
  1. Forgiving nature of the language 
  2. Presence of dyanmic code
   
  So, the browsers create custom parsers for parsing HTML. The algorithm consists of two stages: **tokenization and tree construction**. This is the basic flow of HTML parser:

![Screenshot 2022-05-13 121731](https://user-images.githubusercontent.com/56493775/168250051-e36835db-4303-4fb5-8f33-b336637341b5.png)

  **Tokenization** is like the lexical analysis phase of the compiler. It parses the input tokens. The algorithm is designed in a form of state machine. Every state takes on or more characters of the input stream and updates the next state according to one of those characters.

 **Tree Construction Algorithm**:

  The document object is created when the parser starts running. In the tree construction phase, the root of DOM tree i.e. the Document will come into the picture and new elements will get added to it. Each node released by the tokenizer will be processed by the tree constructor. For every token, the specification defines which DOM element is relevant to it and will be created for this token.

  ### CSS Parsing

  Unlike HTML, CSS is a context free grammar and is parsed using the types of parsers commonly used. 
  Let's see example of WebKit CSS parser- 

  WebKit uses Flex and Bison parser generators to create parsers automatically from CSS files. Bison creates a bottom-up shift reduce parser. The CSS file is parsed into a StyleSheet object. Each object contains CSS rules. The CSS rule objects contain selectors and declaration objects and other objects corresponding to CSS grammar. The following image is an example:

![Screenshot 2022-05-13 123654](https://user-images.githubusercontent.com/56493775/168250095-6055d166-bdfe-4be5-ae78-e6b4c905e363.png)

## Script Processors and Order of Script Processing

The whole model of the web is more or less synchronous. The script are parsed and executed when the parser reaches a `<script>` tag. The parsing of the document stops until the script has been executed. External scripts are also fetched synchronously. Nowadays, in HTML5, authors can add "defer" attribute to a script which doesn't halt the parsing of the document and is executed after the whole document is parsed. We can also mark the script asynchronous so it will be parsed and executed by a different thread altogether.

### Speculative Parsing

Many browsers do this optimization. When the execution of the script is going on, another thread parses the rest of the document ans lists down the resources needed to be loaded and loads them. Parallel connections are made and overall speed is increased. The speculative parser only parses external scripts, style sheets and images; it can't modify the DOM tree- that is the task of the main parser.

### Style Sheets

It seems from the upper level that styles sheets don't change the DOM tree. So, we assume that there is no reason to wait for styles sheets and stop document parsing. But, sometimes what happens is scripts ask for style information during parsing. Now, if the styles is not loaded and parsed, the script execution will be faulty and will cause lots of problems.Different browsers use different techniques to tackle this problem. For example, Firefox blocks all scripts when there is a style sheet that is still being loaded and parsed. 

## Render Tree Construction
 
 Along with DOM tree, browser constructs another tree known as render tree. This tree is of visual elements in the order in which they will be displayed. This tree is then used to paint the contents in the correct order. The elements in the render tree are known as "frames" or "renderers". Each renderer represents a rectangular area corresponding to a node's CSS box. It includes all geomteric information like width, height and position. The box type is affected by the "display" value of the style attribute that is relevant to the node.

 ### Relationship between DOM tree and render tree

 The realtion between DOM tree and render tree is not one to one. Not all elements from DOM tree are added to the render tree. Non-visual elements like "head" and elements with `display: none` property will not be added to the render tree. Sometimes, the DOM elements can be very deeply nested like a "select" tag. Also, sometimes text is broken into several lines to fit the width of screen. All these scenarios account for addition of extra "renderers" in the DOM tree.
  
 The picture below shows an example of render tree construction from DOM tree: 

![Screenshot 2022-05-13 133832](https://user-images.githubusercontent.com/56493775/168250164-81b4db50-29f7-48cb-a358-492b1de8fa18.png)

 But, before adding elements in the render tree, we should also know the style properties of each element. This is done using style computation. Many techniques are applied for style compuatation and many optimisations are also applied. For example, WebKit shares style data by referencing style objects. They are shared only if the fulfill some conditions.

 Firefox, on the other hand, uses extra trees for easier style computation. They are the rule tree and the style context tree. The style contexts contains the end values (final calculated values). The rule tree enables to save time by sharing these values between nodes. After the style context tree is created, it is divided into structs. These structs contain information regarding a certain category. This is how Firefox handles the style computation as it can get lenghty very easily.

 The rules are then applied in correct cascading order if there is more than one definition of a style. The cascade order is(from low to high):
 1. Broswer Declarations
 2. User normal declarations
 3. Author normal declarations
 4. Author important declarations
 5. User important declarations

## Layout and Painting

 ### Laoyout
  
 When the renderer is created and added to the tree, it does not have a position and size. Calculating these values is called laoyout or reflow.

 As HTML has a flow based layout model, meaning that most of the time it is possible to compute the geometry in a single pass. Layout is a recursive process. It begins from `<html>` element and travels all hierarchies and computes geometric information for each renderer that requires it. Root renderer's position is 0,0 and its dimensions are the viewport.
 We don't want to calculate full layout for even a small change. To handle this, browser use "dirty bit" system. A renderer that is changes or added marks itself and its children as "dirty" needing layout calculation.
 So, in this way, layout process works in two ways - Globally and incrementally. Global layout is fired when suppose a global style has been changed or the screen has been resized. Incremental layout is fired when only the dirty renderers are laid out. It is done asynchronously.

### Painting

In this stage, the render is tree is traversed and the renderer's *paint()* method is called to display content on the screen. Painting makes use of the UI infrastructure component.

**Global and incremental Painting**
Like layout, painting can also be global - the entire tree is painted - or incremental. In incremental painting, some renderers change without affecting the entire tree. The changed renderer's rectangle becomes invalid. The OS then sees this as a "dirty region" and generates a "paint" event. The OS then merges several regions into one very cleverly. 

There is a specific order for the painting process. The order is: 
1. Background Color
2. Background Image
3. Border
4. Children
5. Outline

After the painting process is completed, the content that the user requested by typing a URL and hitting Enter finally gets rendered on the screen. All of this happens within a fraction of seconds!

