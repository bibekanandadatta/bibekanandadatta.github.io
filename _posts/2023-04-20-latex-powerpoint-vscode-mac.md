---
layout: post
title: LaTeX in MS PowerPoint and VS Code on macOS
date: 2023-04-20 11:59:00-0400
description: A tutorial to use LaTeX with VS Code and PowerPoint
tags: LaTeX PowerPoint LaTeXiT VS-Code macOS
categories: tutorial
giscus_comments: false
related_posts: true
toc:
  sidebar: left
---

My presentations often include a fair share of math equations and videos/ animation. Although making a presentation in Beamer is always an option however as of now, embedding animation or video in a Beamer-generated PDF isn't very intuitive or natively supported. So, I have always opted for PowerPoint because of the ease of use. While equation editor in MS Word and PowerPoint has improved drastically over the last few years, it's still nowhere near what LaTeX can produce. Additionally, I already have equations written in LaTeX and I use Overleaf these days for LaTeX.


## LaTeXiT and PowerPoint

After I got the 2022 M2 Macbook Air, I was looking for a way to integrate LaTeX into MS PowerPoint. Previously, I had the full LaTeX distribution installed via MacTeX and I used the IguanaTex plugin to generate the LaTeX equation within the PowerPoint. But this time around, I was reluctant to install the full MacTeX distribution as it would occupy 8 GB+ space on my SSD. MacTeX is essentially TexLive distribution specific to macOS. On Windows, the popular Tex distribution is MiKTeX. Anyway, also to use IguanaTex on Apple Silicon processors, MS PowerPoint needs to be run with the "Open with Rosetta" option. Rosetta is a software that emulates Intel x86 architecture for Apple's ARM silicon processor, but that comes with a performance penalty. I decided not to opt in for that either.



Considering the situations at hand, my new workflow for having LaTeX equations in the PowerPoint slides requires the installation of BasicTeX, Ghostscript, and LaTeXiT. BasicTeX is a lightweight TeX distribution (only a few hundred megabytes) of MacTeX, Ghostscript is a rendering package of PostScript and PDF, and LaTeXiT is a very lightweight equation editor. Download basictex and ghostscript [from here](https://www.tug.org/mactex/morepackages.html). Install in this order; first BasicTeX and then Ghostscript. Both of these come as .pkg files, so the installation procedure is straightforward and no customization is necessary. Now [download and install LaTeXIt from here](https://www.chachatelier.fr/latexit). The installation process is trivial. Open LaTeXiT and then from the Menu bar on the top of the screen, go to Settings. Make sure your export format is "PDF Vector Output". You can also export the equations in .png, .jpg, .tiff, .eps, and .svg format, but I found PDF to be the most compatible format which you can easily edit later by dragging and dropping to LaTeXiT.


You can now start typing equations and if you click on the LaTeXiT icon, it will produce the beautifully crafted LaTeX formula in the preview box on the top. You can drag it to the Desktop Window and it will be saved as a PDF file. You can also directly drag it to a PowerPoint slide. It will embed there as an image. If you'd like to edit the equation later, you can drag that image to the preview window of LaTeXiT, it will generate the .tex code for you which you can edit and repeat the process of drag and embed. There are two things I should mention about this workflow:

  - BasicTeX does not have all the packages you may need and manual installation of the packages might be needed. We will see how to do that.
  - I would have loved it even more if LaTeXiT had a PowerPoint plug-in like IguanaTeX. I wish the developer steps forward at some point.

### LaTeXiT Settings (optional)

In the typesettings tab, you can create your preamble. This is what my current preamble looks like:

``` latex
\documentclass[12pt]{article}

\usepackage[utf8]{inputenc}

\usepackage{amsfonts,amssymb,amsmath,amsthm,dsfont,mathtools,mathbbol,upgreek}
\usepackage[usenames]{color}
\usepackage{enumitem}
\usepackage[T1]{fontenc}

\newcommand{\dC}{$^{\circ}$C}
\DeclareMathOperator{\R}{\mathrm{R}}
\DeclareMathOperator{\T}{{\top}}
\newcommand{\vect}[1]{\mathbf{#1}}
\newcommand{\mat}[1]{\mathrm{#1}}
\DeclareMathOperator{\tr}{tr}
\DeclareMathOperator{\sym}{sym}
\DeclareMathOperator{\skw}{skw}
\DeclareMathOperator{\divg}{div}
\DeclareMathOperator{\grad}{grad}
\DeclareMathOperator{\curl}{curl}
\DeclareMathOperator{\sgn}{sign}
```


LaTeX packages, dsfont and mathtools, are not part of the basictex distribution. So, I had to install them manually. If you'd like to see what LaTeX packages are installed in your distribution, type:
```
tlmgr list --only-installed
```

`tlmgr` is the TexLive distribution manager. You can search for TexLive (tlmgr) documentation to learn more commands. To install the additional packages I have here, open the terminal and type the following:

```
sudo tlmgr install doublestroke mathtools
```

The LaTeX package, `dsfont`, is distributed as doublestroke on the repository. You can follow the same procedure to install any LaTeX package that is hosted on the CTAN repository. We will see a few more examples later.

## Integrating LaTeX with VS Code

Since I already installed BasicTeX, I decided to give local LaTeX compilation a shot but without using any additional software like TexShop, TexWorks, TexMaker, etc. Since I am a regular user of Overleaf, this is not a priority to me but I thought it would be nice to have the option, just in case. Visual Studio Code is my favorite text editor for a while, so I decided to check out the options to integrate LaTeX compilation to VS Code. If you haven't used VS Code before, you should try it out: https://code.visualstudio.com. VS Code is probably the most popular code/text editor now. Anyway, on my VS code, I installed the extension: LaTeX Workshop to manage LaTeX linking and compilation. Before configuring LaTeX Workshop for compilation, we need to install a few LaTeX packages: `chktex`, `synctex`, `latexmk`, `texcount`, `latexindent`. The installation process is the same as before; open the terminal app and type:

```
sudo tlmgr install chktex synctex latexmk texcount latexindent
```
Now we need to get the PATH for these packages to be included in the LaTeX Workshop settings inside VS Code. To get the path for each package, on the terminal, type:

```
which chktex
```

This will return the path for chktex installation. You can repeat the same command for other packages as well. Now, let's open the VS code, and from the top Menu bar, go to Code > Settings > Extension > LaTeX Workshop. From the top search bar menu, search "latex-workshop path". This will result in some options that require the path variable. I had to enter the path for chktex, synctex, texcount, and latexindent. By default, VS Code uses latexmk for compilation, so does not require entering the path in there. I guess, it finds automatically from the system if installed. You can also customize other options from the LaTeX workshop extension, such as cleaning additional files after compilation and choice of PDF viewer, etc. Once all the setup is done, you can now start writing in LaTeX and compile it.

## Final Words

While this method is capable of compiling LaTeX files to PDF via pdflatex, the lack of packages in the BasicTeX installation makes it harder to compile a more realistic LaTeX file. You may have to install LaTeX packages manually. MikTeX on Windows has an on-the-fly package installation feature which is awesome, but I have not found a way to do it with MacTeX or TexLive. As of now, the apparent remedy is to install the complete MacTeX distribution. MikTeX is also now available on macOS; so installing MikTeX instead of BasicTeX might be helpful in that case.