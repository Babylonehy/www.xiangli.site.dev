---
layout: article
title:  "Latex for Ubuntu with Vscode"
date:  2020-03-05
Author: Xiang Li
tags: [Ubuntu, Latex,Vscode]
comments: true
toc: true
---

# [Install](https://blog.csdn.net/wuguangbin1230/article/details/78017013) 

[Simple Version](https://zhuanlan.zhihu.com/p/65931654)

```
sudo apt-get install texlive-full
```
<!--more-->
## Vscode Setting Json
搜索latex-workshop.latex.recipes
点击Edit in settings.json
```
    "latex-workshop.latex.recipes": [
        {
            "name": "pdflatex",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "pdflatex -> bibtex -> pdflatex*2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "xelatex->bibtex->exlatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
```