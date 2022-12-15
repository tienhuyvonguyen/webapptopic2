# Business logic

# LAB 00: ****Weak isolation on dual-use endpoint****

## Overview

![Untitled](Business%20logic%20c9ec7f5859b14ed498696b4a16311ba9/Untitled.png)

We start with recon blog’s function, and noted that the change passwd function is kind of behave some behavior that can be a flaw lead to a vulnerability. 

We may try to many cases but if you try to remove parameter from the request, we may discover some interesting weird behavior of the logic.

![Untitled](Business%20logic%20c9ec7f5859b14ed498696b4a16311ba9/Untitled%201.png)

Noted from the above shoot, we may see that if we completely remove the current-password parameter, the process is still go on and response with successful change the password.

## Step to reproduce

So, from here we already noted that the blog’s passwd changing have a big flaw in the logic of the process. And the target of our lab is to change the admin passwd, then we can use this credential to login and perform administrator functions.

![Untitled](Business%20logic%20c9ec7f5859b14ed498696b4a16311ba9/Untitled%202.png)

So, cause of the logic flaw, we may reproduce the vulnerable by change the username to administrator and take whatever passwd we want. 

The response is 200 successful change the admin passwd.

![Untitled](Business%20logic%20c9ec7f5859b14ed498696b4a16311ba9/Untitled%203.png)

## Mitigation

Fix the logic from the code of the program and perform 1 more check again in the backend, make sure that the logic from both the FE and BE is implemented correctly.

## Appendix