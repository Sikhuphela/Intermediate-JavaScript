# Intermediate-JavaScript
WEEK 1: WINDOW EVENTS IN JAVASCRIPT
          
   Day 1: Frames and Windows
   Popup Usage:
Popups are used to display additional content or documents to users without closing the main window.
Popup Blocking:
Browsers have implemented popup blocking to protect users from abusive and unwanted.
Popup Blocked:
If pop-ups are blocked by the browser settings or in the current context (e.g., if the code is executed without user interaction), the window.open() method may not work, and nothing will happen when you run the code.
// popup blocked
window.open('https://javascript.info');popups.
Popup Allowed: If pop-ups are allowed by the browser settings or in the current context (e.g., if the code is executed in response to a user action like clicking a button), the window.open() method will open a new window/tab with the URL 'https://javascript.info'.
// popup allowed
button.onclick = () => {
window.open('https://javascript.info');
};
Popup Behaviour:
Popups are blocked if not triggered by user interactions like onclick.
Browser settings and context determine whether popups are allo

Day 2: Cross window Communication
Same Origin Policy:
Two URLs share the "same origin" if they have the same protocol, domain, and port.
Subdomains can be treated as the "same origin" by setting document.domain for cross-window communication.
Iframe Document Access Pitfalls:
When creating an iframe, it immediately has a document associated with it, but it's not the actual loaded document.
The correct document becomes available only after the iframe has loaded its content.
Use the onload event of the iframe to ensure the correct document is ready for manipulation.
If you need earlier access, use periodic checks with setInterval, but it may introduce a delay.
Iframe Window Object Access:
Access the window object of an iframe using window.frames by number or name.
window.frames includes a collection of "children" windows for nested frames.
window.parent provides a reference to the parent window.
window.top refers to the topmost parent window.
Sandbox Attribute:
The sandbox attribute in iframes allows for restricting certain actions to prevent the execution of untrusted code.
It treats the iframe as if it's from another origin or applies other limitations.
You can specify a list of limitations to relax the default restrictions, such as allowing forms, scripts, or popups.
Setting allow-same-origin removes the "different origin" policy for the iframe.
Cross Window Messaging with postMessage:
The postMessage interface enables communication between windows, regardless of origin, bypassing the "Same Origin" policy.
It consists of two parts: calling the postMessage method and specifying the target window's origin.
The data argument can be any JavaScript object and is cloned using the structured cloning algorithm.
The targetOrigin argument ensures data is received only by windows from the specified origin, enhancing security for sensitive data exchange.
Use postMessage for secure cross-window communication in web development.

Day 3: Clickjacking
Today I learnt about clickjacking:
Clickjacking is a way to “trick” users into clicking on a victim site without even knowing what’s happening. That’s dangerous if there are important click-activated actions.
A hacker can post a link to their evil page in a message, or lure visitors to their page by some other means. There are many variations.
From one perspective – the attack is “not deep”: all a hacker is doing is intercepting a single click. But from another perspective, if the hacker knows that after the click another control will appear, then they may use cunning messages to coerce the user into clicking on them as well.
The attack is quite dangerous, because when we engineer the UI we usually don’t anticipate that a hacker may click on behalf of the visitor. So vulnerabilities can be found in totally unexpected places.
It is recommended to use X-Frame-Options: SAMEORIGIN on pages (or whole websites) which are not intended to be viewed inside frames.
Use a covering <div> if we want to allow our pages to be shown in iframes, but still stay safe. To insert a few words of code, use the <code>tag, for several lines – use <pre>, for more than 10 lines – use a sandbox.

Day 4: ArrayBuffer and Binary Arrays

