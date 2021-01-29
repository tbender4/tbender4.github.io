---

title: "Learning More About Bash"
categories: [general, coding]
tags: [general, bash, coding, blog, notes]
fullview: true
comments: true


---

### The Bash Continues
----

My first blog post was with tinkering with bash and VMWare. Upon deploying this jekyll blog and learning it's Markdown-driven format for creating blog posts, I realized that creating posts from scratch is an inconvenient, repetitive process.

The solution is to automate!

The initial code was rather rough around the edges. It took a long time to get the blog up and running so I wrote the bare minimum. Now that I'm incorporating images into my blog, it felt right to continue working on the script.

### What It Does
----

Enter in the title and the script will do the rest. It'll format the blog post's filename with the correct date and title format. The images directory wil also be created.

	$ ./create-post.sh
	Title: My Dank Blog Post
	$ ls | cat
	create-post.sh
	2018-04-11-my-dank-blog-post.md
	$ ls -F ../assets/images/ | cat
	2018-04-11-my-dank-blog-post/

Upon entering the title, you'll be taken straight to your editor editor as follows:

<div class="demonstration-video">
  <video  style="display:block; width:85%; height:auto;" autoplay controls loop="loop">
   <source src="{{ site.baseurl }}/assets/images/2018-04-11-learning-more-about-bash/demonstration.mp4" type="video/mp4" />
   </video>
</div>

### New Features
----

* Edits post with user's default editor
Instead of hardcoding vim, I've set it to use the user's $EDITOR. If it is not defined, vim or vi will be the backup editor. Instructions on how to set it are given if the user decides to terminate.

* Checks if post exists before overwriting.

* Creates image directory.

* General code cleanup.

There's a lot of code now. Below is a link to the GitHub if you wish to go to the next section.


### Clever solutions
----

Pulling together the code to make this happen with my current knowledge was a lot headsratching and research. Here's some notable bits of accomplishment:


	warning="${bold}WARNING: ${normal}Default text editor has not been set.\nDefaulting to ${bold}$defaultEditor${normal}. Continue? (Y/n): "

	read -p "`echo -e $warning`" confirm	#echo -e allows escaping in warning message

The prompt in the `read` command in Bash doesn't abide by escape sequences such as /n for newlines. The workaround is to create a string variable of the warning message first. Then `echo -e`  is used as part of the prompt.

	titleConversion=$(echo $titleNoSpaces | tr '[:upper:]' '[:lower:]')

The Mac computers in Queens College are not up to date. The version of OS X installed uses Bash 3.2, I had to resort to piping into the `tr` command to convert characters to lowercase.

	cat <<EOF > $filename.md
	...
	EOF

I wanted to file to be the barebones of a post without having to memorize the format. Creating this body of text has conflicting syntax and newlines that isn't cooperating with bash. `EOF` made this possible! I was able to write out the entirety of the basic file without having to insert any escape scquences and it remains fairly legible. This is also my first time using `<<`. Will definitely look up its proper usage next time.

### GitHub
----

[This script is now available by itself on GitHub!](https://github.com/tbender4/create-blog)

I won't force too many features onto this script. Now I'm back onto finishing the rest of my projects.