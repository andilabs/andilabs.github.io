---
title: Using dotenv files for managing environment variables
---

# What is `.env` file?
This is a plain text file containing key value entries like that:
`FOO=bar`

# .gitignore
Important: because environment variables are almost always meant to be used for storing secrets you have to make sure it is in your `.gitignore` and will never travel to repo.


# python
Handling usage of `.env` files in python (and django) is very easy using that library [https://github.com/theskumar/python-dotenv](https://github.com/theskumar/python-dotenv)

# Django
setting dotenv in django is easy: you need to add it to yours `wsgi.py` and `manage.py` and eventually `celery.py`:
```
import dotenv
dotenv.load_dotenv(os.path.join(os.path.dirname(os.path.dirname(__file__)), '.env'))
```

# Fabric
you can integrate setting new environment variables on remote machines easily into your `fabfile` see here:
[https://github.com/theskumar/python-dotenv#setting-config-on-remote-servers](https://github.com/theskumar/python-dotenv#setting-config-on-remote-servers)

For transporting the `.env`  file to remote machine just use fabric's `put` method.
# shell snippet
for fast export from file
`export $(cat .env)`