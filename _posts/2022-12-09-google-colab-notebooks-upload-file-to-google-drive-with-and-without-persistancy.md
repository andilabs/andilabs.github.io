---
title: "Google colab notebooks \U0001F64F❤️ - upload file to google drive with and
  without persistancy"
---

# file(s) upload

After running the:

```python
from google.colab import files
uploaded_files = files.upload()
```

you will get button for file(s) upload

![](/assets/upload_files.png)

the upload_files is **dictionary** containing **file names as keys** and **file content as value**.

# preview

you can easily preview the files, evne binary ones with `files.view('filename.png')`

![](/assets/upload_files_preview_img.png)


# where are my files???? Why were they lost?

When you upload your files you might be suprised why this are not visible inside the google drive

![](/assets/uploaded_files_but_empty_drive_google_colab.png)

This files are just temporary for lifetime of running the notebook, until you....

# mount your google drive
```python
from google.colab import drive
drive.mount('/content/drive')
```

WARNING: you will be prompted by google to gran notebook runtime to your google drive

after navigating to proper directory and running upload again, you will find now your files persisted inside your gdrive

![](/assets/upload_of_files_after_drive_mount_and_cd_to_proper_driectory.png)

# happy coding 👩‍💻👨‍💻❤️

<script src="https://gist.github.com/andilabs/23bebf0f27fa5435324e09e4920b6d1d.js"></script>
