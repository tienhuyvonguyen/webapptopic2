# Directory Traversal

## LAB 00: ****File path traversal, traversal sequences stripped with superfluous URL-decode****

### Overall

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled.png)

The blog is vulnerable to path traversal when loading image file is handle by /image but not properly defense against path traversal.

### Step to reproduce

In the context of the lab, we already know that the blog will decode the content of the URL once it reach the server before its start to process. 

So, what if we pretend that we will URL encode the payload we input right before submit the payload, then once it reach the backend, it will start URL-decode the input, but in this context, we already encode it, which mean when it URL-decode the payload, the exploit is accepted by the backend cause to path traversal.

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%201.png)

The browser will automate url-encode the payload, but cause of us already encode it 1 more time before submit, after the backend decode the payload, the correct payload to exploit is success.

### Mitigation

Validate input

Access control 

### Appendix

[Directory Traversal Mitigation: How to Prevent Attacks](https://brightsec.com/blog/directory-traversal-mitigation/)

Real case newest bypass WAF:

[https://twitter.com/nav1n0x/status/1602305654416367616?s=20&t=nWm19ekjANltggC3wQoMXQ](https://twitter.com/nav1n0x/status/1602305654416367616?s=20&t=nWm19ekjANltggC3wQoMXQ)

## LAB 01: **File path traversal, simple case**

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%202.png)

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%203.png)

## LAB 02: ****File path traversal, traversal sequences stripped non-recursively****

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%204.png)

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%205.png)

```bash
....//....//....//etc/passwd
```

## LAB 03: ****File path traversal, traversal sequences blocked with absolute path bypass****

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%206.png)

![Untitled](Directory%20Traversal%20ec0e332278224f3fbe00041a6f842903/Untitled%207.png)

```bash
/etc/passwd
```