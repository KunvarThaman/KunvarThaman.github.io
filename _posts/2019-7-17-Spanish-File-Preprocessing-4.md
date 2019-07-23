---
layout: post
title: Spanish File pre-processing - Part 4!
---

In this post, we will discuss the three task of finding potential speakers and creating all the frequency lists.

First of all, let’s import the necessary libraries:

import re, os
from collections import Counter
import argparse

We would want to give input of the file name later on, so, we define args here:

parser = argparse.ArgumentParser()
parser.add_argument("-inf", "--input_file", type=str, help="Name of\input file")
args = parser.parse_args()

Here we may also directly give the output_clean.txt file as input as it already has the font tags removed.

Since we are going to match the words at the beginning of the sentence i.e words after a stop mark such as !, ?, and ., we capture them by the following regex expression:

reg_matches = re.compile(r'(?:^|(?:\.\s)|(?:\.)|(?:\.\s\s))(\w+)')

Note: I found once instance where this regex was giving an incorrect result. Hence consider it temporary as I’ll update it again.

Now, we find the matches and write them in a new file, containing solely of the words at the beginning of the sentence.

with open(args.input_file, 'r') as rf, open('freq_square.txt', 'a') as wf:
    content = rf.read()
    matches = reg_matches.findall(content)
    for match in matches:
        wf.write(match + '\n')

Using Counter from the collections module, we can easily get the frequencies of the words at the beginning of all sentences in any file.

with open('freq_square.txt') as f:
    wordcount = Counter(f.read().split())
    print(wordcount)

Since we want the frequency data of all the words in the corpus, we run the script over the entire directory and generate the complete list which is then converted to a .csv format.
For demonstration purposes, it’s easier to run it on a single file. The results look something like:
:
Counter({'No': 68, 'Y': 54, 'Es': 52, 'La': 42, 'Pero': 40, 'Lo': 34, 'El': 30, 'Si': 30, 'En': 28, '000': 24, 'Hay': 18, 'Me': 18, 'Una': 16, 'Vamos': 14, 'Como': 14, 'Por': 14, 'Que': 12, 'Ha': 12, 'Se': 12, 'Al': 12, 'Tenemos': 10, 'Ahora': 10, 'Los': 10, 'Sobre': 10, 'De': 10, 'Con': 10, 'Han': 8, 'Aqu': 8, 'Nos': 8, 'Cuando': 8, 'Buenos': 8, 'Creo': 8, 'Para': 8, 'Eso': 8, 'Porque': 8, 'Yo': 8, 'A': 8, 'Tambi': 8, 'Algunos': 6, 'Todo': 6, 'Nosotros': 6, 'Genera': 6, 'Luego': 6, 'Claro': 6, 'Esta': 6, 'Estoy': 6, 'Voy': 6, 'Esa': 6, 'Esto': 6, 'Las': 6, 'Hasta': 6, 'Realmente': 4, 'Ra': 4, 'Tienen': 4, 'Habr': 4, 'Insisto': 4})

We compile them now into an excel sheet for easy readability using the text_to_csv.py.

The tasks of finding the frequency of words in the round and square brackets are quite straightforward after the previous task. There’s no need to use the output file with the font tags removed.
We can directly use the regex of :

[. * ? \] and ( . * ? \)

for the square and round bracketed words respectively.
The rest of the process is the same as the first one.

## Note: 

it would be wrong to treat every word at the beginning of a line followed by a colon as metainformation. These frequency lists are being examined by native speakers who will then classify them into categories, including speaker or non-speaker information.
