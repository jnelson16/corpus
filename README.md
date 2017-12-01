# The QuantGov Corpus

## The Corpus Architecture

This repository is for those who would like to create new datasets using the QuantGov platform. To use or modify this corpus, clone it using git or download the archive from the [QuantGov website](https://quantgov.org/tools) and unzip it on your computer.

### Basic Structure

A corpus represents a set of documents, and implements the corpus driver interface. The root directory for a corpus should contain:

* A `Snakefile` to manage the workflow of the corpus.
* A module named `driver.py` that implements the corpus driver interface.
* A subdirectory named `scripts` containing the scripts needed to obtain, organize, and prepare the documents that make up the corpus, as well as to generate any metadata for the corpus.
* A subdirectory named `data` that holds any intermediate data generated during preparation of the corpus.

One file that each corpus should generate is a csv file in the `data` directory named `metadata.csv`. This file should contain any additional information about individual documents that may be relevant. For example, the `metadata.csv` generated by the CFR corpus includes which agency and department authored each individual CFR part, as well as the restriction and word count for each part.

### Corpus Driver Interface

The driver serves two important functions. First, it specifies how the corpus should be indexed. Second, the driver provides a function named `stream`, which emits the index value (or values) and text of each document in the corpus. Drivers may implement other features (such as only streaming a subset of documents based on the index), but these will be non-standard, and estimators should not expect them.

### Corpus Metadata

Relevant metadata will vary from corpus to corpus. Metadata can be generated from one of two sources: from the text itself or from additional external information. In the first case, the best practice is to write scripts that understand the corpus driver interface, and can therefore be used in other corpora. An example of this approach can be seen in the `get_wordcount.py` and `get_restriction_count.py` in the QuantGov generic corpus. In the second case, the external resources should be stored in a read-only databank separate from the corpus itself. An example of this approach is the agency attribution in the CFR corpus, which relies on a set of documents separate from the main CFR text.

## Official QuantGov Corpora

This repository contains all official QuantGov corpora, with each corpus stored in its own branch. The current list of official corpora includes:

* Generic Corpus
* CFR
* eCFR
* Federal Register

If you would like to find the data that has been produced using the QuantGov platform, please visit https://www.quantgov.org/data.

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

* **Recursive directory corpus driver** serves a corpus with files organized in a recursive directory. The index labels are defined by the user in the driver and the index values are the names of the subdirectories.

* **Name pattern corpus driver** serves a corpus with all files in a single directory and filenames defined by a regular expression. The index labels are the group names contained in the regular expression in the order that they appear.

* **Index driver** serves a corpus using an index csv where the final column is the path to the file and the other columns form the index. The index labels are taken from the csv header.

## Builtin Functions

The QuantGov library includes several builtin text-analysis functions for use on the files in the corpus:

* **Word counter** counts the number of words in each file. The word pattern is defined by a regular expression.

* **Occurrence counter** counts the number of specified words in each file. The user must specify one or more words to be counted.

* The following complexity measures estimate the linguistic complexity of each file:

    * **Shannon Entropy** does Shannon Entropy stuff.
    * **Conditional counter** counts the number of conditionals (words like "if," "but," "except, " etc.) as an estimate of linguistic complexity.
    * **Sentence length** measures the average length of sentences within each file.

* **Sentiment analysis** estimates the polarity and subjectivity of the language in each file. Polarity is measured from -1 to 1, with -1 respresenting very negative language and 1 representing very positive. Subjectivity is measured from 0 to 1, with 0 representing very objective text and 1 representing very subjective.

Each of these functions can be used on the command line. For example, to estimate the sentiment of a corpus, the following command can be used:

```
python -m quantgov corpus sentiment_analysis CORPUS_PATH
```

## Organization Strategies

The organization of the corpus will depend on its purpose. However, there are three principle problems to solve in the creation of any new corpus:

1. How can the text be obtained and, if necessary, converted to plain text?
2. What is the logical unit of analysis for the corpus?
3. How can the text be organized to best reflect the unit of analysis?

The first problem will determine the scripts needed for downloading or otherwise obtaining and cleaning the text from its published format. The second will determine the index for the corpus, and identify what makes an individual document, appropriate to be served through the driver. The third will determine how the driver is actually implemented.

## Requirements

Using this corpus requires the [QuantGov library](https://github.com/quantgov/quantgov). 
