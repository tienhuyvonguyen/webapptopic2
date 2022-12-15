# Information Disclosure

# LAB 00: ****Information disclosure in version control history****

## Overall

From the instruction, we want to recon for files or folders that exposed to the public, that contains information that is sensitive and can be used to take advantage.

Then, we may want to traverse through some sensitive exposure that developer may forgot to delete, hide or misconfigure when push to production such as robots.txt, .git, .htaccess…

![Untitled](Information%20Disclosure%2006154dafad704f91afa870074ccc7b67/Untitled.png)

In this lab context, we can go to .git folder contains the following contents. Which is useful for us in some cases.

## Step to reproduce

Travel around and we saw the following contents that contains admin in it, 

![Untitled](Information%20Disclosure%2006154dafad704f91afa870074ccc7b67/Untitled%201.png)

Some more hint for the discover.

Using git-cola, which provide lots function for discover git to help discover and way back the commit which mentioned above. We get the following content when undo the last commit which change the password commit.

![Untitled](Information%20Disclosure%2006154dafad704f91afa870074ccc7b67/Untitled%202.png)

![Untitled](Information%20Disclosure%2006154dafad704f91afa870074ccc7b67/Untitled%203.png)

Successful login.

## Mitigation

Restrict or completely remove the unwanted to public contents and always double check the contents before push to production.

## Appendix