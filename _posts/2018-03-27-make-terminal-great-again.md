---

title: "Make Terminal Great Again"
categories: [general, coding]
tags: [general, coding, blog, notes]

---
### Preface

For the past two years, most of my workflow has been inside a terminal and editing files with vim. I've gone down the rabbit hole of learning around a unix-like OS.

It is definitely worth all the time and research put in. There's an overwhelming amount of tools at disposal but as I continue to discover, I become a faster, **less frustrated** coder.

Most of my peers use *Atom*, *Sublime Text*, or *VSCode* to write their code. And none of them are bad! They're all fantastic text editors. This isn't about bashing others for the tools they use. I'm a novice coder but I have great joy in learning how to what I need in vim and a plain terminal.


---
### Onto the setup

Here's the basics of my coding environment on a macOS machine:
 - iTerm2
 - NeoVim
 - Dracula theme
 
That's it. All other dependencies and tools for any projects I can pull with Brew.

Here's a bit more explanation of these tools:

**1. iTerm2 > Terminal**

iTerm2 has truecolor support which is great for tinkering with the *right* terminal theme. That's the only reason I switched over.
[Read about it here.](https://gist.github.com/XVilka/8346728)
[iTerm2 can be found here.](http://www.iterm2.com)

Here's a quick comparsion showing the color reproduction found in iTerm vs stock Terminal:
![Neomake](/assets/images/2018-03-27-make-terminal-great-again/image1.png)

**2. Using Dracula as a colorscheme**

Vim's default colorscheme is rather bad. I found Dracula in a search for something easy on the eyes yet had a bit of color variety in it. It works great on a HiDPI display such as my Macbook or my LG 4k display.


I have Dracula as my theme for both iTerm2 and for vim.
[Here's the home of Dracula.](https://draculatheme.com)

**3. Using NeoVim as an editor**

Vim is a fantastic text editor as it has plenty of shortcuts to traverse and edit manipulate code. After climbing over the learning curve of the shortcuts required to use vim, it becomes a rather fun experience.

Let's go straight into an example:

Here's a snippet of code from my Halo 5 HUD project:

```python
ammoFont = Font(family = "Arame", size = (root.winfo_screenheight() - 50))

here's	here's	here's	here's
garbage	garbage garbage garbage
that	that	that	that
I	I	I	I	
should	should	should	should
delete	delete	delete	delete

Label(root, bg = "#02262d", fg = "#2DFCFB", font = ammoFont, textvariable=ammoVar).pack(fill="none", expand=True)

root.configure(background='#02262d')
```

Let's say I need to change the background color **#02262d** to **#57766b**. I also want to delete the garbage code. Lastly,

Without the powers of vim, that involves:
- Hunting every instance of #02262d, pressing highlighting and deleting the old color, then typing the new color. Then repeat for every instance.
- Having to use the mouse to highlight the garbage code and then deleting.

There's a lot of mouse movement and repetition.

Let's try that again with vim:
1. Enter Normal mode by pressing Esc.
2. Type /%s/02262d/57766b/g.
3. Type 3gg to jump to the first line of garbage code. Press SHIFT+V.nter. Type 6ggd.

That's it!

At first these commands are gibberish but with a bit of documentation-sluething, they're invaluable text manipulation tools. Modern text editors come with a vim mode of some sort as well.

Plug-ins also extend the functionality to bring IDE-like features to the editor.

Here's how editing C++ code looks like when I have an error thanks to [neomake](https://github.com/neomake/neomake):
![Neomake](/assets/images/2018-03-27-make-terminal-great-again/image3.png)

And here's how autocompletion looks with [nvim-completion-manager](https://github.com/roxma/nvim-completion-manager):
![autocomplete](/assets/images/2018-03-27-make-terminal-great-again/image4.png)


Thanks to Neovim's popularity, async support is now found in stock Vim! As of writing this post, the plugins I have are configured for Neovim. If I were to do a fresh setup, I'd probably set them up for normal vim.

I'll leave my Vim explanations up to here. I'd love to further talk about Vim in another blog post.

Stop by [Neovim's website!](https://neovim.io/)

---
### To be Continued

This serves as a long-winded introduction to my editing setup. As time goes on I'll continue to document more.
