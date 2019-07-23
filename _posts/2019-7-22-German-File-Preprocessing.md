

At this point in the project, we move on to the German files.

German files already contain turn tags. What we want is to add an attribute for identification. We’d have to replace the turn tags with new ones that contain the color information as well.

This will be slightly more complicated as we’ll have to ensure that no two consecutive speakers have the same color code. In addition to that, if three speakers are present, the color code for Speaker 1 and Speaker 3 must be distinct and so on.

Another problem to be fixed with the German files is that there are words with fused forms such as „am“ (short for „an dem“) and „im“ (short for „in dem“). UDPipe outputs the word "am" but also the separate forms "an dem" when parsing German text. We have to come up with a strategy to deal with such cases.

That’s all for now. Stay tuned for the progress with my project!
