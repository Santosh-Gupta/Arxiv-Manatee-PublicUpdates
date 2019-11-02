<p align="center">
  <img src="https://snag.gy/cwnUGB.jpg">
</p>

This is the public repo we're going to be using to post updates on Arxiv-Manatee, which are going to be tools for searching through Arxiv-Sanity papers. We are looking to use deep language models to produce keyword/phrase invarient searches through machine learning papers.

If you would like be involved, feel free to reach out to Research2Vec@gmail.com. We could use those with experience with Tensorflow/Keras/Pytorch, NLP, abtractive summarization, and text/data processing, but we're open to considering anyone!

Our previous projects:

- https://github.com/re-search/DocProduct
- https://github.com/re-search/gpt2-estimator
- https://github.com/Santosh-Gupta/Research2Vec
- https://github.com/Santosh-Gupta/Lit2Vec

I update this Readme with questions and problems I run into through the project. If you have any insight to these, please give feedback through opening a new issue. 

## Update 11-1-19

Not much of an update this week. At the moment, I'm trying to figure out what are the best practices for the HuggingFace Transformers library, specifically for training similarity on passages of text. More details in this issue
https://github.com/huggingface/transformers/issues/1700

## Update 10-25-19 

I am currently processing the final dataset for training, which will take a while to complete. In the meanwhile, I am trying to figure out how to most optimally use the Hugingface transformers with the newly updated Pytorch XLA library for TPU training. Details here

https://github.com/pytorch/xla/issues/1217
https://github.com/huggingface/transformers/issues/1540#issuecomment-543406725

Also, in DocProduct, a Relu was used as the final non-linearity for creating the embeddings. Although DocProduct ebmedddings had great retrieval properties, I am not sure if it's the best non-linearity for creating embeddings which will then be used in cos similarity. More details here. 

https://www.reddit.com/r/LanguageTechnology/comments/dn3zam/thought_experiment_is_relu_the_best_activation/?



## Update 10-21-19 

This project will just a extractive summarizer which I have recently finished training. Details on the training data and model released can be found here
https://github.com/Santosh-Gupta/ScientificSummarizationDataSets


Currently, I am analyzing the input length distributions of the data to see the batching and padding should be handled. I asked a question on the HuggingFace Transforers repo for insights on this consideration
https://github.com/huggingface/transformers/issues/1581

## Update 10-20-19

I haven't been updating this page too, much, but I will be now, to keep track of the considerations and things I learn a long the way. 

Current Considerations:

I am sampling data with different distributions of input lengths. Since inputs need to be padded to the maximum length in the batch, I am wondering if I should seperate these batches while inputting them into the Bert model. This is fully described in this question I asked
https://github.com/huggingface/transformers/issues/1581

I would like to use Pytorcher's DataSet and DataLoader classes, and I am trying to figoure how to handle sampling. Since the training will have negative sampling, I will be retrieving several random samples during training. Since I do not watch the loading of files to be a bottle neck in the training, I would like my pipeline to be this: Random select a file to open, but then go down each sample in that file as training inputs. 

My data is in multiple files, with multiple lines, as described here
https://discuss.pytorch.org/t/dataloaders-multiple-files-and-multiple-rows-per-column-with-lazy-evaluation/11769
But the solution presentented is for selected any sample, at random. 

I think for my requirements, I need to define a custom sampler here https://pytorch.org/docs/stable/data.html#torch.utils.data.Sampler


## Update 07-1-19

I am proud to present a summarization dataset for machine learning concepts. 

https://drive.google.com/open?id=1B8qqHQNXZ4OVMKpGHAvrxPxdQlKt2Ia4

The dataset contains all 89252 machine learning papers ( cs.[CV|CL|LG|AI|NE]/stat.ML ) from Arxiv (1993-June 2019). The 'summary' is the title of the paper, and the 'source' is the abstract of each paper.  

I am focused on concept summarization (as opposed to factual summarization), so all special characters and digits have been filtered out, so that the summarizars that are trained over this data can focus on the concepts described in the text. 

---

## Update 06-30-19

