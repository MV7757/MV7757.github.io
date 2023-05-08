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
2. Extract structured information from processed documents in our case n-grams, key terms, and text readability

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
However, we have just done this with a single row in the file. In order to do it over the first 1000 rows of the file, we can write code that looks like this
```
records = dataset.records(limit=1000)
# Using preprocessor from above
preproc_records = ((preprocessor(text), meta) for text, meta in records)
```
### 3.2 Extracting n-grams, key terms, and text readability from the document
Extracting key terms is something that is made extremely easy in textacy. You will first need to create a corpus (a textacy specific object that holds any number of records) then call teh [extract_keyterms](https://textacy.readthedocs.io/en/0.11.0/api_reference/extract.html?highlight=extract_keyterms#module-textacy.extract.keyterms) function on it. The below example will extract the keyterms from the first record in the dataset. 
```
# Using preproc_records from earlier
corpus = textacy.Corpus("en_core_web_sm", data=preproc_records)
corpus[0]._.extract_keyterms("textrank", normalize="lemma", window_size=10, edge_weighting="count", topn=10)
```
The output of this function would be
```
[('year balanced budget plan', 0.033721812470386026),
 ('Mr. Speaker', 0.032162715590532916),
 ('Mr. Gingrich', 0.031358819981176664),
 ('american people', 0.02612752273629427),
 ('republican leadership', 0.025418705021243045),
 ('federal employee', 0.021731159162187104),
 ('Newt Gingrich', 0.01988327361247088),
 ('pay', 0.018930131314143193),
 ('involuntary servitude', 0.015559235022115406),
 ('entire Senate', 0.015032623278646105)]
```

Extracting n-grams is just as easy in textacy. We can use the corpus object above and extract n-grams from the first record in the dataset using an import from textacy. For this example we will extract trigrams. 
```
from textacy import extract
list(extract.ngrams(corpus[0], 3, filter_punct=True))
```
The output of the following would be:
```
[480,000 Federal employees, employees are working, working without pay, form of involuntary, 280,000 Federal employees, workers have mortgages, mortgages to pay, children to feed, obligations to meet, workers is immoral, continue to hold, hold the House, American people hostage, push their disastrous, year balanced budget, balanced budget plan, gentleman from Georgia, leadership must join, join Senator Dole, Senate and pass, pass a continuing, American people want]
```

Computing text readability is just as simple as retrieving ngrams or key terms once you have your corpus. You simply have to import a package provided by textacy and call functions from that package. In order to compute text readability we will call the [flesch_kincaid_grade_level](https://textacy.readthedocs.io/en/0.12.0/api_reference/text_stats.html?highlight=flesch_kincaid_grade_level#textacy.text_stats.readability.flesch_kincaid_grade_level) function which will output a number between 1 and 100 that with 100 being a very high reading level and 0 being very low. Below is an example using the first row of our dataset
```
from textacy import text_stats as ts
ts.readability.flesch_kincaid_grade_level(corpus[0])
# the output of this function would be 11.40259124087591
```
