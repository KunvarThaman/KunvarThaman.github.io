---
layout: post
title: Spanish File pre-processing!
---

This post will cover the steps for pre-processing the Spanish files and writing their tools. 

An important aspect of Spanish files is that they come in three different types: European Spanish, American Spanish, and Mexican Spanish.

American Spanish comes in all upper case.
Note that if the text is all uppercase, all lowercase, badly or inconsistently capitalized, then there will be a negative effect on the performance of most annotators. The Spanish dataset that I’m working with contains 3 types of Spanish: European Spanish, American Spanish, and the Mexican Spanish. The Mexican Spanish has the language code ES-MX, while the other two are clubbed under SPA. 

Now, though the European Spanish text is fine, the American Spanish comes all capitalized, which causes a lot of performance loss. After taking some sample files and running them through the Pragmatic Segmenter, first, as they come, all uppercase, and then once after lowercasing everything, we find that though lowercasing will inevitably lead to loss of some information, it also leads to a better overall performance than an all uppercase format.

I was directed by my mentor to study the Stanford CoreNLP page and they have two main strategies for dealing with this type of text:

The first one is using TrueCaseAnnotator. The other strategy is to use models which specifically work better for ill-cased text.

We’ll discuss the TrueCaseAnnotator (TCA) first.

Our first attempt was to convert the files to all lowercase. Although lowercasing is quite common, it will inevitably affect the performance. This idea is called case-folding. A word ‘Auction’ will match with ‘auction’ if this strategy is used. This will lead to problems such as matching the company ‘Apple’ with the fruit ‘apple’. 

Then perhaps, instead of making all the tokens lowercase, we can lowercase only some of the tokens. A machine learning sequence model can be trained which makes the decision of when to case fold. This strategy is called true-casing, of which TCA makes use of. Here, the idea is to lowercase only certain words such as those at the beginning of the sentence. We would have to keep in mind not to lowercase abbreviations and initials. The idea was suggested in the true casing paper: https://www.cs.cmu.edu/~llita/papers/lita.truecasing-acl2003.pdf 

TCA attempts to recognize the true case of the tokens: how it would have been in well edited text. It also stores where the information was lost, for example, an all uppercase text.

TCA is implemented with a discriminative model using the CRF sequence tagger. CRF refers to a Conditional Random Field sequence labelling model. Given the previous labels, feature extractors, and the weights, it predicts the current label. 

As the text is processed, a category label is given to each word, such as INIT_LOWER, INIT_UPPER, etc.,  and this information is stored in TrueCaseAnnotation. The token text adjusted to represent the true case of the text is then created and stored in TrueCaseTextAnnotation. 

The second strategy is to use caseless models. The Stanford NLP team has made slightly different CoreNLP models for the tagger, parser, and the NER that ignore capitalization. However, they only have trained model for English, but suggest that the process for other languages is straightforward and can be found here: https://stanfordnlp.github.io/CoreNLP/caseless.html 
