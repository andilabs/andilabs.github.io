---
title: "Itermocil \U0001F60D magic tool for iTerm"
---

In the past I made several attempts inspired by linux tools like tmux, byobu etc to have  predefined terminal shell layouts which can be setup with one click. But this didn't work very cool. And here is the tool that makes the difference for all iTerm users.

# Itermocil
[https://github.com/TomAnthony/itermocil](https://github.com/TomAnthony/itermocil)

# setup
setup is as easy as:
1. install with brew `brew install TomAnthony/brews/itermocil`
1.  create `.itermocil` directory in your home directory
1. and create e.g `magic.yml`  files inside with config for your projects (some example configs [https://github.com/TomAnthony/itermocil#simple-three-pane-window-flipped](https://github.com/TomAnthony/itermocil#simple-three-pane-window-flipped)
1. run `itrmocil magic` to get you iTerm nicly setup

# example
Here is some example of mine with 4 panes for some django project (server, git, shell_plus, postgres)

```
windows:
  - name: shopify-store
    root: ~/Projects/shopify-store
    layout: tiled
    panes:
      - commands:
        - source /venv/bin/activate
        - ./manage.py runserver 0:9090
      - commands:
        - git pull
        - watch git st
      - commands:
        - source /venv/bin/activate
        - ./manage.py shell_plus
      - commands:
        - /Applications/Postgres.app/Contents/Versions/latest/bin/psql -d shopify_store
```