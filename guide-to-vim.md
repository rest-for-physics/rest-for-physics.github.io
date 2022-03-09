---
layout: default
title: Guide to VIM
nav_order: 99
---

# A quick guide to VIM
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Using VIM as an editor might be handy when connecting through ssh using a terminal. There are few advantages versus a traditional text editor when writing code (or any formatted text as for example XML). Advantages such as automatic text formatting, quick variable searches, code indenting, variable and method names auto-completion, tags file and method searches, or even advanced code scripting.

Learning VIM allows us to wrote code efficiently by **avoiding the use of mouse** when editing the files, and move between different files at a common unique terminal. Using VIM we gain in efficiency at doing our work, but it will also help us to avoid common spelling errors. For example, auto-indentation will help us to identify sources of { } ( ) [ ] mismatch, allowing us to early identify sources of error in our code and avoid a usual cause of brainstorming while starting our life as a code developer.

VIM is more than just a powerful editor, it has nothing to envy to the most sophisticated IDE (Integrated Developer Environment) and it is available at every UNIX system where we may need to work remotely.

This document it is intended to give a quick overview describing the VIM commands that I use more commonly and that leads me to the question. How could I live without them before?

### Getting started with *vimtutor*

If you didn’t do that already it is important to catch the basic edition basic features in VIM. Execute the following command from your terminal:

```
vimtutor
```

This will lead you through a 30-minutes interactive tutorial showing you the basics of text edition with VIM, removing entire lines, adding text, the different modes of text selection, cut/paste, copy/paste, etc. *If you want to become efficient in the task of editing text, then, those will be the 30-minutes best invested in your life*, with a bit of practice you will get the text edition, and code navigation to a new dimension.

### Preparing your environment

Once you get used with the basic VIM commands is the time to test more advanced and useful features. First thing you need to do is to configure VIM at your system. For that, I have uploaded the basic scripts I use in my daily life (with high probability they could be better) to the following [GitHub repository](https://github.com/jgalan/basic_scripts). You will need to place those at your `HOME` system.

First, download the repository:
```
cd git
git clone git@github.com:jgalan/basic_scripts.git
```

And then copy the following files to your `HOME` directory (assuming you are now at the `git` directory):

```
cp -r basic_scripts/.vim/ $HOME/
cp basic_scripts/.vimrc $HOME/
cp basic_scripts/updateTags.sh .
```

### Creating a tags file

A tags file is a file that registers important elements on your code, such as variable names, methods, class definitions, etc, and identifies in which file those key elements of your code are found, and at which position inside each file. This will allow VIM to extend its features to a real development environment.

Modify the script `updateTags.sh` (available now at your local `git/updateTags.sh`) to point to any code repository you want to be able to be recognized by VIM using tags.

```
vim updateTags.sh
```

This will generate a file at the present location named `tags`. The tags file used by VIM is defined at the `.vimrc` file that you placed previously at your `HOME` directory. Double-check that it is pointing to the right place/path.

### VIM basic commands (Memory card)

This section will cover the most common used actions when working on a development project with VIM. Most of those are covered by the `vimtutor` you followed previously and they will be placed at this section just for completeness. It is important that you got familiar and get used with the commands you learnt at the VIM interactive tutorial. Still it might be handy to remember some of them.
 
**Searching text** inside VIM is straight forward and it is one of my favourite features. How easy is to move through the text using VIM and the search command. Press the key `/` then write the text you are looking for:

```
/textToFind
```

Press ENTER to find the first coincidence, then press the key `n` to go forward to the next coincidence of textToFind, or `shift+n` (uppercase n) to go to the previous coincidence.

There are different ways of **inserting text** in our file:
 - `i` it enters in edition mode

**Deleting text** 

### VIM operating modes

Although you probably remarked this fact after the tutorial, it is important to highlight that VIM defines 3 different operating modes. To exit any of these modes, just press the key `ESC`. When we are not in any of those modes, we will be able to navigate the file with the powerfull VIM shortcuts. By default, when we enter VIM we are *not* in any of those modes.

 1. **Command mode**: We enter in this mode using the key `:`. We enter in a new infinite universe of possibilities to explore in this mode.
 2. **Visual mode**: We will be able to highlight and select text where we will be able to apply any common edition command. I use it mostly for deleting and copy/paste, and why not edit tables, there are three options to enter visual mode.
	- Enter using character `v`: Just press `v` and move through the text as usual to select text from the character you are placed to the character you are moving.
	- Enter using `shift+v`: It will highlight full lines. For example, press `shift+v` and move up/down to select any number of lines desired. Then, to delete them, press `x`, go somewhere else, and copy those lines `y`.
 3. **Edition mode**: We enter in this mode using different shortcuts, such as `o`, `i`, `a`, `shift+a`, etc.

**Opening files**

**Navigating trhough** different VIM open files.

Replacing text :s/thisone/bythisone/

#### Moving efficienctly through the file

**Moving quickly through the code file**.  w 0 shift+A gg shift+g CTRL+F CTRL+B n

Moving to the beginning of the file. Press key “g” twice: gg

f and F

Moving to the end of the file. Press shift+g.

Moving backward and forward by half page. Press CTRL+F and CTRL+B

### Using the VIM visual mode

**Selecting text** inside VIM is useful so that the commands are executed

#### Code development shortcuts

gd w

**Moving though the whole project** as defined by the tags file:
g and CTRL+]

CTRL+T

:ts file

vim -t keyword

:ls

:bN

**Compiling from VIM**:

:make

:cn

:cN

:!

:syntax on
