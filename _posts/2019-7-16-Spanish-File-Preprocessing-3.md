---
layout: post
title: Spanish File pre-processing - Part 3!
---

Even after successfully indicating a change of speaker in the text, there was no proper information about the speaker nonetheless. Sometime earlier, Peter had collected data in English about which words were occurring at the beginning of the sentence, and some other cases, then calculated their frequencies and then classified them. This was a helpful strategy for English files, and we decided to go with it in the Spanish files as well.

There are different conventions for non-spoken text in Spanish files, such as [APLAUSO] and sometimes words are in (ROUND) brackets. We want to convert this information to XML metadata.  For instance, audio descriptions are often found in round or square brackets such as [APLAUSO]. 

However, not everything in such brackets is a description of the audio signal. But first, we have to find these words and see what they mean.

My task here was to find three different types of words in the text and calculate their frequencies:

1. Potential Speakers - Words at the beginning of the sentence which can then be classified as a speaker or not by the native speakers.

2. Words in round brackets.

3. Words in square brackets.
