---
title: prevent commiting unwanted changes using git hooks pre-commit
---

to avoid pushing stuff you don't want, or do some basic validations you can create in your `.git/hooks` directory the `pre-commit` file containg e.g some bash script and returning `exit 1` when some condition identifing things shouldn't be commited is true, othersie `exit 0` for OK case.

Remember to make this file executable with `chmod +x pre-commit` 

E.g check if file `secrets_vars.yml` contains word `password`

{% highlight bash %}
if grep -q password secrets_vars.yml; then
  echo "please encrypt the secrets, found raw word password"
  exit 1
fi

exit 0
{% endhighlight %}

more on git hooks here: [https://www.atlassian.com/git/tutorials/git-hooks](https://www.atlassian.com/git/tutorials/git-hooks)