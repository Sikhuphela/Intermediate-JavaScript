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
