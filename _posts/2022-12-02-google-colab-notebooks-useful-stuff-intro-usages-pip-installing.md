---
title: "Google colab  notebooks \U0001F64F❤️ - useful stuff [intro, usages, pip, installing]"
---

# INTRO

Is there something more lovely than having python interpreter accessible via web browser with jupyter  on whatever computer with just internet access?

I know jupyter notebooks (and jupyterhub instance as server for creating and running notebooks in my company) but having it offered as part of Google free services was great surprise!


# my personal use cases:
* playing with new libraries
* automating borning staff (e.g paying monthly taxes, bills by generating payments package I just upload in online banking, automated messaging of customers) 
* debugging
* consuming 3-rd party API-s
* visualizing data by ploting graphs

## It comes with plethora of pre-installed useful python libraries for data science and machine learning:

below short-list with links to documentation

### data, scientific calculations
* [pandas](https://pandas.pydata.org/docs/)
* [numpy](https://numpy.org/doc/stable/)
* [scipy](https://docs.scipy.org/doc/scipy/)

### natural language processing libraries like:
* [NLTK (Natural Language Toolkit)](https://www.nltk.org/)
* [Gensim](https://radimrehurek.com/gensim/auto_examples/index.html#documentation)
* [spacy](https://spacy.io/)
* [textblob](https://textblob.readthedocs.io/en/dev/)
* [torchtext](https://pytorch.org/text/stable/index.html)

### machine learning

* [scikit-learn](https://scikit-learn.org/stable/)
* [TensorFlow](https://www.tensorflow.org/api_docs/python/tf)
* [Keras](https://keras.io/api/)
* [PyTorch](https://pytorch.org/docs/stable/index.html)

### image processing
* [opencv](https://docs.opencv.org/4.x/)
* [torchvision](https://pytorch.org/vision/stable/index.html)

### audio processing
* [torchaudio](https://pytorch.org/audio/stable/index.html)



#  getting started

[ https://colab.research.google.com/](https://colab.research.google.com/)


# installing libs, checking whats already there

### to install smth new

`!pip install package-name`

### to check what is already installed

`!pip freeze`

<script src="https://gist.github.com/andilabs/e4cf61ffe065dda6a855beb93f83a0f0.js"></script>
