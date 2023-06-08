## XSS into JavaScript

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


## Breaking out of a JavaScript string

In cases where the XSS context is inside a quoted string literal, it is often possible to break out of the string and execute JavaScript directly. It is essential to repair the script following the XSS context, because any syntax errors there will prevent the whole script from executing.

Some useful ways of breaking out of a string literal are: 

```
'-a`lert(document.domain)-'
';alert(document.domain)//
```

### Reflected XSS into a JavaScript string with angle brackets HTML encoded

1 - Submit a random alphanumeric string in the search box, then use Burp Suite to intercept the search request and send it to Burp Repeater.

2 - Observe that the random string has been reflected inside a JavaScript string.

3 - Replace your input with the following payload to break out of the JavaScript string and inject an alert:

```'-alert(1)-'```
    
4 - Verify the technique worked by right clicking, selecting "Copy URL", and pasting the URL in the browser. When you load the page it should trigger an alert.

### Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

Some applications attempt to prevent input from breaking out of the JavaScript string by escaping any single quote characters with a backslash. A backslash before a character tells the JavaScript parser that the character should be interpreted literally, and not as a special character such as a string terminator. In this situation, applications often make the mistake of failing to escape the backslash character itself. This means that an attacker can use their own backslash character to neutralize the backslash that is added by the application.

For example, suppose that the input:

```';alert(document.domain)//```

gets converted to:

```\';alert(document.domain)//```

You can now use the alternative payload:

```\';alert(document.domain)//```

which gets converted to:

```\\';alert(document.domain)//```

Here, the first backslash means that the second backslash is interpreted literally, and not as a special character. This means that the quote is now interpreted as a string terminator, and so the attack succeeds. 


1 - Submit a random alphanumeric string in the search box, then use Burp Suite to intercept the search request and send it to Burp Repeater.

2 - Observe that the random string has been reflected inside a JavaScript string.
    
3 - Try sending the payload test'payload and observe that your single quote gets backslash-escaped, preventing you from breaking out of the string.

4 - Try sending the payload test\payload and observe that your backslash doesn't get escaped.

5 - Replace your input with the following payload to break out of the JavaScript string and inject an alert:

```\'-alert(1)//```

6 - Verify the technique worked by right clicking, selecting "Copy URL", and pasting the URL in the browser. When you load the page it should trigger an alert.


