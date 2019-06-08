<p align="center">
  <img src="https://snag.gy/cwnUGB.jpg">
</p>

This is the public repo we're going to be using to post updates on Arxiv-Manatee, which are going to be tools for searching through Arxiv-Sanity papers. 

If you would like be involved, feel free to reach out to Research2Vec@gmail.com. We could use those with experience with Tensorflow/Keras/Pytorch, 
NLP, abtractive summarization, and text/data processing, but we're open to considering anyone!

Our previous projects

https://github.com/re-search/DocProduct

https://github.com/re-search/gpt2-estimator

https://github.com/Santosh-Gupta/Research2Vec

https://github.com/Santosh-Gupta/Lit2Vec

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

