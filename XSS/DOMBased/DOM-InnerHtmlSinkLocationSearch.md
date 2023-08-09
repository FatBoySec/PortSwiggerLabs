## DOM XSS in innerHTML sink using source location.search

The ```innerHTML``` sink doesn't accept ```script``` elements on any modern browser, nor will ```svg onload``` events fire. This means you will need to use alternative elements like ```img``` or ```iframe```. Event handlers such as ```onload``` and ```onerror``` can be used in conjunction with these elements. For example: 

```element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...'```

Lab:

1 - Enter the following into the into the search box:

```<img src=1 onerror=alert(1)>```
    
2 - Click "Search".

The value of the ```src``` attribute is invalid and throws an error. This triggers the ```onerror``` event handler, which then calls the ```alert()``` function. As a result, the payload is executed whenever the user's browser attempts to load the page containing your malicious post.
