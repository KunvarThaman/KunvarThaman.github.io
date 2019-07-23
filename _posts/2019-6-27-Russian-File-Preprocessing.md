---
layout: post
title: Russian File pre-processing
---

## Week 1 (May 27 – June 2)

Firstly, I read the pages on Red Hen Data format, compared with the text files on Gallina to understand what denoted what, the coding standards etc.

Then I installed Pragmatic Segmenter and UDPipe, and went through their how to use guides.

Then I used the preprocess.py script on the above mentioned text files to understand how the relevant text is extracted from NewsScape files.

Finally, I Ran the Pragmatic Segmenter with a some files, understood the problem Pragmatic segmenter has with quotation marks, and how quotefix.rb resolves it.

This week was the first one and I spent a lot of time understanding what files did what. In a nutshell, it was a lot of experimenting with different files to see how they worked, and setting up everything, and not much actual code/work. (For example, on the second day, my key got deleted from authorized_keys and I couldn’t even log in to my account, and wasted that day figuring this out).

## Week 2: June 3 to June 9

First of all, I went through an overview of the pipeline from my mentor, Peter Uhrig’s notes.

I didn’t have proper knowledge of why utf-8 is better from other encoding so I looked it up and went through some common questions and problems.

My first task was to improve the conversion of broken files (files which come with some wrong character encoding). The existing code tries to open the file with utf-8 encoding. I checked if the encoding was actually being used in a variety of files. I wrote a script check_encode.py to do this, which is actually straightforward. In my updated version of the convert_broken_russian.py code, I tried using utf8_decode function but it was disappointing as Cyrillic characters are turned into a ‘?’ with it.

Then, I saved all these test files with utf-8 forefully. This gave positive results. After that, I went to the outputs of some of these files and tried to reformat the XML using Notepad++.

After it, this time included a lot of experimentation with encoding and decoding everything and trying different rearrangements. It involved checking things like did the output XML files include encoding for characters like ‘<’, ‘>’, etc. Of course, I also checked if there were any spaces in any tags which might cause complications. In the end, I was able to solve this problem and the tags weren’t being broken. The remaining task was to check the code on a range of files and see if it works. (The objective was to preserve things such as tags). I’ll be updating this code on the GitHub for this project soon.

Also, I ran the quotefix.rb on the files and ran them through pragmatic segmenter.

Towards the end of the week I caught fever and couldn’t work properly, so I took some time off and postponed the remaining work into the next week.

## Week 3: June 10 to June 16
There were some files which weren’t being processed completely. My task was to find out why and fix it. I isolated some of the files and decided to compare them with some normal files. I tried finding any major differences manually but didn’t find any. Was the encoding in some regions of the text file not proper? In the preprocessing script, the files are opened and read directly:

with open(args.input_file) as f: content = f.readlines() content = [x.strip() for x in content]

I figured that it’s possible that some regions of the file weren’t in the standard encoding and that was causing the problem.

So, I wrote a small script to enforce the utf-8 encoding on these broken files. In a couple of files I got errors. I took one of such files and split it into two parts to see if there was problem in both halves. Surprisingly, one of the halves had utf-8 encoding done properly, while the other showed error still. After a bit of digging around, I hypothesized that there were some Russian characters, which when encoded into utf-8, maybe fell into the range of utf-8 set aside for control characters. Not entirely, but since some of them are multi-byte, a small part may be overlapping. I’m yet to verify this with some of other files, but it very well could be the reason for the error. If this is the case, there is the question of how to deal with these characters. (Note: In utf-8, any character above 127 is represented by some sequence of two or more numbers.)

I had to do some research on this and found several other potential reasons as well. It’s a task of trial and error to see which ones are causing problems. One possible problem could be that ultimately, there are only numbers to be dealt with and a single character may get stored as 2 different numbers (like 208/159), and when retrieved, we get two numbers back instead of just one. For this, I’ll ensure that data goes in and comes out in proper formats. Of course, in UTF-8, we always know where we are, unlike many other multi-byte character encodings, which makes my job easier by eliminating certain sequences which won’t occur naturally.

Then for the task of including tags around speakers, I just wrote a small code which searches for – symbols at the beginning of metadata sentence (By modifying the existing preprocess.py file) and inserts and tags alternatively. I hadn’t even done any testing with this file and as you’ll see later, this was entirely incorrect. Towards the end of the week, I ran some files through the existing pipeline and tried out the sentence splitter, pragmatic segmenter,

## Week 4: June 17 to June 24

As I was going through the code for speaker turn tags, I realized that there might be a problem with what I interpreted as the beginning of the line. Did it mean every dash at the beginning of a new line in the metadata, or every dash preceded by a punctuation mark?
For example,

XXXX1|Does this sentence constitute as a 

XXXX2| - New sentence? - And does this

XXXX3| Second sentence?

XXXX4| -And check for this new sentence.

I wrote to my mentor Peter about this. Neither of us is fluent in Russian, so he asked me to collect a couple of examples like this, along with their video links, so that a native speaker may verify my suspicions. I did, and he forwarded my mail to the speaker. 

However, I had written down the seconds incorrectly in my video links. So, naturally the text and video snippets weren’t matching up.
Since I couldn’t understand what I did wrong while writing the seconds, I went to some English subtitles and videos and experimented with them. I was copying the HMS (Hours, minutes, seconds) number into the video, instead of just seconds. I corrected this and sent the correct links.

Of course, only the sentence ending punctuation marks were the source of doubt (period, exclamation marks, question mark, etc.) and not all punctuation marks, such as quotations, something like: 

XXXX1| - “Is this change of speaker”.

And what about a case like this:

XXXX1| This is speaker 1. 

XXXX2| - Hello... //Speaker 2 

XXXX3| - Does this dash mean again speaker 1 since it’s after a period mark?

Still I included examples of this type just to be sure.

I got the response which confirmed that speaker change occurred whenever there was a dash after a sentence ending punctuation mark. This is easier than the previous method to denote speaker change and I’d be done with this code within a day, and you can find it on the GitHub repo for this project.

While going through the examples in English to check the corresponding subtitles to a video snippet, I noticed there were parts where some words spoken weren’t included in the text. I messaged Peter about this potential problem. I though that this would lead to an increasing mismatch as we go down the video. It turns out that this is actually a quite common problem with subtitles and their limit on the number of words allowed per minute. It doesn’t lead to any problem between words and corresponding text.

That's all for now. Stay tuned for more update about the project!
