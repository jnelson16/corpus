# The QuantGov Corpus

## Official QuantGov Corpora

This repository is for those who would like to create new datasets using the QuantGov platform. If you would like to find data that has been produced using the QuantGov platform, please visit https://www.quantgov.org/data.

This repository contains all official QuantGov corpora, with each corpus stored in its own branch. The current list of official corpora includes:

* Generic Corpus
* CFR
* eCFR
* Federal Register

### The Generic Corpus

The `master` branch of this repository is the Generic Corpus, which serves all files in the data/clean folder, with the file path as the index. See the `Snakefile` for more details.

### CFR

The `cfr` branch of this repository is the Code of Federal Regulations (CFR) corpus, which uses the file path as the index. Each folder along the path represents the title, chapter, and part of the regulation to be analyzed.

### eCFR

The `ecfr` branch of this repository is the Electronic Code of Federal Regulations (eCFR) corpus, a daily updated version of the CFR available through the [U.S. Government Publishing Office](https://www.ecfr.gov/cgi-bin/ECFR?page=browse). The corpus organization is similar to the CFR.

### Federal Register

The `federal-register` branch of this repository is the Federal Register corpus.

## Builtin Drivers

The QuantGov library includes three builtin corpus drivers:

* **Recursive directory corpus driver** serves a corpus with files organized in a recursive directory. The index labels are defined by the user in the driver, and the index values are the names of the subdirectories.

* **Name pattern corpus driver** serves a corpus with all files in a single directory and filenames defined by a regular expression. The index labels are the group names contained in the regular expression in the order that they appear.

* **Index driver** serves a corpus using an index csv where the final column is the path to the file and the other columns form the index. Index label names are taken from the csv header.

## Builtin Functions

The QuantGov library includes several builtin text-analysis functions for use on the files in the corpus:

* **Word counter** counts the number of words in each file. The word pattern is defined by a regular expression.

* **Occurrence counter** counts the number of specified words in each file. The user must specify one or more words to be counted.

* Complexity measures estimate the complexity of the language in each file:

    * **Shannon Entropy** does Shannon Entropy stuff.
    * **Conditional counter** counts the number of conditionals (words like "if," "but," "except, " etc.) as an estimate of linguistic complexity.
    * **Sentence length** measures the average length of sentences within each file.

* **Sentiment analysis** estimates the polarity and subjectivity of the language in each file. Polarity is measured from -1 to 1, with -1 respresenting very negative language and 1 representing very positive. Subjectivity is measured from 0 to 1, with 0 representing very objective text and 1 representing very subjective.

Each of these functions can be used on the command line. For example, to estimate the sentiment of a corpus, the following command can be used:

```
python -m quantgov corpus sentiment_analysis CORPUS_PATH
``` 

## Using this Corpus

To use or modify this corpus, clone it using git or download the archive from the [QuantGov Site](https://quantgov.org/tools/) and unzip it on your computer.

## Requirements

Using this corpus requires the [QuantGov library](https://github.com/quantgov/quantgov). 
