---
title: Misadventures in Scanning
date: 2024-09-25T00:32:29.722Z
categories: []
image:
  path: assets/computerlib/lib_preview.png
  alt: Computer Lib Snippet
---

I've had a bunch of images of raw scans of a book called Computer Lib/Dream Machines sitting around since 2022, and I wanted to mention it in a blog post I'm working on. However, this book is large (10x14" according to Amazon), and while the scans online are acceptable quality, I thought a book so visually interesting and important deserved a super-high-resolution scan. So here is the series of false starts I went through to prepare them for distribution.

[Computer Lib Lossless.pdf](/assets/computerlib/computerlib.pdf)

[Dream Machines Losslesss.pdf](/assets/computerlib/dream_machines.pdf)

[Combined.pdf](/assets/computerlib/computerlib_DM.pdf)

The combined PDF is compressed, but still looks great, while being a lot more managable size. You still get more ~texture~ with the originals through.

## Scanner

I scanned this book using two different scanners. The first was a Canon scanner (I sadly don't remember the model) that was in the basement of the Architecture library, using the factory software. What a mistake! It offers no lossless compession, so you get a choice of 100 MB PNGs or 500 MB TIFFs (!) with no auto-cropping or straightening. The PNGs it produces are also split across ~2500 IDAT chunks, while most PNGs have a few dozen, which makes most viewers really struggle to open them. Additionally, it outputs scans with a white border for some reason, which totally breaks most automated cropping/deskewing tools, and the PNG outputs inexplicably have an alpha channel which inflates the file size and messes with some conversion tools. After a while, I realized how bad this was, and switched to a ScannX Book ScanCenter at another library. This output similar quality scans, with much more reasonable file sizes, AND has auto cropping/deskw. Unlike most hardware, the software that comes packed with scanners is usually the best at the first processing steps, so try to use it if available. I also scanned every other page and did some upside down, which I would not reccomend. While slightly more convenient when moving a book around, having to go through and renumber everything is NOT it.

## Image file Processing
If your images are in a weird format, or you need a specific format for a certain tool, ImageMagick is the industry standard. While it has its quirks, and speed isn't great, it will input or output pretty much anything you throw at it. [Running jobs in parallel](https://stackoverflow.com/a/26169406) can help. Unlike JPEGs, PNGs and TIFFs offer a lot of different ways data can be structured internally, which can lead to some _weird_ images. [OxiPNG](https://github.com/shssoichiro/oxipng) is a great PNG optimizer/normalizer which both losslessly reduced the file sizes, and made the PNGs output by the Canon scanner actually usable.

## Scan processing
Scan processing includes cropping, deskew (which is really straightening), and page splitting if you scanned the folio spread instead of single pages. The standard tool for this seems to be ScanTailor, but its _messy_. The original project is now abandonware, and there are various ports like ScanTailor-X and ScanTailor Advnaced. It refused to open any of my output files, even after reencoding them. I wish I could've gotten it working, as its users seem happy with it, but alas.

Another tool used is called `unpaper`, an old-school Unix utility. Not only does it use the archaic PBM format, but its deskew straight up did not work for me. Worth a shot if other tools don't work, but not a first choice.

What I ended up doing is just importing all the scan images into Apple Photos, then cropping and rotating them there. This is not ideal as the black background makes cropping difficult, but it DOES offer lossless export in the file menu throught PNGs.

## Image to PDF conversion
* `img2pdf` is a popular choice, although it ran out of storage space on my machine
* `magick -debug All *.png merge.pdf` gave more interesting debug output and was a little more space efficient.
* JBIG2 compression can work wonders for PDFs (like 25x storage reduction), as long as they're mostly text, and you don't care about a slight quality downgrade from converting to 1bpp. It has a bit of a bad rep from some buggy implementations in scanners, but as long as you set the threshold high enough (I use 0.85), it works great. Unfortunately [the command line tools](https://github.com/agl/jbig2enc) are...archaic. Use the [script from a PR](https://github.com/agl/jbig2enc/pull/92) to generate a PDF if you don't have Python 2.
* As I cropped the photos by hand, they were all slightly different sizes, which made for an annoying PDF to read. What I did is open them all in Preview, then printed the entire thing to PDF. Make sure you have a borderless paper size selected, and check 'fill entire page'.

## Final file preperation
While many of these tools have been old, quirky, and/or slow, `ocrmypdf` is none of those things, and is an amazing option for general PDF processing. I used it for OCR and final PDF generation. Some notes on its use:
* Unfortunately it only accepts a single image, or a PDF, so a series of images will need to be converted
* The deskew straight up didn't work for me :(
* If you have a high resolution file with small text that isn't OCRing well, add `--no-tesseract-downsample-large-images --tesseract-timeout 6000`. It was automatically scaling the image, causing the text to be unreadable, and making the OCR fail.
* This does handle JBIG2 PDFs, but I can't quite figure out the heuristics it uses to convert your PDF into the format, so I just did the conversion myself.

Happy scanning!