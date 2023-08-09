## Sources and sinks in third-party dependencies

### DOM XSS in jQuery

If a JavaScript library such as jQuery is being used, look out for sinks that can alter DOM elements on the page. For instance, jQuery's ```attr()``` function can change the attributes of DOM elements. If data is read from a user-controlled source like the URL, then passed to the ```attr()``` function, then it may be possible to manipulate the value sent to cause XSS. For example, here we have some JavaScript that changes an anchor element's ```href``` attribute using data from the URL:

```js
$(function() {
	$('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl'));
});
```


You can exploit this by modifying the URL so that the ```location.search``` source contains a malicious JavaScript URL. After the page's JavaScript applies this malicious URL to the back link's ```href```, clicking on the back link will execute it:

```?returnUrl=javascript:alert(document.domain)```


Lab:

1 - On the Submit feedback page, change the query parameter ```returnPath``` to ```/``` followed by a random alphanumeric string.

2 - Right-click and inspect the element, and observe that your random string has been placed inside an a ```href``` attribute.

3 - Change ```returnPath``` to:

```javascript:alert(document.cookie)```

Hit enter and click "back".