This day I learnt the following:
ArrayBuffer is the core object, a reference to the fixed-length contiguous memory area.
To do almost any operation on ArrayBuffer, we need a view.
It can be a TypedArray:
Uint8Array, Uint16Array, Uint32Array – for unsigned integers of 8, 16, and 32 bits.
Uint8ClampedArray – for 8-bit integers, “clamps” them on assignment.
Int8Array, Int16Array, Int32Array – for signed integer numbers (can be negative).
Float32Array, Float64Array – for signed floating-point numbers of 32 and 64 bits.
Or a DataView – the view that uses methods to specify a format, e.g. getUint8(offset).
In most cases we create and operate directly on typed arrays, leaving ArrayBuffer undercover, as a “common discriminator”. We can access it as .bufferand make another view if needed.
There are also two additional terms, that are used in descriptions of methods that operate on binary data:
ArrayBufferView is an umbrella term for all these kinds of views.
BufferSource is an umbrella term for ArrayBuffer or ArrayBufferView.

 Day 5
 This is the last day of the week! We had an assessment on week1 of intermediate JavaScript for the following topics: Frames and windows, cross window communication, the clickjacking attack, arraybuffer and binary array.
End of Week 1 Post section.

    WEEK 2: FILES PATTENS AND FLAGS
    Day1
     File and FileReader
 File constructor
new File(fileParts, fileName, [options])
fileParts - is an array of Blob/BufferSource/String values.
fileName - file name string.
options - optional object:
lastModified - the timestamp (integer date) of last modification.
   FileReader
FileReader is an object with the sole purpose of reading data from Blob (and hence File too) objects.
The constructor is as follows:
let reader = new FileReader(); // no arguments
The main methods:
readAsArrayBuffer(blob) – read the data in binary format ArrayBuffer.
readAsText(blob, [encoding]) – read the data as a text string with the given encoding (utf-8by default).
readAsDataURL(blob) – read the binary data and encode it as base64 data url.
abort() – cancel the operation.

Events:
loadstart – loading started.
progress – occurs during reading.
load – no errors, reading complete.
abort – abort() called.
error – error has occurred.
loadend – reading finished with either success or failure.
When the reading is finished, we can access the result as:
reader.result is the result (if successful)
reader.error is the error (if failed).

 FileReader for blobs

As mentioned in the chapter Blob, FileReadercan read not just files, but any blobs.
We can use it to convert a blob to another format:
readAsArrayBuffer(blob) – to ArrayBuffer,
readAsText(blob, [encoding]) – to string (an alternative to TextDecoder),
readAsDataURL(blob) – to base64 data url.
FileReaderSyn
Is available inside Web Workers
Its reading methods read* do not generate events
but rather return a result, as regular functions do.
     Code examples
// Create a Blob
const text = "Hello, this is a blob!";
const blob = new Blob([text], { type: "text/plain" });

// Create a FileReader
const fileReader = new FileReader();

// Read the Blob as ArrayBuffer
fileReader.onload = function(event) {
  const arrayBuffer = event.target.result;
  console.log("ArrayBuffer:", arrayBuffer);
};
fileReader.readAsArrayBuffer(blob);

// Read the Blob as text (UTF-8 encoding)
fileReader.onload = function(event) {
  const text = event.target.result;
  console.log("Text:", text);
};
fileReader.readAsText(blob);

// Read the Blob as a Data URL
fileReader.onload = function(event) {
  const dataURL = event.target.result;
  console.log("Data URL:", dataURL);
};
fileReader.readAsDataURL(blob);

       Fetch
 Fetch is the method to send network requests to the server and load new information whenever is needed.
The basic syntax is:
let promise = fetch(url, [options])
url – the URL to access.
options – optional parameters: method, headers etc.

 Post Requests
 To make a POST request, or a request with another method, we need to use fetch options:
method – HTTP-method, e.g. POST,
body – one of:
a string (e.g. JSON),
FormData object, to submit the data as form/multipart,
Blob/BufferSource to send binary data,
URLSearchParams, to submit the data in x-www-form-urlencoded encoding, rarely used.
For example, this code submits.

 Response
esponse provides multiple promise-based methods to access the body in various formats:
response.json() – parse the response as JSON object,
response.text() – return the response as text,
response.formData() – return the response as FormData object (form/multipart encoding, explained in the next chapter),
response.blob() – return the response as Blob(binary data with type),
response.arrayBuffer() – return the response as ArrayBuffer (pure binary data).

 Response properties:
