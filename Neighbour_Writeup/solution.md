A quick hit in browser http://10.49.185.184.

It gave us a login page.

![main_page](main_page.png)

In the login page it hinted us to do CTRL + U.

Here is the source code.

![source_page](source_page.png)

As we can see in line 34 we have the credentials of guest.

After logging in with guest, it shows

![def_page](def_page.png)

Consider the URL: http://10.49.185.184/profile.php?user=guest

Since this room is about IDOR. So it is simple nature we will try for parameter tampering.

After trying a bunch of values of user when I hit the value=admin. I got the flag. 🚩

Easiest flag ever caught.
