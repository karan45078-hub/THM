As we hit the url that we got "http://10.48.136.217/" Into the browser it gave us a homepage of songs that looks like this 

<homepage_pic>

Here is the source code 
```html

<!DOCTYPE html>
<html lang="en">

<head>

    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"
        integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">

    <style>
        body {
            padding-top: 56px;
            padding-bottom: 56px;
            min-height: 100vh;
            position: relative;
        }
        .footer {
            bottom: 0;
            width: 100%;
            position: absolute;
            height: 56px;
        }
    </style>

    <title>Lo-Fi Music</title>
</head>

<body>

    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
        <div class="container">
            <a class="navbar-brand" href="/">Lo-Fi Music</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarResponsive"
                aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarResponsive">
            </div>
        </div>
    </nav>

    

        <!-- Page Content -->
    <div class="container">

        <div class="row">

            <!-- Blog Entries Column -->
            <div class="col-md-8">

                <h1 class="my-4">Cool beats to listen to</i></h1>


                <!-- Blog Post -->
                <div class="card mb-4">
					<div class="card-body">


						<h2 class="card-title">Relax</h2>

<p class="card-text">
    <iframe width="680" height="360" src="https://www.youtube.com/embed/5qap5aO4i9A" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>                	</div>
                </div>

            
            </div>

            <!-- Sidebar Widgets Column -->
            <div class="col-md-4">

                <!-- Search Widget -->
                <div class="card my-4">
                    <h5 class="card-header">Search</h5>
                    <div class="card-body">
                        <form action="/" method="get">
                            <div class="input-group">
                                <input type="text" class="form-control" name="search" placeholder="Search for...">
                                <span class="input-group-btn">
                                    <button class="btn btn-secondary" type="submit">Go!</button>
                                </span>
                            </div>
                        </form>
                    </div>
                </div>

                <!-- Categories Widget -->
                <div class="card my-4">
                    <h5 class="card-header">Discography</h5>
                    <div class="card-body">
                        <div class="row">
                            <div class="col-lg-6">
                                <ul class="list-unstyled mb-0">
                                    <li><a href="/?page=relax.php">Relax</a></li>
                                    <li><a href="/?page=sleep.php">Sleep</a></li>
									<li><a href="/?page=chill.php">Chill</a></li>    
									<li><a href="/?page=coffee.php">Coffee</a></li>
									<li><a href="/?page=vibe.php">Vibe</a></li>
									<li><a href="/?page=game.php">Game</a></li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
        <!-- /.row -->
    </div>
    <!-- /.container -->


    <!-- Footer -->
    <footer class="py-3 bg-dark footer">
        <div class="container">
                &nbsp;
        </div>
        <!-- /.container -->
    </footer>


</body>

</html>
```

As we can see source code is not giving any clue to flag.

So we proceed to Directory enumuration and it gave us

```text
[23:34:48] 403 -  244B  - /.ht_wsr.txt                                      
[23:34:48] 403 -  242B  - /.htaccess.bak1                                   
[23:34:48] 403 -  243B  - /.htaccess.sample                                 
[23:34:48] 403 -  242B  - /.htaccess.orig
[23:34:48] 403 -  242B  - /.htaccessBAK
[23:34:48] 403 -  244B  - /.htaccess_extra
[23:34:48] 403 -  241B  - /.htaccessOLD
[23:34:48] 403 -  243B  - /.htaccess_orig
[23:34:48] 403 -  241B  - /.htaccess.save
[23:34:48] 403 -  242B  - /.htaccess_sc                                     
[23:34:48] 403 -  242B  - /.htaccessOLD2
[23:34:48] 403 -  237B  - /.htm                                             
[23:34:48] 403 -  238B  - /.html                                            
[23:34:48] 403 -  246B  - /.htpasswd_test                                   
[23:34:48] 403 -  241B  - /.httr-oauth                                      
[23:34:48] 403 -  241B  - /.htpasswds     
[23:35:44] 403 -  241B  - /server-status/                                    
[23:35:44] 403 -  240B  - /server-status  

```
But this  also doesn't gave us a Interesting endpoint for the further enumuration.

In the Homepage under the section Discography When tapped on Relax the url changes to **http://10.48.136.217/?page=relax.php**

So here we can do fuzzing on this url for different pages.

<fuzz_res>

As we can see it has only listed 2 endpoints.

So we are trying manually different values for the page=

and in this many tries wehn i put page=/home it showed me something interesting.

<hackerr_spot>

but with some steps i understand what whatever will be after / no matter it prints *HACKKERRR!! HACKER DETECTED. STOP HACKING YOU STINKIN HACKER!*

Now we know it somehow reacts to some payload so this room is about LFI path transversal and File Inclusion.

We need to do fuzzing with the wordlist mainly focused on LFI path transversal and File Inclusion. As it webserver is apache.

```
Not Found
The requested URL /fas was not found on this server.

Apache/2.2.22 (Ubuntu) Server at 10.48.161.15 Port 80
```

Now we do fuzzing with those having the websever as apache. The wordlist is */home/Seclists/Discovery/Web-Content/Web-Servers/Apache.txt* with the command

```
ffuf -u http://10.48.161.15/?page=FUZZ -w /home/Seclists/Discovery/Web-Content/Web-Servers/Apache.txt
```