response.status – HTTP code of the response,
response.ok – true is the status is 200-299.
response.headers – Map-like object with HTTP headers.
Methods to get response body:
response.json() – parse the response as JSON object,
response.text() – return the response as text,
response.formData() – return the response as FormData object (form/multipart encoding, see the next chapter),
response.blob() – return the response as Blob(binary data with type),
response.arrayBuffer() – return the response as ArrayBuffer (pure binary data),

Fetch options so far:
method – HTTP-method,
headers – an object with request headers (not any header is allowed),
body – string, FormData, BufferSource, Blob or UrlSearchParams object to send.

fetch users from Github
 async function getUsers(names) {
const userPromises = names.map(async (name) => {
const url = `https://api.github.com/users/${name}`;
try {
const response = await fetch(url);
if (response.ok) {
const user = await response.json();
return user;
} else {
return null; // Return null if the request fails
}
} catch (error) {
return null; // Return null if there's an error
}
});

const users = await Promise.all(userPromises);
return users;
}

// Example usage:
const githubUsernames = ["user1", "user2", "user3"];
getUsers(githubUsernames)
.then((result) => {
console.log(result); // Array of GitHub users or null values
})
.catch((error) => {
console.error(error);
});
In this code:
We create an array of Promise objects (userPromises) by mapping over the names array and making a fetch request for each GitHub username.
Inside the try block, we check if the response is successful using response.ok. If it is, we parse the JSON response and return the user object. If the request fails, we return null.
If there's an error during the fetch request (e.g., a network error), we also return null.
We then use Promise.all to wait for all the requests to complete. This ensures that the requests run concurrently and don't wait for each other.
Finally, we return the array of GitHub users (including null values for failed requests) as the result.
Remember to replace the example githubUsernames array with your desired GitHub logins when calling the getUsers function.

Day 2
 FormData methods are used to modify fields in a form.
FormData methods:
formData.append(name, value) – add a form field with the given name and value,
formData.append(name, blob, fileName) – add a field as if it were <input type="file">, the third argument fileName sets file name (not form field name), as it it were a name of the file in user’s filesystem,
formData.delete(name) – remove the field with the given name,
formData.get(name) – get the value of the field with the given name,
formData.has(name) – if there exists a field with the given name, returns true, otherwise false
A form is technically allowed to have many fields with the same name, so multiple calls to append add more same-named fields.

There’s also method set, with the same syntax as append. The difference is that .set removes all fields with the given name, and then appends a new field. So it makes sure there’s only field with such name:
formData.set(name, value),
formData.set(name, blob, fileName).

Fetch

The fetch method allows to track download progress.
To track download progress, we can use response.body property. It’s a ReadableStream – a special object that provides body chunk-by-chunk, as it comes. Readable streams are described in the Streams API specification.
Unlike response.text(), response.json() and other methods, response.body gives full control over the reading process, and we can count how much is consumed at any moment.

Example
// instead of response.json() and other methods
const reader = response.body.getReader();

// infinite loop while the body is downloading
while(true) {
// done is true for the last chunk
// value is Uint8Array of the chunk bytes
const {done, value} = await reader.read();

if (done) {
break;
}

console.log(`Received ${value.length} bytes`)
}
The result of await reader.read() call is an object with two properties:
done – true when the reading is complete, otherwise false.
value – a typed array of bytes: Uint8Array.
Sending a form with Blob data
.servers are usually more suited to accept multipart-encoded forms, rather than raw binary data.

Fetch: Abort
 AbortController is a simple object that generates an abort event on its signal property when the abort() method is called (and also sets signal.aborted to true).
fetch integrates with it: we pass the signal property as the option, and then fetch listens to it, so it’s possible to abort the fetch.
We can use AbortController in our code. The "call abort()"

Day 3 Fetch: Cross Origin Request

Cross-Origin Requests (CORS):

