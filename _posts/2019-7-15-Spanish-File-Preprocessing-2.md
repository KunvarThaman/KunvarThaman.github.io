---
layout: post
title: Spanish File pre-processing - Part 2!
---

Similar to the Russian files, we wish to determine when the speaker changes and who the speaker change was from in the text files.

It was decided to find the instances where there were, say, <font color = “#00ff00”> and other similar font tags. Earlier, it was believed that these tags indicated the speaker change. After going through several files and verifying with another Red Hen, Javier, it was found that in some cases, the font tags, and in others, dashes at the beginning of the sentence were being used to indicate the speaker change.

To have a better representation, it was decided to use the <turn> and </turn> tags to indicate the speaker change instead. 

We have to first find all the files containing the font tags, although not immediately required by the current task.

$grep -iRl “<font” ./

Output these file names into another text file for cleaner results and better readability:

$grep -iRl “<font” ./ > files_with_font_tag.txt

For finding the potential speakers, we first run the file(s) through the preprocess.py.

python preprocess.py -inf <input-file-name> -t <0 or 1>
Where -t [ 1 or 0] can be used to toggle the relative time occurrence of each block of words.

For example: 

python preprocess.py -inf 2018-07-13_0730_ES_Antena-3_El_hormiguero.txt -t 0 > output_messy.txt

We want to determine which words form the starting of a sentence. Hence we would look for the words following a stop-punctuation mark, such as . or ! or ?. However, when we look at output_messy.txt, we can see that there are <font> tags to indicate the speaker change, sometimes at the beginning of a new line such as :

<font color="#00ff00">que pueden matizar posiciones.</font> <font color="#00ff00">Pero hay que recordar</font> <font color="#00ff00">las verdades del barquero.

They’re going to interfere when we are collecting the words which are at the beginning of the sentence for the frequency list. Here, the output contains the font tags after the period, so, we would have to skip over them to get to the first word. I decided to create a temporary file where we would store the text file after removing the font tags. This would make finding the words at the beginning of the sentence straightforward.

So, we create a new output file output_clean.txt which removes all the font tags as we don’t need them for the frequency list and important information isn’t lost.

Looking carefully, I found that the number of font tags being used wasn’t large. So, I manually searched for them and created a list as:

delete_list = ['</font>','<font color="#00ffff">','<font color="#00ff00">', '<font color="#ffff00">']

The rest of the process is quite straightforward as:

We open the files as infile and outfile:

fin = open(infile)
fout = open(outfile, "w+")

We go line by line in the infile and see if any of the words from the delete_list occur. If it does, we replace it with whitespace in the outfile.

for line in fin:
    for word in delete_list:
        line = line.replace(word, "")
    fout.write(line)

This does the job and we close the files

fin.close()
fout.close()

Looking at the output_clean.txt, we can see that there are no instances of font tags occurring now. We now apply the code for finding the first words of each sentence and create a frequency list, as explained in the next blog post.
