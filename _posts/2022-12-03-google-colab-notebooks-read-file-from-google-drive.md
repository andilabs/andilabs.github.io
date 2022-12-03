---
title: "Google colab  notebooks \U0001F64F❤️ - read file from google drive"
---

### Imagine you would like to process some file based next to your noteboook.


If you try to acess it without certain prerequisit (in form of mounting, and moving to proper directory) you will get **FileNotFoundErrror**

You have to do the following:

### 1) mount google drive

```python
from google.colab import drive
drive.mount('/content/drive')
```

### 2) navigate to right directory 

```
import os
os.chdir('/content/drive/My Drive/Colab Notebooks/python-audio-processing')
```


### Some example with reading audio file:

<script src="https://gist.github.com/andilabs/f90122f1f4cfd9edb1db34ea62fdf323.js"></script>
