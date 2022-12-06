---
title: freezgun - to the rescue for tests relaying on time
---

Time related issues are tricky part in software engineering. So they require proper testing. 

### Few ideas how to tackle testing time related part of code:
- parametrize tested methods/functions with explicit time passed - sometimes a bit kind of test-only usage, because inside anyway datetime.now() is used
- mocking
- using specialized library like freezgun for efficent time mocks


### Two major options to use freezgun in your tests:

- conext manager

- decorator

<script src="https://gist.github.com/andilabs/acadc35cbff2705614265dc1a6f9964f.js"></script>

### other cool things
- support for timezones
- support for manual ticking and auto ticking with delta

more: https://github.com/spulec/freezegun