CORS is a security feature implemented by web browsers to control and restrict web page requests made to different domains or origins.
Same-Origin Policy (SOP) is the default behavior that restricts web pages from making requests to domains different from the one serving the web page.
CORS is essential for security, preventing malicious websites from making unauthorized requests and allowing legitimate websites to request and share resources with different origins securely.
Simple Requests vs. Non-Simple Requests:

Cross-origin requests can be categorized into two types: "Simple Requests" and "Non-Simple Requests."
Simple requests meet specific conditions, including using common HTTP methods (GET, POST, or HEAD) and having limited allowable headers.
Non-simple requests include non-standard methods (e.g., PUT, DELETE) or custom headers like API-Key, which do not fit the limitations of simple requests.
Non-simple requests trigger a preflight request (OPTIONS method) to seek permission from the server before the actual request is sent.
CORS for Simple Requests:

When a request is cross-origin, the browser automatically includes an Origin header in the request, specifying the origin of the requesting page.
The server receiving the request can inspect the Origin header and respond with the Access-Control-Allow-Origin header, specifying the allowed origin.
The browser acts as a mediator, ensuring the presence of the correct headers for secure cross-origin requests.
Response Headers:

JavaScript is, by default, only allowed to access a limited set of response headers known as "simple response headers."
Simple response headers include Cache-Control, Content-Language, Content-Type, Expires, Last-Modified, and Pragma.
The Content-Length header is not included in simple response headers.
To grant JavaScript access to other response headers, the server must list them in the Access-Control-Expose-Headers response header.
CORS for Non-Simple Requests:

Non-simple requests, which include non-standard methods and custom headers, trigger a preflight request (OPTIONS method) to the server.
The preflight request includes headers like Access-Control-Request-Method and Access-Control-Request-Headers.
The server should respond with headers like Access-Control-Allow-Methods, Access-Control-Allow-Headers, and Access-Control-Max-Age if it grants permission.
The browser ensures that these headers are present and match the requesting origin before allowing the actual request.
Credentials and Origin:

Cross-origin requests made by JavaScript do not send credentials (cookies or HTTP authentication) by default, enhancing security.
Servers must explicitly allow requests with credentials by adding the header Access-Control-Allow-Credentials: true.
To send credentials, the fetch request should include credentials: "include," and the response must specify the exact trusted origin.
The Origin header is crucial for cross-origin requests, as it reliably identifies the origin, unlike the Referer header.
Fetch API:

The Fetch API offers various options for customizing HTTP requests, including referrer, referrerPolicy, mode, credentials, cache, redirect, integrity, and keepalive.
These options provide fine-grained control over various aspects of HTTP requests and responses, enhancing security and customization.

Day 4: Patterns and Flags

 Patterns and Flags
A regular expression (also “regexp”, or just “reg”) consists of a pattern and optional flags.
There are two syntaxes to create a regular expression object.

The long syntax:
regexp = new RegExp("pattern", "flags");

…And the short one, using slashes "/":
regexp = /pattern/; // no flags
regexp = /pattern/gmi; // with flags g,m and i (to be covered soon).

Patterns and Flags
 A regular expression (also “regexp”, or just “reg”) consists of a pattern and optional flags.
There are two syntaxes to create a regular expression object.
The long syntax:
regexp = new RegExp("pattern", "flags");
…And the short one, using slashes "/":
regexp = /pattern/; // no flags
regexp = /pattern/gmi; // with flags g,m and i (to be covered soon). 

    Usage
 To search for a pattern within a string, the search method can be used.
The search method takes a regular expression as an argument and returns the position of the first occurrence of the pattern in the string.
In the example, str.search(regexp) searches for the /love/ pattern in the string str and returns the position 2.
Simple substring searches can also be performed without regular expressions, like searching for the substring "love" directly.
Using new RegExp() allows for dynamic construction of regular expressions from strings, making it more flexible when patterns need to be generated dynamically.

 Flags: 
