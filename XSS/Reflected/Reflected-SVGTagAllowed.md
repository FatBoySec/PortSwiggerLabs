### Reflected XSS with some SVG markup allowed


1 - Inject a standard XSS payload, such as:
    ```<img src=1 onerror=alert(1)>```
    
2 - Observe that this payload gets blocked. In the next few steps, we'll use Burp Intruder to test which tags and attributes are being blocked.

3 - Open Burp's browser and use the search function in the lab. Send the resulting request to Burp Intruder.

4 - In Burp Intruder, in the Positions tab, click "Clear §".
    
5 - In the request template, replace the value of the search term with: ```<>```

6 - Place the cursor between the angle brackets and click "Add §" twice to create a payload position. The value of the search term should now be: <§§>

7 - Visit the XSS cheat sheet and click "Copy tags to clipboard".

8 - In Burp Intruder, in the Payloads tab, click "Paste" to paste the list of tags into the payloads list. Click "Start attack".

9 - When the attack is finished, review the results. Observe that all payloads caused an HTTP 400 response, except for the ones using the ```<svg>```, ```<animatetransform>```, ```<title>```, and ```<image>``` tags, which received a 200 response.

10 - Go back to the Positions tab in Burp Intruder and replace your search term with:
```<svg><animatetransform%20=1>```

11 - Place the cursor before the = character and click "Add §" twice to create a payload position. The value of the search term should now be:
  
```<svg><animatetransform%20§§=1>```
    
12 - Visit the XSS cheat sheet and click "Copy events to clipboard".
    
13 - In Burp Intruder, in the Payloads tab, click "Clear" to remove the previous payloads. Then click "Paste" to paste the list of attributes into the payloads list. Click "Start attack".
  
14 - When the attack is finished, review the results. Note that all payloads caused an HTTP 400 response, except for the ```onbegin``` payload, which caused a 200 response.

  Visit the following URL in the browser to confirm that the alert() function is called and the lab is solved:
    ```https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E```

