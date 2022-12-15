# Access control vulnerabilities - IDOR

### LAB 00: ****Multi-step process with no access control on one step****

### Overall

The blog is vulnerable to access control which is when promoting a user from normal user to admin, it take 1 more step to confirm the process, which lead to the vulnerable.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled.png)

Noted with normal user session, trying to access the admin panel or try to exploit the IDOR vulnerable it will not work at all, or even trying to access the page where the change user privilege take action will response to unauthorized.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%201.png)

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%202.png)

### Step to reproduce

Stay at the normal user session, you can only access to yourself page which is /my-account?id=wiener.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%203.png)

Noted when we get familiar with the admin panel, /admin will be the admin profile page and /admin-roles will be the place where user role will be changed, it take 1 more step undercover to confirm the process.

And of course, the form use POST method. Proved with 2 pictures below.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%204.png)

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%205.png)

So, the target we need to do here is to directly make the second process which is the confirmation process run and promote user wiener to admin immediately.

So right here we may want to use another function that wiener is allowed to use is change the email.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%206.png)

Noted that both changing email which is the method allowed for normal user is 100% similar to the request allowed only to admin to perform the promotion. 

Which allowed us to conduct the AC attack by using the request to changing normal user email again, change some content of the request and send it to the server.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%207.png)

Switching the method to GET, which allow us to put parameters that perform the same as POST method on the /admin-roles page, change the referrer to /admin-roles because at this point, you will tell the server that you already passed the 1st stage of promoting the user, which is [this step.](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711.md)

Then, the body of the request will contains the information that will similar to the 2nd process which is [this step.](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711.md) 

This step will force the server to understand that the request you push to the server already passed the 1st process and ready to run the 2nd process.

But due to the broken access control, the 2nd process solely depend on the 1st process, pretended that the 1st process run then the 2nd process do not need to check the AC again, lead to the vulnerable.

Sending the request will respond you redirect to the 200 page. Promoting wiener to admin privilege. 

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%208.png)

Always noted that the session is normal user which is wiener to the whole lab, but due to mimic the ways change email function perform to exploit the vulnerable that can promote wiener to admin.

### Mitigation

Always double check the credential of every process made from the request

Mostly use a single-wide application mechanism for the entire application.

### Appendix

Contains some real cases:

[https://medium.com/@cxosmo/owasp-top-10-real-world-examples-part-1-a540c4ea2df5](https://medium.com/@cxosmo/owasp-top-10-real-world-examples-part-1-a540c4ea2df5)

## LAB 01: ****Method-based access control can be circumvented****

Noted that /admin-roles can be access with GET at the login page , without any credentials, and also, this page performs a change user role function if you login as administrator like the picture below. Its need 2 params to perform the function and from the admin, its a POST method which is safer and more securer to GET method. 

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%209.png)

But the flaw appear when this page can be compromised from the outside without any credentials with GET method, which mean we can pass some param in the URL in this case and try to figure out the behavior of the website.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2010.png)

So, we already noted that the GET method in this page can be used to perform task, we try to login as wiener which is a normal user and find to the /admin-roles page which seem vulnerable. Then we noted that the output is asking for a params, which seen nice to us.

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2011.png)

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2012.png)

Then we try to put the params like we have familiar in admin panel above. We got 302 found redirect and follow the redirect we found that there is a 200, double check it we can see that wiener has been promoted to admin…

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2013.png)

Mitigate: same as login, validate for the session of the admin 

## LAB 01: ****Unprotected admin functionality****

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2014.png)

robots.txt exposed & admin panel exposed

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2015.png)

![Untitled](Access%20control%20vulnerabilities%20-%20IDOR%20215faa5ea8384d0d8d9c816eac1b3711/Untitled%2016.png)

Extension

Auth analyzer