i: Case-insensitive search. It makes the search treat uppercase and lowercase characters as equivalent. For example, /LOVE/i would match both "love" and "LOVE."
g: Global search. This flag searches for all matches in the input string. Without it, only the first match is found.
m: Multiline mode. This flag changes how the ^ and $ anchors work, allowing them to match the start and end of individual lines in a multiline string.
s: Dotall mode. This flag allows the . metacharacter to match newline characters (\n), making it useful for matching across multiple lines.
u: Unicode mode. This flag enables full Unicode support, including the correct processing of surrogate pairs. It is essential for working with non-ASCII characters.
y: Sticky mode. This flag affects the behavior of the exec and test methods, making them start searching from the last matched position in the string.

example: i flag

let str = "I love JavaScript!";
alert( str.search(/LOVE/i) ); // 2 (found lowercased)
alert( str.search(/LOVE/) ); // -1 (nothing found without 'i' flag)

   Methods of RegExp and String
 str.match(regexp) finds all matches of regexp in the string str.
With the g flag in regexp, it returns an array of all matches.
Without the g flag, it returns an array with the first match at index 0.
If there are no matches, it returns null.
str.replace(regexp, replacement) replaces matches found using regexp in the string str with the replacement string.
Without the g flag, it replaces only the first match.
With the g flag, it replaces all matches.
Special character combinations in the replacement string allow you to insert fragments of the match.
regexp.test(str) checks if at least one match exists in the string str and returns true if found, otherwise false.

    Character classes
 Character classes in regular expressions are a special notation to match any symbol from a certain set.
\d matches any single digit (0-9).
\s matches any space symbol, including spaces, tabs, and newlines.
\w matches a "wordly" character, which includes English alphabet letters, digits, and underscores. It does not include non-Latin letters.
Regular expressions can contain a combination of regular symbols and character classes.
Character classes are enclosed in backslashes, e.g., \d for digits.
Using the g flag allows regular expressions to find all matches in a string.
Character classes can be used in combination with other regular expression patterns to match specific patterns within a text.

      Word boundary \b
 \b is a special character class in regular expressions that represents a word boundary.
A word boundary is not a character itself but rather a boundary between characters.
\bJava\b would match "Java" in the string "Hello, Java!" but not in "Hello, JavaScript!" because it checks for word boundaries.
Word boundaries are tested in three scenarios:
Before a word character \w and immediately after a non-word character, or vice versa.
At the start of the string if the first character is a word character.
At the end of the string if the last character is a word character.
\b is often used to find standalone words in a text, ignoring instances where the word is part of a larger word (e.g., matching "Java" but not "JavaScript").
Word boundaries are based on the \w character class, which represents English letters, digits, and underscores. They may not work correctly for non-Latin alphabets.
Unicode character classes can be used to handle word boundaries in different languages.

      Inverse Classes
 character classes in regular expressions are as \D, \S, \W, and \B, match characters that are not in the specified character class.

Example:
let str = "+7(903)-123-45-67";
alert( str.replace(/\D/g, "") ); // 79031234567

     Spaces are regular characters
 space is a character. Equal in importance with any other character.
Extra spaces (just like any other extra characters) may prevent a match:
alert( "1-5".match(/\d - \d/) ); // null, because the string 1-5 has no spaces

          example:
Here we fix it by adding spaces into the regexp \d - \d:
alert( "1 - 5".match(/\d - \d/) ); // 1 - 5, now it works
Daily Notes - A dot is any character
 The dot "." is a special character class that matches “any character except a newline”. It means “any character”, but not the “absence of a character”.

Example:
1. alert( "CS-4".match(reg) ); // CS-4
2. alert( "CS 4".match(reg) ); // CS 4 (space is also a character).
3. alert( "CS4".match(/CS.4/) ); // null, no match because there's no character for the dot.
Daily Notes - The dotall “s” flag
 s flag in regular expressions affects the behavior of the dot . character, making it match any character, including newline characters. Additionally, you've mentioned several commonly used character classes like \d, \D, \s, \S, \w, and \W, along with their inverse classes.

Unicode properties in regex patterns, such as matching characters based on their script or category. For example, \p{Script=Cyrillic} would match Cyrillic letters.
Daily Notes - Activity 1 - Find the Time
 let time = "09:00";
