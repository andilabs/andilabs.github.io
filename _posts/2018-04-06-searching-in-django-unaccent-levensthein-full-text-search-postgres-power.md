---
title: searching in Django - Levenshtein distance + postgres power
---

The aim is to show off how to play a bit with searching text in Django (assuming Postgres as a db)


Levenshtein distance based search
=====
**What is Levenshtein distance?**

>  (...) the Levenshtein distance between two words is the minimum number of single-character edits (insertions, deletions or substitutions) required to change one word into the other.(...)
From: [https://en.wikipedia.org/wiki/Levenshtein_distance](https://en.wikipedia.org/wiki/Levenshtein_distance)

**Why use it?**

The most common goal is to enhance searching quality by providing user even if he misspell the search term.

**Basic postgres Example**

{% highlight sql  %}
better_spots=# select levenshtein(name, 'Kfaka'), name from core_spot;

 levenshtein |          name
-------------+-------------------------
           6 | Szwejk
          22 | Charlotte. Chleb i Wino
          12 | Cafe Kulturalna
           2 | Kafka
           5 | Jeffs
          12 | Pardon to tu
           9 | Cafe Próżna
          17 | Drukarnia Jazz Club
{% endhighlight %}

In first collumn is the distance (number of changes required to get acctual term from `Kfaka` term). One can easily create filters by setting in WHERE clause that these distance is e.g `<= 3`.

**Extension needed `fuzzystrmatch`**

To run it you neeed adding postgres extension to your db in psql:
`CREATE EXTENSION fuzzystrmatch;` 

To add it in ansible role:
{% highlight yaml %}
  - name: Add fuzzystrmatch extension
    become_user: postgres
    become: yes
    postgresql_ext:
      name: fuzzystrmatch
      db: better_spots
{% endhighlight %}

**Getting there in Django:**

Lets define custom function with wich we can annotate queryset. It just take one argument the search_term and uses postgres `levenshtein` function (see [docs](https://www.postgresql.org/docs/10/static/fuzzystrmatch.html#id-1.11.7.25.6)):

{% highlight python %}
from django.db.models import Func

class Levenshtein(Func):
    template = "%(function)s(%(expressions)s, '%(search_term)s')"
    function = "levenshtein"

    def __init__(self, expression, search_term, **extras):
        super(Levenshtein, self).__init__(
            expression,
            search_term=search_term,
            **extras
        )
{% endhighlight %}

then in any other place in project we just import defined `Levenshtein` and `F` to pass the django field.

{% highlight python %}
from django.db.models import F

Spot.objects.annotate(
    lev_dist=Levenshtein(F('name'), 'Kfaka')
).filter(
    lev_dist__lte=2
)
{% endhighlight %}

Disclaimer: Django offers since 1.10 trigram-similarity
=====================

Worth notice that Django offers since 1.10 trigram-similarity it may be fair enough for your needs.

{% highlight python %}
Spot.objects.filter(name__trigram_similar="Kawka")
{% endhighlight %}


https://docs.djangoproject.com/en/2.0/ref/contrib/postgres/lookups/#trigram-similarity

it requires:

- adding trigram extension to postgres `CREATE EXTENSION pg_trgm;`
- having `'django.contrib.postgres'` in your `INSTALLED_APPS `