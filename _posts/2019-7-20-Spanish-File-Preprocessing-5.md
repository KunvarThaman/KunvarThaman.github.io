---
layout: post
title: Spanish File pre-processing - Part 5!
---

Spanish Pre-processing part 5!

The speaker change occurs in two different ways, as verified by a native speaker after looking at several instances and then comparing the text with the audio in the respective clips.

The first way is by the font tags which have some color associated with them according to the speaker. It’s ensured that the colors never overlap for consecutive speaker changes. 

The other way, as was in Russian files, is by dashes at the beginning of the sentence.

Our goal here is to introduce turn tags wherever such dashes occur, and introduce turn tags in place of font tags whenever they occur. In the latter, we want to retain the color information. Hence, the new tag would look something like:

\’<turn speaker = “color_code” >’

We call the speaker_dash.py which has a function called speaker_change which takes in the inputs of the filename, list from which we want matches, and the string to replace it with.

For the list from which we want matches:

 dashes = 

re.compile(('(\.\s\s-)|(\.\s-)|(\.-)|(\?-)|(\?\s-)|(\?\s\s-)|(!-)|(!\s-)|(!\s\s-)|(S____(\d+)\s-)|(S____(\d+)\s\s-)| (S____(\d+)-)')

And we would replace it with the string: ‘</turn> - <turn>’ so that every dash will be succeeded by a turn tag that ends just before the next dash. This doesn’t cause any problem with the first dash as the speaker#1 doesn’t have any dash since there’s no speaker ‘change’ at the beginning of the file.

Next, we replace the font tags with the turn tags, retaining the color information. 

We create lists of the font tags and then for each of those font tags, we go line by line in the input file and whenever there’s a match with any word in any of the delete lists, it’s replaced by the turn tag containing the corresponding color change. It could’ve been more complicated had there been lots of different colors being used. 

fin = open(infile) 

fout = open(outfile, "w+")

for line in fin: 
     for word in delete_list:
           line = line.replace(word, "</turn>") 
     fout.write(line)

As each file is processed this way, we copy the resulting text to another directory under the file's original name. Thus, we have updated the files with the turn tags and retained the original files as well.
