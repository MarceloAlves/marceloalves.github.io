---
layout: post
title:  "Create Images from PDF's Using Lambda"
date:   2016-02-26 17:00:00
categories: tutorials lambda
banner_image: "/assets/img/header/2015-02-26-pdf_to_image_thumbnail_using_lambda.jpg"
featured: true
comments: true
---

Easily create thumbnails from uploaded PDF's

<!--more-->

A recent project of ours needed thumbnails to be generated from PDF's that were uploaded to S3. It took some time searching but I finally found a script from [<i class="fa fa-github"></i> kageurufu](https://github.com/kageurufu) that did exactly what I was looking for. Unfortunately there wasn't anything on how to actually set it all up so here's a handy guide!

## Create The Project

1. Download this [gist <i class="fa fa-external-link"></i>](https://gist.github.com/kageurufu/68667fcb6d3a08078616) and save it in a new folder as `index.js`
2. In a terminal window run the following commands (replace `/path/to/your/project/folder` with the actual path of course)

{% highlight bash %}
  $ cd /path/to/your/project/folder
  $ npm init
  $ npm install --save gm async mktemp
{% endhighlight %}

You can modify the desired thumbnail size on line 8 and 9. The resulting thumbnail will fit within the bounds specified. Once you're finished, zip up the contents of the folder. Make sure you compress the files and not the folder itself.

## Set up S3

Create a new bucket where the PDF files will be stored. Inside that bucket create a folder named "thumbnails". The Lambda function and S3 Bucket need to be in the same region.

## Set Up Lambda

1. Visit Lambda in your AWS Console. If this is your first Lambda function there will be an option labeled "Get Started Now". If not, select "Create a Lambda Function"
2. Since we have our own function to upload, we won't be selecting a blueprint. Click "Skip"
3. Give your Lambda function a name. Here's a free one: `pdfThumbnailer` (clever right?)
4. Select "Upload a .ZIP file" then click "Upload" and select your newly created zip file
5. Handler should be left as `index.handler` since the main file is `index.js`
6. For Role select "S3 Execution Role". We'll need to make a small tweak to this role. In the new window, hit "View Policy Document" then "Edit". Replace the contents with:

    {% gist MarceloAlves/cd68813995edbc0284e8 %}

    The small change we've made was added `s3:PutObjectAcl`

7. If the PDF are pretty large you can increase the timeout value. Our PDF's would convert somewhere around 6 seconds.
8. Click "Next", verify everything you've put in is correct then hit "Finish"

## Event Source

Now that you have a newly created Lambda function we need to add an Event Source. Visit the Event Source tab and select "Add event source".

* Select `S3` as your Event source type
* Select the bucket where your PDF's will be uploaded
* Select Object Created (All) as the Event type
* If you'd like to limit the filetype to PDF only, put in `pdf` for the Suffix

Hit "Submit" to finish

## Test It

You've got a Lambda function and now an Event source so whats next? Well test it of course. The easiest way to do this is to upload a PDF file to your bucket. Within a second or two you should have a brand new PNG in the "thumbnails" folder with the same name of the PDF you've uploaded.

## Conclusion

If everything works out you'll have a fancy Lambda function that can create a thumbnail image from a PDF file. This function can actually be reused for anything you need a thumbnail for including other images. This only scratches the surface of what Lambda can do. Definitely check out the [docs <i class="fa fa-external-link"></i>](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html) and have some fun!
