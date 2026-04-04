# Part 1 Reconnaissance

First Thing first we get ip and connect to vpn then add the ip to /etc/hosts and runs a nmap scan cuz the first question is 
```
Scan the machine, how many ports are open?
```
After doing the simple nmap scan with nmap -sV <room_ip> i got

<nmap_room>

We can clearly see how many ports are open.

This nmap result also reveals the answer of 2nd and 3rd question.

4th Question Doesn't needs an answer.

Now the 5th Question We will use ffuf without any trouble of any additional tools.

The command we will use for it is ffuf -u http://<room_ip>/FUZZ -w /home/Seclists/Discovery/Web-Content/common.txt 

<ffuf_res>

As we can see the unusual directory name that matches perfectly with the no of words required in 5th question is a notable directory. 

# Part 2 Initial Foothold

At the hidden endpoint as we hit that point into the browser it allows us to upload files to server. which can be viewed at the another endpoint that is already found in ffuf result.

The malicious file uplaod can lead to rce. since tha page is a php file we will use <?php system($_GET['cmd']); ?> to within our file that we upload in the server. To check that sever allows us to rce or not. for this we will first unsure file uplaods then we care about its execution

It allows us to upload all filetypes except php. It filter those files that have php extention. But after a bunch of tries like: -

Try 1  Modified filename with jpg and content type to image/jpg by burp interceptor.

Try 2  Modified filename with txt and content type to plain/txt by burp interceptor.

Try 3  Straight filename with php and content type to application/x-php by burp interceptor.

Try 4  Modified filename with .php.jpg and content type to image/jpg by burp interceptor.

Try 5  Modified filename with .php.txt and content type to plain/txt by burp interceptor.

Try 6  Straight filename with php and content type to image/jpg by burp interceptor.

Try 7  Straight filename with php and content type to plain/txt by burp interceptor.

Try 8  Modified magic header of jpg + Modified filename with jpg and content type to image/jpg by burp interceptor.

Try 9  Modified magic header of jpg + Modified filename with txt and content type to plain/txt by burp interceptor.

Try 10 Modified magic header of jpg + Modified filename with php and content type to application/x-php by burp interceptor.

Try 11 Modified magic header of jpg + Modified filename with .php.jpg and content type to image/jpg by burp interceptor.

Try 12 Modified magic header of jpg + Modified filename with .php.txt and content type to plain/txt by burp interceptor.

Try 13 Modified filename with .php5 and content type of plain/txt by burp interceptor.

<file_upload>

# Getting a shell - Part 3
After the file succeesfully uplaods we will check its execution at the uploaded endpoint.

We will hit this url http://<ip>/<upload_endpoint>?cmd=whoami

If we get any text congrats we can now get a shell.

Go to revshells.com and copy the ip under your tun0 interface and enter there as ip and now onto your local machine set up the listerner by nc -lvnp 4444. and also copy the reverse shell command and generate one with python simplest

As you hit this endpoint

```
http://<ip>/<upload_endpoint>/cmd=<reverse_shell_command>
```
You will get your connection back into the terminal and you have a shell now.

Now do find here and there and you will get the user flag at /var/www in the shell. 

<user_flag>

And in this way Q6. gets solved.

# Privilage Escalation Part - 3
Now Get to our Q7. we will execute the command that we have given in the hint to the rev_shell

<perm_image>

Now spot the file from the above image that which file permission is weired.

And after that Name that file into the question 7.

Now as the hint of Q8 tells us that search for that binary on gtfobins.

When i searched for it i got

<gtfo_bins>

Here you can clearly see the command python3 -c 'import os; os.execl("/bin/sh", "sh")'

But this command was not working. Its a classic one

it failed cuz the binary might have suid bit set, but when you spawn a shell through os.execl, the shell drops privilages for safety. Modern bin/sh(dash) and /bin/bash do this by default.

We are going to use -p to keep privilages the **working** command is 
```
python2.7 -c 'import os; os.execl("/bin/bash", "bash", "-p")'
```

The -p option tells bash don't drop the SUID privileges.

Now doing the similar enumuration like Q6. You will got your answer as find into it.
