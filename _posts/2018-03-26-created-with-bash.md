---

title: Created With Bash
categories: [general, coding]
tags: [general, bash, coding, blog, notes]

---

#### [*EDIT:* This script has been superceded. Check here for the updated version.](http://tbender.io/general/coding/2018/04/11/learning-more-about-bash.html)


This post has been quickly created thanks to what is probably my first real Bash script. It took a *LOT* of trial and error.


----
### What it does

1. Asks for the title of the blog post
2. Creates a markdown file with the appropriate date and file name formatting
3. Opens markdown file in vim, ready to be edited!


----
### Source Code

~~~~
#!/bin/bash

read -p "Title: " title
if [ -z "$title" ]
then
	echo "No title entered"
	exit 1
fi

#Goals: 1. Convert to lowercase. 2. replace spaces to dashes
#Input is not sanitized! Do this later.

titleNoSpaces="${title// /-}"
titleConversion=$(echo $titleNoSpaces | tr '[:upper:]' '[:lower:]')
echo $titleConversion

#titleConversion="${titleNoSpaces,,}"
#the Macs at Queens College are outdated and have Bash 3.2. Using tr instead.



today=`date '+%Y-%m-%d'`
filename="$today"-"$titleConversion.md"

cat <<EOF > $filename
---
layout: post
title: "$title"
categories: []
tags: []
fullview: true
---

post goes here

EOF


vim "$filename"



#Sources:
#https://askubuntu.com/questions/615178/getting-the-default-text-editor-used-in-system
#https://stackoverflow.com/questions/18544359/how-to-read-user-input-into-a-variable-in-bash
#http://tldp.org/LDP/abs/html/comparison-ops.html
#https://stackoverflow.com/questions/1706431/the-easiest-way-to-replace-white-spaces-with-underscores-in-bash
#https://stackoverflow.com/questions/11159043/bash-tr-command
~~~~



The script has potential. Here's on the todo list:

 - Sanitize inputs properly
 - Support other editors
 - Have functionality past the editing phase.


----
### Closing notes

I am learning everything as I go. Creating a bash script was a challenge on its own, let alone the rest of the setup. Just last week I've never touched Markdown, Ruby or Jekyll, or even ran a GitHub-hosted page. 