let regexp = /^\d\d:\d\d/;
console.log(time.match(regexp));

Code Explanation: 
The time variable should be assigned a string value, so I enclosed it in double quotes: "09:00".
The regular expression should be defined correctly using the RegExp constructor. The pattern should be enclosed in / characters, and you should use backslashes (\) to escape special characters.
The regular expression pattern /^\d\d:\d\d/ matches the "hours:minutes" format correctly. The ^ indicates the start of the string, \d matches digits, and : is matched literally.

Escaping, Special character
 To match a literal dot (.), you need to escape it with a backslash (\.).
 
Example: 
/\d\.\d/ matches the pattern "digit dot digit," like "5.1," but not "511."
To match literal parentheses (( or )), you should escape them with a backslash.

Example:
/g\(\)/ matches the pattern "g()," as in "function g()".
To match a literal backslash (\), you should double it to escape it properly within a string.

Example: 
/1\\2/ matches the pattern "1\2."
To match a literal slash (/) when using /.../ notation for regular expressions, you should escape it with a backslash.

Example: 
/\// matches the pattern "/" in the string.
When creating regular expressions with the new RegExp constructor, you also need to escape backslashes properly in the string you provide. Use double backslashes to ensure they are interpreted correctly within the regular expression.

Example: let regStr = "\\d\\.\\d"; creates a string with the pattern "\d.\d," and then new RegExp(regStr) creates a regular expression with that pattern.
example:
let regStr = "\\d\\.\\d"; // Escaped backslashes
let reg = new RegExp(regStr);
console.log("Chapter 5.1".match(reg)); // Matches "5.1"

END OF DAY 4


 Day 5: Presentation Day 
 Summary notes for week two  
 
 Day 1: File and FileReader

Introduced the File constructor for creating file objects.
Explained the FileReader object and its constructor.
Detailed the main methods of FileReader for reading data from Blob and File objects.
Covered the events associated with FileReader.
Mentioned the availability of FileReader in Web Workers and its reading methods.
Provided code examples for working with File and FileReader.
Introduced the fetch method for making network requests and sending data.
Explained POST requests with fetch and the use of the method and body options.
Discussed response handling with the Response object, including methods for parsing various data formats.
Summarized the fetch options for method, headers, and body.
Provided an example of using fetch to retrieve user data from GitHub.

Day 2: FormData
Covered various methods of the FormData object for modifying form fields.
Explained the usage of FormData.append(), FormData.delete(), FormData.get(), and FormData.has().
Mentioned the set method of FormData for setting fields.
Explained how to send form data with blob data.
Introduced the concept of an AbortController for aborting fetch requests.

Day 3: Fetch - Cross-Origin Request
Discussed Cross-Origin Requests (CORS) and Same-Origin Policy (SOP).
Differentiated between Simple and Non-Simple Requests in CORS.
Explained how CORS works, including the role of the Origin header.
Covered response headers and the handling of credentials in CORS.
Introduced the Fetch API and its options for customizing HTTP requests.

Day 4: Regular Expressions
Explained the syntax for creating regular expressions with the RegExp constructor and slashes.
Detailed the usage of regular expressions for searching, matching, and replacing patterns in strings.
Covered commonly used flags like i, g, m, s, and u and their functions.
Introduced the y flag for sticky mode.
Discussed character classes like \d, \s, \w, and their inverse classes.
Explained word boundaries with \b.
Mentioned the use of the dot . to match any character.
Introduced the s flag for dotall mode.
Covered escaping special characters like ., (, ), and backslashes.
Provided an example of using regular expressions to match specific patterns in strings.
Mentioned Unicode properties in regex patterns.

------ END OF WEEK 2

WEEK 3: NODE JS
DAY 1 : INTRODUCTION TO NODE JS

Introduction to Node Js
Introduction to Node.js: Node.js is an asynchronous event-driven JavaScript runtime designed for building scalable network applications. It is particularly well-suited for handling multiple connections concurrently.

