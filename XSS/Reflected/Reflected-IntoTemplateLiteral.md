## XSS in JavaScript template literals

JavaScript template literals are string literals that allow embedded JavaScript expressions. The embedded expressions are evaluated and are normally concatenated into the surrounding text. Template literals are encapsulated in backticks instead of normal quotation marks, and embedded expressions are identified using the ```${...}``` syntax.

For example, the following script will print a welcome message that includes the user's display name:

```document.getElementById('message').innerText = `Welcome, ${user.displayName}.`;```

When the XSS context is into a JavaScript template literal, there is no need to terminate the literal. Instead, you simply need to use the ${...} syntax to embed a JavaScript expression that will be executed when the literal is processed. For example, if the XSS context is as follows:
```
<script>
...
var input = `controllable data here`;
...
</script>
```

then you can use the following payload to execute JavaScript without terminating the template literal:

```${alert(document.domain)}```


### Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

1 - Submit a random alphanumeric string in the search box, then use Burp Suite to intercept the search request and send it to Burp Repeater.

2 - Observe that the random string has been reflected inside a JavaScript template string.

3 - Replace your input with the following payload to execute JavaScript inside the template string: ```${alert(1)}```

4 - Verify the technique worked by right clicking, selecting "Copy URL", and pasting the URL in the browser. When you load the page it should trigger an alert.

