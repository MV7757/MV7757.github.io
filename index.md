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
1. Loading a prepared dataset with both text content and metadata, as well as, normalizing the data.
2. Extract structured information from processed documents in our case n-grans and key terms
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
### 3.1 Loading a prepared dataset with both text content and metadata and cleaning the data.
Textacy has a number of built in datasets with thousands of rows of data. For example, this tutorial will look at a dataset that contains ~11k speeches spoken by members of congress. The following code will load the dataset. To find a list of the datasets that are provided by textacy click [here](https://textacy.readthedocs.io/en/0.11.0/api_reference/datasets_resources.html)
```
import textacy.datasets
dataset = textacy.datasets.CapitolWords()
dataset.download()
```
Now that we have our dataset downloaded, we can look at that information is provided to us. We will act is if these commands were run into a python shell after the above code.
```
>>> record = next(dataset.records(limit=1))
>>> record
Record(text='Mr. Speaker, 480,000 Federal employees are working without pay, a form of involuntary servitude; 280,000 Federal employees are not working, and they will be paid. Virtually all of these workers have mortgages to pay, children to feed, and financial obligations to meet.\nMr. Speaker, what is happening to these workers is immoral, is wrong, and must be rectified immediately. Newt Gingrich and the Republican leadership must not continue to hold the House and the American people hostage while they push their disastrous 7-year balanced budget plan. The gentleman from Georgia, Mr. Gingrich, and the Republican leadership must join Senator Dole and the entire Senate and pass a continuing resolution now, now to reopen Government.\nMr. Speaker, that is what the American people want, that is what they need, and that is what this body must do.', meta={'date': '1996-01-04', 'congress': 104, 'speaker_name': 'Bernie Sanders', 'speaker_party': 'I', 'title': 'JOIN THE SENATE AND PASS A CONTINUING RESOLUTION', 'chamber': 'House'})
```
Record is now the first row of the dataset, and we can see we now have some text, as well as, the metadata behind when this was spoken such as datae, congress, speaker_name, speaker_party, title, and chamber. 

We would now want to normalize the data so that we don't have any errors caused by inconsistency in whitespace, punctuation, or unicode characters. To do this, we can run the following script.
```
from textacy import preprocessing as preproc
preprocessor = preproc.make_pipeline(
        preproc.normalize.unicode,
        preproc.normalize.quotation_marks,
        preproc.normalize.whitespace)
preproc_text = preprocessor(record.text)
```

### 3.2 Extracting n-grams and key terms from the document

