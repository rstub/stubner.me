---
title: Using qpdfview with AUCTeX
author: ''
date: '2019-06-05'
slug: using-qpdfview-with-auctex
categories: []
tags:
  - emacs
  - LaTeX
  - AUCTeX
image:
  caption: ''
  focal_point: ''
---

I recently set up a new computer using [Debian Buster](https://www.debian.org/releases/buster/) with [LXQt](https://lxqt.org/) as desktop environment, which uses [qpdfview](https://launchpad.net/qpdfview) as default PDF viewer. 
Obviously I wanted to make use of it when writing [LaTeX](https://www.latex-project.org/) documents in [Emacs](https://www.gnu.org/software/emacs/) using [AUCTeX](https://www.gnu.org/software/auctex/).
[Mathieu Morey](https://twitter.com/moreymat) once wrote an explanation how to configure an alternative PDF viewer, which is nowadays [archived](https://web.archive.org/web/20170528094903/http://mathieu.3maisons.org:80/wordpress/how-to-configure-emacs-and-auctex-to-work-with-a-pdf-viewer). 
However, this explanation uses Emacs' inbuilt configuration buffers, while I prefer adding simple Emacs Lisp to the initialisation file.

First one has to define qpdfviewer as a potential viewer for AUCTeX:

```
(setq TeX-view-program-list
    '(("qpdfview"
          ("qpdfview --unique %o"
              (mode-io-correlate "#src:%b:%n:0"))
          "qpdfview")))
```
The `--unique`  argument ensures that an already open file is re-opened, while `#src:%b:%n:0` can be used for [forward search](https://www.gnu.org/software/auctex/manual/auctex/I_002fO-Correlation.html) from the source to the PDF file.

Next one uses qpdfviewer in the list of available viewers for PDF ouput:

```
(setq TeX-view-program-selection
    '(((output-dvi has-no-display-manager) "dvi2tty")
         ((output-dvi style-pstricks) "dvips and gv")
         (output-dvi "xdvi")
         (output-pdf "qpdfview")
         (output-html "xdg-open")))
```

## Setting up SyncTeX

In order to use backward search from the PDF to the source file, one has to use *correlation mode*.
This mode can be toggled using `C-c C-t C-s` or enabled on a global

```
(setq TeX-source-correlate-mode t)
```

or buffer local basis

```
% Local Variables:
% TeX-source-correlate-mode: t
% End:
```

Finally one has to set the "Source Editor" in qpdfviewer under Edit -> Settings -> Behavior to `emacsclient +%2:%3 "%1`, c.f. https://answers.launchpad.net/qpdfview/+question/272672.

