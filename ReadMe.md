<p align="center">
  <img src="https://snag.gy/cwnUGB.jpg">
</p>

This is the public repo we're going to be using to post updates on Arxiv-Manatee, which are going to be tools for searching through Arxiv-Sanity papers. We are looking to use deep language models to produce keyword/phrase invarient searches through machine learning papers 

If you would like be involved, feel free to reach out to Research2Vec@gmail.com. We could use those with experience with Tensorflow/Keras/Pytorch, 
NLP, abtractive summarization, and text/data processing, but we're open to considering anyone!

Our previous projects

https://github.com/re-search/DocProduct

https://github.com/re-search/gpt2-estimator

https://github.com/Santosh-Gupta/Research2Vec

https://github.com/Santosh-Gupta/Lit2Vec

--------

6-23-2019

2nd dataset is in process of being created. Dataset design is inspired by this paper https://arxiv.org/abs/1905.07695 Structured Summarization of Academic Publications , by Alexios Gidiotis, Grigorios Tsoumakas. The data set they have created is a reserach paper sectional summarizar, which is exactly I am working on! The dataset design is they use papers with structured abstracts (so each abstract has a Intro, Background, Methods, Results, Conclusion, etc. section), and use each section of the structured abstract to act as a summary for the corresponding main section of the paper. 

There are several differences between the dataset they have released and the one I will release sometime this week:

-The papers in the Gidiotis/Tsoumakas dataset are directly from pubmed, and are in the field of chem/bio/medical. The papers from my dataset are from the Semantic Scholar corpus ( https://api.semanticscholar.org/corpus/ ). I initiated starting my own dataset because I wanted to have an equivilant dataset that is for summarizing CS papers. Unfortunatly, when the SemanticScholar Corpus papers where filtered to those which only contain structured abstracts, 95% of those were from the biomedical domain. It looks like it is very rare that structured abstracts are used outside the biomedical domian. Since Pubmed is a source of research papers for the Semantic Scholar Corpus, there will probably be much overlap. 

-My dataset has digits filtered out, I do not think Gidiotis/Tsoumakas filtered out any digits. The summarization I am focused on is for concepts rather than facts/metics/quantitative information, so I removed all digits from both the source and summary so that the summarizer I am designing can focus on concept summarization. 

-Gidiotis/Tsoumakas classified sections marked as 'experimental', 'experiments', and 'experiment' as 'resuts'. From my own review of my data, those sections to me are closer in meaning to 'methods', so that's thats how I classified those sections. 

-------------

6-7-19

Computer science summmarization datasets completed

This is a dataset of 5.6 million title / abstract data points, about 75% of which are from computer science papers (I tried my best to filter all non-CS papers (perhaps the non-CS papers add a bit of a "regularization" effect . . . ?) ) .

Title/Abstract pairs have been used to train biomedical summarizers [https://arxiv.org/pdf/1804.08875.pdf] , but I am doing a project on CS/ML papers so I made my own.

The dataset is basically a filtered version of the Semantic Scholar Corpus https://api.semanticscholar.org/corpus/

But it took some effort to produce it and I figure I may save some people time if they wanted the same.

This is a zip file containing 12 parquet files

https://drive.google.com/open?id=1WEdf-_au3vg2EzmWhawmW9xsYaHAE7iV

it's ~2.5 gb zipped, I think like 6 something gigs unzipped

This is the sqlite database version, 1 file

https://drive.google.com/open?id=1IhIaBD98BEseteAUi1S_f_SfIaUI8V4D

it's 2.5 gb zipped, 7.5 gb unzipped

----------------

