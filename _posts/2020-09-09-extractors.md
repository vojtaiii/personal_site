---
title: "Audio features extraction tools"
date: 2022-09-09T12:00:00-04:00
categories:
  - tools
tags:
  - wav
  - audio processing
  - audio features
  - deep learning
---

List of tool utilizing feature extraction from audio files and transcibed audio files (text).

## From waveform - acoustic

### openSMILE

Open-source audio feature extraction tool, notably know to extract the widely used extended Geneva Minimalistic Acoustic ParameterSet(eGeMAPS).

<https://www.audeering.com/research/opensmile/>

### DeepSpectrum

DeepSpectrum features extract spectral images from speech instances and are then fed in to pre-trained image recognition Convolutional Neural Networks(CNNs),and the resulting activations acan be extracted as feature vectors.

<https://github.com/DeepSpectrum/DeepSpectrum>

## From trascriptions - linguistic

The audio can be transcibed using Google Speech-to-Text (or Amazon transcribe) or Whisper (<https://github.com/openai/whisper>).

For Whisper, the audio is better to be normalized (-3 to -1 dB). It achieves the same result when rerun on the same file.

### LIWC-22 software

<https://www.liwc.app/>

A pre-trained NLP software anylysing text with various useful output features. Claims to have a good implementation of working with the words context and meanings, not just their linguistic type.

The drawback is that it costs money, even for research purposes.

### SenticNet

<https://sentic.net/>

Another powerful NLP tool, similar to LIWC-22 but not commercialized for research. Provides various APIs for sentiment analysis/emotional classification/text topic classification and others:

* Negative/positive ranking of texts
* Intensity ranking
* Emotion estimation from words (Hourglass of Emotions model)
* Toxicity ranking
* Well-being assessment
* Depression ranking
* Personality prediction

In the limited trials it showed significant influence on text length. When the text was short can outcomes were easily affected by changing few words.

### Latent semantic analysis

NLP approach using word vectors. Enables coherence assessment, or topic clustering and classification. The word vectors can be extracted by tools such as

* Fast Text (<https://fasttext.cc/>)
* Spacy.io (<https://spacy.io/>)
* Or other word2vec algorithm

### Tokenizers and lemmatizers

Used for PoS tagging and lemmatizing the words, finding similar words, and relationships between them.

* Spacy.io
* ConceptNet
* NLTK WordNet

### Deep approaches

AuDeep & end2you - powerful DNN techniques for classifying audio with an options for multimodal data (video, text). However, need proper training data.

* <https://github.com/auDeep/auDeep>
* <https://github.com/end2you/end2you>

BERT - A tranformer approach of classifying and processing texts. Pretrained models are only for English but bilingual models are to be released in the future. The training of the models is extremely computationally demanding, but tuning should not be such a problem. In MuSe challenge 2021, the authors used the ending layer of BERT as a feature vector for assessing topic and emotion classification (<https://www.muse-challenge.org/>).

<https://github.com/google-research/bert>

### Tools for sentiment analysis of words

* WordNet-affect: only for English
* Text Blob: sentiment analysis & subjectivity, pretrained for English, German, and French. Otherwise have to be trained.
