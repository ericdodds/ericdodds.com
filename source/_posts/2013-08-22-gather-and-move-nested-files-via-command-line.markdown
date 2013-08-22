---
layout: post
title: "Gather and move nested audio files via command line"
date: 2013-08-22 15:32
comments: true
categories: Nerdery, Code
---

Over the years my brother has amassed a large music library of 20,000+ songs. He has a great taste for sound, but when it comes to technology his nerd DNA is dwarfed by his [outdoorsman DNA](http://www.flickr.com/photos/ericdodds/8693251917/). After several computer upgrades, multiple iTunes Library restorations and transfers, and no file file management whatsoever, a significant portion of his audio files were corrupt and wouldn't play (via iTunes or in the Finder on his Mac). Music is a very important part of his life, so I set out to help him.

After tinkering for a bit I discovered that running bad files through an mp3 converter fixed the problem—the content was intact, but iTunes was having trouble reading them fully[^1].

Having discovered the fix for individual files, I faced another problem: those items were spread across a directory with thousands of nested folders, some several levels deep in the hierarchy. There's no way I was going to manually move 20,000 files. 

<img src="/images/blog/2013/08/nested-audio-files-1.png" alt="Screenshot of nested audio files several folders deep" style="border: 1px solid gray">

My first crack at automating the process was finding an mp3 conversion tool that would search through folders, find files, and convert them in place. [Switch](http://www.nch.com.au/switch/index.html) seemed like it would do the job initially, but after several failed attempts I figured that some combination of names in the file path was breaking the process. I tried a few more programs unsuccessfully before remembering that I had no need to preserve the existing order as iTunes automatically generates an artist/album/song folder structure when you import new files.

After that, I turned to the terminal to see if I could efficiently gather all of the files into a single clean directory. 

Exploring regex commands[^2] seemed promising[^3] until I realized that I needed to move multiple file types. Frustration drove me to enlist my business partner (and hacker extraordinaire) [Mason Stewart's](http://twitter.com/masondesu) help. 

We discovered that using a unix find command, file name specification, and move command for each individual file types worked like a charm. 

{% codeblock %}
//Use this command to find and move a specific kind of file in a directory. 
//This command searches recursively through nested folders.

$ find ~/old-music-folder -name '*.mp3' -exec mv {} ~/new-music-folder \;
{% endcodeblock %}

*[Here's a link to the gist on GitHub.](https://gist.github.com/ericdodds/6306638.js)*

The result: all of the audio files in a single folder, easily digestible by the mp3 converter.

<img src="/images/blog/2013/08/nested-audio-files-2.png" alt="Screenshot of nested audio files moved to a single directory" style="border: 1px solid gray">

[^1]: More specifically, iTunes recognized the presence of a corrupt file in the library, but couldn't actually play it and would skip to the next song. 

[^2]: Mason showed me [this amazing regex playground](http://leaverou.github.io/regexplained/) by [Lea Verou](http://lea.verou.me/). I still understand very little about regex, but the taste I had showed me that it's an incredibly powerful tool.

[^3]: "Some people, when confronted with a problem, think 'I know, I'll use regular expressions.' Now they have two problems. —Jamie Zawinski"