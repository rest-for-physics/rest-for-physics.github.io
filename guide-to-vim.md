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

Using VIM as an editor might be handy when connecting through ssh using a terminal. There are few advantages versus a traditional text editor when writing code (or any formatted text as for example XML). Advantages such as automatic text formatting, quick variable searches, code indenting, and variable and method names auto-completion, tag file and method searches, or even advanced code scripting.

Learning VIM allows us to wrote code efficiently by avoiding the use of MOUSE when editing the files, and move between different files open at a common unique terminal. Using VIM we gain in efficiency at doing our work, but it will also allow us to avoid common spelling errors. For example, auto-indentation will help us to identify sources of { } ( ) [ ] mismatch, allowing us to early identify sources of error in our code and avoid a usual cause of brainstorming.

Therefore, this document it is intended to give a quick overview describing the VIM commands that I use more commonly and that leads me to the question, how could I live without them before?

### Getting started with *vimtutor*

If you didn’t do that already it is important to catch the basic edition features in VIM by executing in your terminal

```
vimtutor
```

This will lead you through a 30-minutes interactive tutorial showing you the basics of text edition with VIM, removing entire lines, adding text, the different modes of text selection, cut/paste, copy/paste, etc. If you want to become efficient in the task of editing text, then, those will be the 30-minutes best invested in your life, with a bit of practice afterwards you will get the text edition and code search to a new dimension.

Once you get used with the basic VIM commands is the time to test more advanced and useful features. First thing you need to do is to configure VIM at your system. For that, it is possible to copy just few files I hold under my HOME into your HOME and GIT directories.

```
cp -r /home/jgalan/.vim/ $HOME/
cp /home/jgalan/.vimrc $HOME/
cp /home/jgalan/git/updateTags.sh $HOME/git/
```

Update the paths inside the copied “updateTags.sh” file in your local git/updateTags.sh to point to any code repository you would be able to index using tags.

cd $HOME/git/
vim updateTags.sh

Once you have followed these steps …


 
Searching text inside in VIM is straight forward. Press the key “/” then write the text you are looking for:

/textToFind

Press ENTER to find the first coincidence, then press the key n to go forward to the next coincidence of textToFind, or shift+n (uppercase n) to go to the previous coincidence.

Selecting text inside VIM is useful so that the command executed

Replacing text

Moving quickly through the file. 

Moving to the beginning of the file. Press key “g” twice: gg

Moving to the end of the file. Press shift+g.

Moving backward and forward by half page. Press CTRL+F and CTRL+B

