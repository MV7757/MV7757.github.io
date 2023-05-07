# Textacy
A technical tutorial for utilizing textacy in natural language processing projects.

# Table of contents
1. [Overview](#overview)
2. [Installation](#installation)
3. [Usages](#usages)

<div id='overview'></div>

## 1. Overview  
Textacy is a tool built on top of [spacy](https://spacy.io/) that handles problems before and after the natural languge processing that spacy already provides. Commonly addressed problems that textacy solves are:
- Access and extend spaCy's core functionality for working with one or many documents through convenient methods and custom extensions
- Load prepared datasets with both text content and metadata, from Congressional speeches to historical literature to Reddit comments
- Clean, normalize, and explore raw text before processing it with spaCy
- Extract structured information from processed documents, including n-grams, entities, acronyms, keyterms, and SVO triples
- Compare strings and sequences using a variety of similarity metrics
- Tokenize and vectorize documents then train, interpret, and visualize topic models
- Compute text readability and lexical diversity statistics, including Flesch-Kincaid grade level, multilingual Flesch Reading Ease, and Type-Token Ratio

However, in our tutorial we will be focusing on three common use cases of textacy from above.
1. Loading a prepared dataset with both text content and metadata.
2. Extract structured information from processed documents in our case n-grans and key terns
3. Compute text readability and lexical diversity statistics

<div id='installation'></div>

## 2. Installation
Textacy is library that is specific to Python, so we will be utilizing Python3 in order to run the commands we will need. 

### 2.1 How to install Python3
Python can be downloaded [here](https://www.python.org/downloads/) by simply following the instructions from the link provided.

### 2.2 How to install Textacy
Textacy is a package that can be installed simply by using `pip`. Run the following command in command prompt in order to install textacy.
```
$ pip install textacy
```

<div id='usages'></div>

## 3. Common Textacy Usages 
### 3.1 Loading a prepared dataset with both text content and metadata.

