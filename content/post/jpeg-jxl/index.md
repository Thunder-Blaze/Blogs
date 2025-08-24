---
title: JPEG Should Die, This Is Why
description: Why its time for JPEG to go.
date: 2025-08-25 00:00:00+0000
slug: jpeg-jxl
categories: 
    - tech
tags:
    - tech
math: true
---


# 1	My Experience with JPEG
## 1.1	Introduction
JPEG, the image encoding format that has been the standard for a very long time, has its own shortcomings, including the lack of a lossless format for a long time, which was **fixed** in J2K, but not along with all its other shortcomings. Neither was J2K widely adopted.

Behold - JXL, the format overcomes the shortcomings of its predecessor and compresses even the regrets of the developers.

Hatred and Biases aside, for a long time in my life, I've lived unaware of the problems that would ruin a good chunk of my "work time", one of them being: JPEG. This is not a blog; this is a full log of how I ended up hating an image encoding format because of **my own mistake**.

## 1.2	The Error
I was working on one of my projects that requires a JPEG image to be loaded in the memory and then saved again *after some processing, which might include shifting pixels*. The testing went fine - due to a major flaw in my testing unit: I was supposed to shuffle the pixels, then save the file, **then** unshuffle the **saved** image. But I accidentally shuffled and unshuffled in the image in **memory** itself. The results were fine - and oh boy was I so naive.

## 1.3	The Panic
When the data(shuffled image) was received by the front-end, and when I tried to un-shuffle the pixels **on the front-end**, artifacts showed up. And the thing to notice was that the artifacts were **very** noticeable. 

I will demonstrate the intensity of artifacts by shuffling, then saving, and then finally unshuffling with a grid size of 10x10.

### 1.3.1	Input Image

![[input.jpeg]]

### 1.3.2	Output Image
![[output.jpeg]]


As you can see, there are some grid lines in the output image, even when the settings were set to `quality = 100` for this task. I was aware of the lossy nature of JPEG, but the lines could not be there because of that(**RIGHT??**). This made me doubt JavaScript for its number precision many times during the process of fixing this issue. Causing me to waste upwards of 5-6 hours on this.

### 1.3.3	The Observation
I went all in, went onto re-reading my implementation of shuffling in the back-end, to checking JavaScript(the front-end) for its precision for numerical data types: Result? - **NOTHING**

No clues pointed to the **real** culprit out in the wild: JPEG. I gave up, was going to sleep, but I was still thinking about the cause of this issue. I opened my phone and searched about JPEG, and here was the big reveal: [**Quantization**](https://en.wikipedia.org/wiki/Quantization_(image_processing)#Frequency_quantization_for_image_compression).

I will not dive deep into the algorithm of [how JPEG compresses data](https://cgjennings.ca/articles/jpeg-compression/), but it's certainly not "good" for the image quality. Neither any other compression methods that use the same DCT and Quantization approach will ever be recommended or used by me ever again.

What I learned was that JPEG does Frequency quantization. I know this is a lot of technical jargon, but bear with me for a minute; it will all make sense. **Frequency-based quantization** reduces a rapidly varying array(consider this to be an array of brightness of pixels) into a smooth array(which humans would not be able to distinguish since the changes are on a pixel level). What this essentially does is that it makes compressing the image data easier.

This looks something like:
$$
\begin{bmatrix}
-415 & -33 & -58 & 35 & 58 & -51 & -15 & -12\\
-30  & -24 & -10 & 16 & 24 &  -6 &  -7 &  -5\\
-55  & -17 & -29 & 17 & 19 & -10 &   4 &   1\\
 35  & -23 &   9 & -3 & -1 &   5 &   1 &   1\\
-58  &  -9 &  -2 & 15 &  2 &  -1 &  -3 &   1\\
-39  &  12 &   7 & -3 & -1 &   2 &   4 &  -2\\
  5  &  -2 &  -2 &  1 & -1 &  -1 &   1 &   2\\
 -1  &  -1 &  -1 & -2 & -3 &  -1 &  -2 &  -3
\end{bmatrix}
\;\; \longrightarrow \;\;
\begin{bmatrix}
-26 & -3 & -6 & 2 & 2 & -1 & 0 & 0\\
 -2 & -2 & -1 & 1 & 1 &  0 & 0 & 0\\
 -4 & -1 & -2 & 1 & 0 &  0 & 0 & 0\\
  3 & -1 &  0 & 0 & 0 &  0 & 0 & 0\\
 -3 &  0 &  0 & 0 & 0 &  0 & 0 & 0\\
 -2 &  0 &  0 & 0 & 0 &  0 & 0 & 0\\
  0 &  0 &  0 & 0 & 0 &  0 & 0 & 0\\
  0 &  0 &  0 & 0 & 0 &  0 & 0 & 0
\end{bmatrix}

$$

Now what happens is that when I shuffle the image and save it as a JPEG then the new (scrambled) image is saved with this DCT + Quantization applied on top of it which **compresses the edges of the tiles with respect to their neighbors**, this affects edges the most since they rapidly vary because the image is no longer continuous. This causes a massive loss in data, where the tiles would have to be rejoined after unscrambling the image.

# 2	In search of alternatives
After finding the issue in JPEG itself, I searched for alternatives and came across various encoding formats. **AVIF** and **JXL** were the ones that caught my attention the most.
I reached these 2 formats as alternatives after I read a [blog comparing different image codecs](https://giannirosato.com/blog/post/image-comparison/) and reached the conclusion that AVIF generally gives better compression at lower quality, but as the quality increases, JXL starts to dominate the game.

> The set of Pareto-optimal methods is called the “[Pareto front](https://en.wikipedia.org/wiki/Pareto_front).” It can be visualized by putting the different methods on a chart that shows both dimensions - encode speed and compression density. Instead of looking at a single image, which may not be representative, we look at a set of images and look at the average speed and compression density for each encoder and effort setting. For example, for [this set of test images](https://imagecompression.info/test_images/), the chart looks like this:

![[Pasted image 20250824224512.png]]

> The vertical axis shows the encode speed, in megapixels per second. It’s a logarithmic scale since it has to cover a broad range of speeds, from less than one megapixel per second to hundreds of megapixels per second. The horizontal axis shows the average bits per pixel for the compressed image (uncompressed 8-bit RGB is 24 bits per pixel).

From the Blog - [JPEG-XL and the Pareto Front](https://cloudinary.com/blog/jpeg-xl-and-the-pareto-front) 

> TLDR: Higher means faster, more to the left means better compression.

Results are on the table, the cat is out of the bag, and I will never use JPEG on my own unless I absolutely need it from now on. 

ALL HAIL JXL!

# 3	WHY!!! and Conclusion
> ALL HAIL JXL - famous last words from previous section

The [news](https://news.ycombinator.com/item?id=33399940) is very old, unlike my short-lived belief that I'd be able to finally switch to JXL. There is no official JXL support on Chrome and many browsers after Google decided to deprecate JXL in 2022. They decided to drop the support for JXL for some reason, like "[adding a single frame decode is a minor overhead](https://news.ycombinator.com/item?id=33401474)". Any news of [them bringing it back](https://www.reddit.com/r/jpegxl/comments/15kpacf/google_may_reconsider_jpegxl_image_support_within/) is just a rumor for now. One of the weird things is that out of all the browsers in the world, even SAFARI supports JXL.

Here, the struggle of making JXL work in a browser ends. I ended up converting all my data to WEBP/PNG for lossy issues and bore with the bitter transcoding times for dynamic images.

I guess we just can't have nice things... But I'll keep using JXL as my main image encoding format, at least locally from now. on