Concurrency Model: Node.js uses a non-blocking, event-driven concurrency model, which is different from the thread-based model used by many other systems. This design eliminates concerns about deadlocks and simplifies the development of scalable systems.

Event Loop: Node.js features an event loop as a runtime construct. It automatically enters the event loop after executing the input script and exits when there are no more callbacks to perform. This event loop is a core part of Node.js's asynchronous and non-blocking behavior.

HTTP in Node.js: Node.js treats HTTP as a first-class citizen, making it suitable for building web libraries or frameworks with streaming and low-latency capabilities in mind. It is often used for creating server-side applications and APIs.

Utilizing Multiple Cores: Although Node.js does not use traditional threads, it supports leveraging the power of multiple CPU cores through features such as child processes and the cluster module for load balancing. This allows Node.js applications to take advantage of modern, multi-core CPUs.

ECMAScript 2015 and Beyond: Node.js is built on top of modern versions of the V8 JavaScript engine, ensuring timely support for new features and improvements from the ECMAScript specification. This provides both better performance and stability.

ECMAScript Feature Groups: ECMAScript 2015 (ES6) features are categorized into different groups, including "shipping," "staged," and "in-progress." Activation of these features in Node.js may vary and is typically determined by the language specification and the Node.js team.

Node.js Feature Availability: The notes mention a website called "node.green," which serves as a resource for information on supported ECMAScript features in various Node.js versions. It helps developers understand which JavaScript features are available in their Node.js environment.
How to Install Node.js and NPM on Windows
Downloading Node.js and NPM:
1.	Go to the official Node.js website at https://nodejs.org/en/download/.
2.	On the download page, choose the LTS (Long Term Support) version for stability.
3.	Download the installer for your system architecture (32-bit or 64-bit).
Running the Installer:
4.	Locate the downloaded .msi installer file (e.g., node-vxx.x.x-x86.msi for 32-bit).
5.	Double-click the installer file to run it.
Node.js Installation Wizard:
6.	Click the "Run" button in the first screen of the Installation Wizard.
License Agreement:
7.	Read and accept the Node.js license agreement by checking the "I accept the terms in the License Agreement" checkbox.
Destination Folder:
8.	Choose the installation location for Node.js (the default location is usually suitable).
Select Components:
9.	Keep the default options selected, which include Node.js and npm (Node Package Manager).
Start Menu Folder:
10.	Choose a Start Menu folder for creating shortcuts to Node.js (the default folder is sufficient).
Custom Setup (Optional):
11.	If needed, you can click the "Custom" button to configure advanced settings. However, the default settings are suitable for most users.
Ready to Install:
12.	Review the installation settings and click the "Install" button to begin the installation.
Installation Progress:
13.	The installer will copy the necessary files and install Node.js and npm on your system, displaying a progress bar.
Installation Completed:
14.	Once the installation is complete, you'll see a "Completed" message, indicating that Node.js and npm are installed on your Windows system.
Verification:
15.	Open the Command Prompt or PowerShell.
16.	Use the following commands to verify the installed versions:
•	node -v (to check the Node.js version)
•	npm -v (to check the npm version)
17.	You should see the versions of Node.js and npm displayed in the terminal, confirming the successful installation.
By following these steps, you can install Node.js and npm on your Windows system, allowing you to develop and run JavaScript applications and manage packages effectively.
You should see the versions of Node.js and npm displayed in the terminal, confirming that the installation was successful.

--- END OF DAY 1

  DAY 2: NODE JS LIBRARY 
  
Fun and Games with Node.js

Node.js is a versatile platform for creating various games and interactive applications. Here are some key points:

Text-Based Games: Node.js is suitable for creating text-based games like quizzes or adventures. It can handle user input/output using libraries like readline.

Multiplayer Games: Real-time multiplayer games can be developed using libraries like Socket.io. These games can include card games, board games, or shooters.

Web-Based Games: Node.js can be used to create web-based games by combining HTML5 canvas and JavaScript. Libraries like Phaser can be integrated for game development.

Trivia Quiz Games: Node.js can handle real-time aspects of online trivia quiz games, such as tracking scores and notifying players.

