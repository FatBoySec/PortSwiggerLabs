## Making use of HTML-encoding

When the XSS context is some existing JavaScript within a quoted tag attribute, such as an event handler, it is possible to make use of HTML-encoding to work around some input filters.

When the browser has parsed out the HTML tags and attributes within a response, it will perform HTML-decoding of tag attribute values before they are processed any further. If the server-side application blocks or sanitizes certain characters that are needed for a successful XSS exploit, you can often bypass the input validation by HTML-encoding those characters.

For example, if the XSS context is as follows:

```<a href="#" onclick="... var input='controllable data here'; ...">```

and the application blocks or escapes single quote characters, you can use the following payload to break out of the JavaScript string and execute your own script:

```&apos;-alert(document.domain)-&apos;```

The ```&apos;``` sequence is an HTML entity representing an apostrophe or single quote. Because the browser HTML-decodes the value of the onclick attribute before the JavaScript is interpreted, the entities are decoded as quotes, which become string delimiters, and so the attack succeeds. 


### Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

1 - Post a comment with a random alphanumeric string in the "Website" input, then use Burp Suite to intercept the request and send it to Burp Repeater.
    
2 - Make a second request in the browser to view the post and use Burp Suite to intercept the request and send it to Burp Repeater.

3 - Observe that the random string in the second Repeater tab has been reflected inside an onclick event handler attribute.

4 - Repeat the process again but this time modify your input to inject a JavaScript URL that calls alert, using the following payload:

```http://foo?&apos;-alert(1)-&apos;```
    
5 - Verify the technique worked by right-clicking, selecting "Copy URL", and pasting the URL in the browser. Clicking the name above your comment should trigger an alert.
(maybe no needed)

