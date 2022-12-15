# File upload vulnerabilities

# LAB 00: **Web shell upload via obfuscated file extension**

## Overall

Noted from the instruction that the blog is vulnerable to file upload, and more specifically is the upload avatar function in user profile.

But also mention that, the blog is blacklist some file extensions, but the problem with blacklisting is that it can not cover all cases that development step can guess out.

![Untitled](File%20upload%20vulnerabilities%2096dbacba62b849b48d287415a21c280c/Untitled.png)

Firstly, we try with the sample webshell write in PHP and the response shown that it only allow jpg and png image file. So, we may want to try change the name of the file to see what happened.

![Untitled](File%20upload%20vulnerabilities%2096dbacba62b849b48d287415a21c280c/Untitled%201.png)

We can upload a webshell written in PHP to server only by adding 1 more extension at the end of the file. Shown that the blacklist works bad in this case, it only check for the last 4 characters and not even the MIME of the file.

## Step to reproduce

Noted from the overall step, the avatar file is using GET method to get the content of the user avatar. We can also use this request to reproduce our attack.

Switch to the case in this lab, we want to get the content of Carlos, which we need to conduce a php code that directly run and read then output the content of the file to the response.

![Untitled](File%20upload%20vulnerabilities%2096dbacba62b849b48d287415a21c280c/Untitled%202.png)

The file name we want to obfuscated by using the null byte that will drop out the end of the rest of the file name once it reach the BE of the server.

Then, with the file name, we still follow the rule that the server only accept the image extension that we will append the file extension that the BE is allowed. 

Then, we saw that in the response, the file name is still only webshell.php, which drop out the .jpg extension. 

So, apparently in the server, there is only webshell.php, which mean we can start the exploit.

With the function file_get_contents built in PHP, we can easily get the whole content of the file with only direct to the php file we uploaded.

![Untitled](File%20upload%20vulnerabilities%2096dbacba62b849b48d287415a21c280c/Untitled%203.png)

## Mitigation

Using whitelist instead of blacklist.

Check the MIME of the file is rcm.

## Appendix