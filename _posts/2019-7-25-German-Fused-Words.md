---
layout: post
title: German contracted words problem explained
---

In German, it is common to combine two (or more) words so as to be more efficient while writing or speaking. A few examples are :

zu + der = Zur,

an + dem = am,

in + dem = im,

Bestsellung = order, and Bestelldatum = order date.

Such fused words are called contractions.

During parsing, when these contractions are passed through UDPipe, it outputs not only the contraction but the individual words as well.

We haven’t come up with a solution yet and are working on it. I found two libraries that might be able to resolve this: CharSplit, and SECOS, both of which claim to output all possible splits, ranked by their score. I’ll be running some tests and discussing this with my mentors in the next few days, and we’ll see whether this might be able to solve the problem. Let’s hope for the best!