Chat Games: Chat-based games, where players interact with bots or each other to solve puzzles or riddles, can be built using Node.js with frameworks like Botkit.

Board Games: Classic board games like chess or tic-tac-toe can be developed with multiplayer features, and Node.js can manage game state and player interactions.

Text-Based RPGs: Text-based role-playing games allow players to create characters, go on quests, and interact with an in-game world. Node.js handles game logic and database interactions.

Simulation Games: Simulation games, such as city-building or farming simulators, can be created using Node.js for backend simulation and user interactions.

Card Games: Node.js can manage the game rules and communication between players for card games like poker or blackjack.

Augmented Reality (AR) Games: Combining Node.js with AR libraries like AR.js enables the development of interactive AR games that can be played in web browsers.

AI-Driven Games: Node.js can manage AI logic and facilitate player interactions in games where AI plays against human players.

Mini-Games for Websites: Add simple mini-games like quizzes or puzzles to websites using Node.js to engage visitors.

Node.js is an asynchronous, event-driven JavaScript runtime primarily designed for building scalable network applications. Here are the key points:

Concurrency Model: Node.js uses a non-blocking, event-driven concurrency model, which is different from the traditional thread-based model. This design simplifies the development of scalable systems and eliminates concerns about deadlocks.

Event Loop: Node.js features an event loop as a core runtime construct. It automatically enters the event loop after executing the input script and exits when there are no more callbacks to perform. This event loop enables Node.js's asynchronous and non-blocking behavior.

HTTP in Node.js: Node.js treats HTTP as a first-class citizen, making it suitable for building web libraries or frameworks that prioritize streaming and low latency.

Utilizing Multiple Cores: Although Node.js doesn't use threads in the traditional sense, it supports leveraging multiple CPU cores through features like child processes and the cluster module, enabling load balancing and utilizing modern, multi-core CPUs effectively.

ECMAScript 2015 and Beyond: Node.js is built on modern versions of the V8 JavaScript engine, ensuring timely support for new features from the ECMAScript specification. This provides better performance and stability.

ECMAScript Feature Groups: ECMAScript 2015 (ES6) features are categorized into shipping, staged, and in-progress groups, each requiring different levels of activation. The availability of these features in Node.js depends on the language specification and the Node.js team's decisions.

Node.js Feature Availability: The notes mention a website, node.green, which serves as a resource for information on supported ECMAScript features in various Node.js versions. This resource helps developers understand which JavaScript features are available in their Node.js environment
--- END OF DAY 2

 DAY 3: CLIENT SIDE GAME DEVELOPMENT 
Concurrency Model: Node.js uses a non-blocking, event-driven concurrency model, which is different from the traditional thread-based model. This design simplifies the development of scalable systems and eliminates concerns about deadlocks.

Event Loop: Node.js features an event loop as a core runtime construct. It automatically enters the event loop after executing the input script and exits when there are no more callbacks to perform. This event loop enables Node.js's asynchronous and non-blocking behavior.

HTTP in Node.js: Node.js treats HTTP as a first-class citizen, making it suitable for building web libraries or frameworks that prioritize streaming and low latency.

Utilizing Multiple Cores: Although Node.js doesn't use threads in the traditional sense, it supports leveraging multiple CPU cores through features like child processes and the cluster module, enabling load balancing and utilizing modern, multi-core CPUs effectively.

ECMAScript 2015 and Beyond: Node.js is built on modern versions of the V8 JavaScript engine, ensuring timely support for new features from the ECMAScript specification. This provides better performance and stability.

ECMAScript Feature Groups: ECMAScript 2015 (ES6) features are categorized into shipping, staged, and in-progress groups, each requiring different levels of activation. The availability of these features in Node.js depends on the language specification and the Node.js team's decisions.

Node.js Feature Availability: The notes mention a website, node.green, which serves as a resource for information on supported ECMAScript features in various Node.js versions. This resource helps developers understand which JavaScript features are available in their Node.js environment.

---END OF DAY 3
 
