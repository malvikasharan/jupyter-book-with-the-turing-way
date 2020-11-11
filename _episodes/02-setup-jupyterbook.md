---
title: "Creating a Jupyter Book with The Turing Way"
teaching: 30
exercises: 15
questions:
- "How to set up the repository, add a chapter, create a table of content, and build the book?"
objectives:
- "Explain what files exist in the repository that we will use for the hands on session in this module (if you haven't already, please download the data required for thie tutorial described in the module 1)"
- "Explore the important components for creating Jupyter Book in this tutorial"
- "Build the first minimal version of the Jupyter Book locally using example files from _The Turing Way_"
keypoints:
- "- Jupyter Book uses markdown files to create chapters of an online book. In this tutorial, these files are placed in a directory called `book`."
- "All the images used in the file should also be stored in `book` (we saved them under the folder `figures` in this tutorial)."
- "References used in the book should be stored in a `references.bib` file, which should also be stored in `book`"
- "A `_toc.yml` should be generated inside the folder `book` to structure the jupyter Book (toc is abbreviation for table of content)"
- "With these files in place, you can create a minimal book using the command `$ jupyter-book build {path-to-book}`"
---

## Introduction to Jupyter Book

Welcome! In this Jupyter Notebook we will introduce the basic commands to generate your first Jupyter Book.

In the previous module, we briefly looked into the awesome and very detailed [documentation](https://jupyterbook.org/intro.html) of Jupyter-Book, and its [GitHub repository](https://github.com/executablebooks/jupyter-book).

Jupyter Book has a [command-line interface](https://jupyterbook.org/reference/cli.html), so in this tutorial we will show you how to build your book using shell commands.

If you prefer using Jupyter Notebbok, you can use iPython's special syntax that lets you execute shell commands from a Jupyter Notebook. You can run all the commands used in this tutorial by prefixing the shell command with `!` in each cell.

> ## TIP
> If you are unfamiliar with executing shell commands from Jupyter Notebooks, read this [tutorial by Jake VanderPlas](https://jakevdp.github.io/PythonDataScienceHandbook/01.05-ipython-and-shell-commands.html).
{: .challenge}

## Creating the content of your book based on _The Turing Way_

In order to build our Jupyter Book, we first need to create a folder where all the content of your book exists.
This is also where Jupyter Book will create the `html` files in order to host the book online.
As demonstrated for *The Turing Way*, we will store all the content of our book in a folder named `book` located in the main repository (it doesn't need to be named this way for Jupyter Book to work). Let's create it:

~~~
mkdir ../book/
~~~
{: .language-bash}

>
{: .callout}

The folder should be empty as you have just created it!

__NOTE__: If this folder already exists, you are probably re-running this notebook without having cleared the `book` folder. Make sure you remove it using this command:

```
$ rm -R book/*
```

### Example files for this tutorial

We will now add some contents such as a book chapter and other important components to our `book` folder.

In this tutorial, we will be using some of the chapters from _[The Turing Way](https://the-turing-way.netlify.app/welcome.html)_.
We have provided some example files, which are placed under the folder `content` in the main repository.

Let's inspect whats inside it:
~~~
ls ../content
~~~
{: .language-bash}

There are 2 Markdown files: `welcome.md` and `reproducible-research.md`.
These are [welcome page](https://the-turing-way.netlify.app/welcome) and [introduction to the Guide for Reproducible Research](https://the-turing-way.netlify.app/reproducible-research/reproducible-research.html) in _The Turing Way_ respectively.

The `references.bib` is a bibtex file, which contains all the references and a `figures` folder, which contains all the figures used in these files.
To read more about citing using sphinx and Jupyter Book head [here](https://jupyterbook.org/content/citations.html#).

There are also two folders, `overview` and `open`, that contain subchapters of the overview and open science chapters respectively (you can see them in the online [_The Turing Way_ book](https://the-turing-way.netlify.app/reproducible-research/overview.html)).

### Let's start!

For our first book we don't want all these files.
Let's import to our `book`folder only the necessary files one by one.

We first need an index page for our book, which is `welcome.md` file in _The Turing Way_.
Let's copy it into our `book` folder:

~~~
cp ../content/welcome.md ../book/
~~~
{: .language-bash}

We will now need a chapter. Let's use the second Markdown file `reproducible-research.md` as our first chapter.

~~~
cp ../content/reproducible-research.md ../book/reproducible-research.md
~~~
{: .language-bash}

These files link to some citations and images placed in the `references.bib` file and `figures` folder, which we will also copy to our `book` folder:

~~~
cp -R ../content/figures/ ../book/figures
~~~
{: .language-bash}

~~~
cp ../content/references.bib ../book/references.bib
~~~
{: .language-bash}

Let's make sure our `book` folder now contains all the files we want:

~~~
ls ../book/
~~~
{: .language-bash}

You should see printed a `figures`, `references.bib`, `welcome.md` and `reproducible-research.md` file.

> **We are almost there!**
>
> But we can not yet build our Jupyter Book.
> We have to specify the structure of our book - how would your book look like.
> Let's do this in the next section.

### Creating the structure of your Jupyter Book (`_toc.yml`)

A book always have a structure, that is, a __Table Of Contents (TOC)__  that defines the hierarchy and order of the content-files.

Similarly, to create a Jupyter Book we need to have a file that specifies the TOC. Jupyter Book defines the TOC using a [yaml](https://en.wikipedia.org/wiki/YAML) file that needs to be named `_toc.yml`. This can be created manually, or automatically by running:

```shell
$ jupyter-book toc {path}
```

as long as the intended structure is also ordered alphanumerically.

But in The Turing Way we are always creating new content and restructuring our book. In these cases, is not practical to have our filenames ordered alphanumerically. We will thus show you how to manually create our `_toc.yml` using the python library [ruamel.yaml](https://yaml.readthedocs.io/en/latest/).

Create a file name `create_toc.py` in your current directory and copy-paste the following code:

~~~
from ruamel.yaml import YAML

yaml = YAML()

# Define the contents of our _toc.yml
toc_document = """
- file: welcome
- file: reproducible-research
"""

# Save _toc.yml in the book directory
toc_file = open('../book/_toc.yml', 'w')
yaml.dump(yaml.load(toc_document), toc_file)
~~~
{: .language-python}

If using the Jupyter Notebook, you can run this script directly in a code cell.

{% include links.md %}
