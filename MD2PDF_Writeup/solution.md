So after hitting the ip in browser http://10.49.152.161/. The page looks like 

<homepage_look>

In its text field when i entered some text and hit convert to pdf it is gone through some background process and gave a pdf file with our written text to download.

Now As we will see the text field we first think of injecting something that triggers a effets.

So first i tried with normal text and nothing happened. But as i moved further and gets onto html injection. It gets affected.

```html
<h2>Hello</h2>
```

The string Hello was in big letters.

<html_inj>

So I tried many payload but nothing worked so far.

Then i decided to do directory enumuration on it.


The result were

<directory_enm>

Now when visited to that endpoint it was telling me 

<admin_look>

since it could be only acesssible via intenally. The only process that is happening via internally is out text gets converted to pdf.

So we will convert that endpoint into pdf but it will look like we are acesssing the page.

with thee payload

```html
<iframe src="http://localhost:5000/admin"></iframe>
```


Now this will gets us present the page admin to us and the endpoint contains the flag.
