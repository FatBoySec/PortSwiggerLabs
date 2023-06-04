### Reflected XSS into attribute with angle brackets HTML-encoded

1 - Submit a random alphanumeric string in the search box, then use Burp Suite to intercept the search request and send it to Burp Repeater.

2 - Observe that the random string has been reflected inside a quoted attribute.

3 - Replace your input with the following payload to escape the quoted attribute and inject an event handler:

```"onmouseover="alert(1)```
    
4 - Verify the technique worked by right-clicking, selecting "Copy URL", and pasting the URL in the browser. When you move the mouse over the injected element it should trigger an alert.

### XSS in HTML tag attributes

When the XSS context is into an HTML tag attribute value, you might sometimes be able to terminate the attribute value, close the tag, and introduce a new one. For example:

```"><script>alert(document.domain)</script>```

More commonly in this situation, angle brackets are blocked or encoded, so your input cannot break out of the tag in which it appears. Provided you can terminate the attribute value, you can normally introduce a new attribute that creates a scriptable context, such as an event handler. For example:

```" autofocus onfocus=alert(document.domain) x="```

The above payload creates an ```onfocus``` event that will execute JavaScript when the element receives the focus, and also adds the ```autofocus``` attribute to try to trigger the ```onfocus``` event automatically without any user interaction. Finally, it adds ```x="``` to gracefully repair the following markup. 

### Reflected XSS into attribute with angle brackets HTML-encoded

Sometimes the XSS context is into a type of HTML tag attribute that itself can create a scriptable context. Here, you can execute JavaScript without needing to terminate the attribute value. For example, if the XSS context is into the href attribute of an anchor tag, you can use the javascript pseudo-protocol to execute script. For example:

```<a href="javascript:alert(document.domain)">```
