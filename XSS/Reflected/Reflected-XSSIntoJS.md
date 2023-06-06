### XSS into JavaScript

When the XSS context is some existing JavaScript within the response, a wide variety of situations can arise, with different techniques necessary to perform a successful exploit.
Terminating the existing script

In the simplest case, it is possible to simply close the script tag that is enclosing the existing JavaScript, and introduce some new HTML tags that will trigger execution of JavaScript. For example, if the XSS context is as follows:

```
<script>
...
var input = 'controllable data here';
...
</script>
```

then you can use the following payload to break out of the existing JavaScript and execute your own:

```</script><img src=1 onerror=alert(document.domain)>```

The reason this works is that the browser first performs HTML parsing to identify the page elements including blocks of script, and only later performs JavaScript parsing to understand and execute the embedded scripts. The above payload leaves the original script broken, with an unterminated string literal. But that doesn't prevent the subsequent script being parsed and executed in the normal way. 


### Reflected XSS into a JavaScript string with single quote and backslash escaped

1 - Submit a random alphanumeric string in the search box, then use Burp Suite to intercept the search request and send it to Burp Repeater.

2 - Observe that the random string has been reflected inside a JavaScript string.
    
3 - Try sending the payload test'payload and observe that your single quote gets backslash-escaped, preventing you from breaking out of the string.

4 - Replace your input with the following payload to break out of the script block and inject a new script:

```</script><script>alert(1)</script>```
   
5 - Verify the technique worked by right clicking, selecting "Copy URL", and pasting the URL in the browser. When you load the page it should trigger an alert.

