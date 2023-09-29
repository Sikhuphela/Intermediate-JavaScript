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
Popup Allowed:
If pop-ups are allowed by the browser settings or in the current context (e.g., if the code is executed in response to a user action like clicking a button), the window.open() method will open a new window/tab with the URL 'https://javascript.info'.
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
The current post section is Week 1.