Not the most exciting update, but the sectional summarization dataset is still processing. I'm downloading about ~1.2 million papers, and processing each one of them into source:summary pairs using [Allen AI's Science Parse](https://github.com/allenai/science-parse). I am about 60% done, so the dataset should be done by next week. And yes, the dataset will be publicly availible! 

---

## Update 2019-23-06

### DataSet

2nd dataset is in process of being created. I uploaded the code I used to create the dataset from the Semantic Scholar corpus to this repo. 

Dataset design is inspired by [Structured Summarization of Academic Publications](https://arxiv.org/abs/1905.07695), by Alexios Gidiotis, Grigorios Tsoumakas. The data set they have created is a reserach paper sectional summarizar, which is exactly I am working on! The dataset design used papers with structured abstracts (so each abstract has a Intro, Background, Methods, Results, Conclusion, etc. section), and used each section of the structured abstract to act as a summary for the corresponding main section of the paper. 

There are several differences between the dataset they have released and the one I will release sometime this week:

- The papers in the Gidiotis/Tsoumakas dataset are directly from pubmed, and are in the field of chem/bio/medical. The papers from my dataset are from the [Semantic Scholar corpus](https://api.semanticscholar.org/corpus/). I initiated starting my own dataset because I wanted to have an equivilant dataset for summarizing CS papers. Unfortunatly, when the SemanticScholar Corpus was filtered to those which only contain structured abstracts, 95% of those were from the biomedical domain. It looks like it is very rare that structured abstracts are used outside the biomedical domian. Since Pubmed is a source of research papers for the Semantic Scholar Corpus, there will probably be much overlap between our datasets. 

- Gidiotis/Tsoumakas used XML to directly match abstract sections from main paper sections. I have directly downloaded the PDFs, then use [Allen AI's Science Parse](https://github.com/allenai/science-parse) to split up the papers into it's headers and sections, and then matched the abstract section with the main paper sections. 

- Gidiotis/Tsoumakas has data from 712,911 papers. My dataset will have data from ~1,300,000 papers.

- My dataset has digits filtered out, I do not think Gidiotis/Tsoumakas filtered out any digits. The summarization I am focused on is for concepts, rather than facts/metics/quantitative information, so I removed all digits from both the source and summary text to empahsize concept summarization. 

- Gidiotis/Tsoumakas classified sections marked as 'experimental', 'experiments', and 'experiment' as 'resuts'. From my own review of my data, those sections to me are closer in meaning to 'methods', so that's thats how I classified those sections. 

### Summarization Architectures

I am considering two main summarization architectures, and potentially a 3rd for the time being:

- [Efficient Adaptation of Pretrained Transformers for Abstractive Summarization](https://arxiv.org/pdf/1906.00138.pdf) from Allen AI / University of Washington / Microsoft Research
  - https://github.com/Andrew03/transformer-abstractive-summarization
- [Get To The Point: Summarization with Pointer-Generator Networks](https://arxiv.org/abs/1704.04368) from Stanford / Google Brain
  - https://github.com/yaserkl/RLSeq2Seq
  - https://github.com/abisee/pointer-generator
- [Unified Language Model Pre-training for Natural Language Understanding and Generation](https://arxiv.org/abs/1905.03197) from Microsoft Research

First, I am focusing on abstractive summarization, as opposed to extractive summarization. This is because I want the final paper retrieval model to be robust against phrase and keyword variations. Also, the summaries in my summarization dataset often do not contain the exact phrasing in the source text. 

I like "Efficient Adaptation of Pretrained Transformers for Abstractive Summarization" because they evaluated their results by surverying people to judge the summaries. ROUGE currently is the most commanly used metric to get a quantitative score for summary quality, but it may not translate fully to a human's qualitative assessment for summary quality. 

Pointer-Generator networks are the most commonly used method for abstractive scientific summarization. It was used in [Structured Summarization of Academic Publications](https://arxiv.org/abs/1905.07695), [Ontology-Aware Clinical Abstractive Summarization](https://arxiv.org/abs/1804.05685), and in [Domain-Aware Abstractive Text Summarization for Medical Documents](https://www.semanticscholar.org/paper/Domain-Aware-Abstractive-Text-Summarization-for-Gigioli-Sagar/6f46d1520c18daa93109954652bcd377b86538c0). It seems that since scientific texts contain so much domain-specific phrasing and terminology, Pointer-Generators are well suited to abstractive summarization of those texts since they can point to whatever the laguage model has trouble summarizing. 

Finally, as far as I know "Unified Language Model Pre-training for Natural Language Understanding and Generation" as achieved the highest scores for abstractive summarization challenges. However, their github repo has note been released yet. So first set of summaries, I will be considering the first two options for now. 

---

## 2019-07-06

Computer science summmarization datasets completed

This is a dataset of 5.6 million title / abstract data points, about 75% of which are from computer science papers (I tried my best to filter all non-CS papers (perhaps the non-CS papers add a bit of a "regularization" effect . . . ?) ) .

Title/Abstract pairs have been used to train biomedical summarizers [https://arxiv.org/pdf/1804.08875.pdf], but I am doing a project on CS/ML papers so I made my own.

The dataset is basically a filtered version of the [Semantic Scholar Corpus](https://api.semanticscholar.org/corpus/).

But it took some effort to produce it and I figure I may save some people time if they wanted the same.

This is a zip file containing 12 parquet files

https://drive.google.com/open?id=1WEdf-_au3vg2EzmWhawmW9xsYaHAE7iV

it's ~2.5 gb zipped, I think like 6 something gigs unzipped

This is the sqlite database version, 1 file

https://drive.google.com/open?id=1IhIaBD98BEseteAUi1S_f_SfIaUI8V4D

it's 2.5 gb zipped, 7.5 gb unzipped
