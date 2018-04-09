---
title: using JSON Schema for validating correctness of json
---

JSONSchema is way of defining what shape your desired JSON should have and what certain fields type and content should be.

> JSON Schema is a vocabulary that allows you to annotate and validate JSON documents.

project's webiste: [http://json-schema.org/](http://json-schema.org/)

GREAT DOCS ❤️[https://spacetelescope.github.io/understanding-json-schema/index.html](https://spacetelescope.github.io/understanding-json-schema/index.html)

in python project you can use this pip installed package [https://pypi.python.org/pypi/jsonschema](https://pypi.python.org/pypi/jsonschema)

example validation:
=====

{% highlight python %}


from jsonschema import validate

schema = {
    "type": "object",
    "required": ["department", "employees"],
    "properties": {
        "department": {
            "type": "string"
        },
        "employees": {
            "type": "array",
            "items": {
                "type": "object",
                "required": ["firstName", "lastName", "age", "hobbys"],
                "properties": {
                    "firstName": {
                        "type": "string"
                    },
                    "lastName": {
                        "type": "string"
                    },
                    "age": {
                        "type": "integer"
                    },
                    "hireDate": {
                        "type": "string",
                        "pattern": "([12]\d{3}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01]))$"
                    },
                    "hobbys": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        },
                        "maxItems": 2
                    }
                }
            }
        }
    }
}

foo = {
    "department": "IT",
    "employees" : [
        {
            "firstName": "John", 
            "lastName": "Clever", 
            "age": 44,
            "hireDate": "2018-04-06",
            "hobbys": []
        },
        {
            "firstName": "Andi", 
            "lastName": "Kostanski", 
            "age": 29,
            "hireDate": "2018-04-06",
            "hobbys": [
                "triathlon",
                "skitour",
            ]
        }
    ]
}

validate(foo, schema)
{% endhighlight %}

Defining list of basic types vs list of objects 
=================
You define list of basic types, like e.g `"hobbys"` in employee just by defining `"type" ` inside `"items"` dict:
{% highlight python %}
"hobbys": {
    "type": "array",
    "items": {
        "type": "string"
    },
    "maxItems": 2
}
{% endhighlight %}

while if you need to define list of certain shape objects you have to define `"type": "object"` and define  `"type"` of each `"properties"` inside `"items" `:

{% highlight python %}
"employees": {
    "type": "array",
    "items": {
        "type": "object",
        "required": ["firstName", "lastName"],
        "properties": {
            "firstName": {
                "type": "string"
            },
            "lastName": {
                "type": "string"
            }
        }
    }
}
{% endhighlight %}

More options to validate:
==========
- each of field can be in `required` list to make it obligatory
- for lists you can define `maxItems`, `minItems` to control  allowed number of objects.
- for ensuring list object uniqness just provide `"uniqueItems": true` this works only on basic data types  (not on objects)
-  you can validate strings with regex by using `pattern` like in above example for valid date in `YYYY-MM-DD`
-   tuple validation may be something nice for structured data types [https://spacetelescope.github.io/understanding-json-schema/reference/array.html#tuple-validation](https://spacetelescope.github.io/understanding-json-schema/reference/array.html#tuple-validation)


Usefull links of docs with greate examples:
================
- [object](https://spacetelescope.github.io/understanding-json-schema/reference/object.html)
- [array](https://spacetelescope.github.io/understanding-json-schema/reference/array.html)
- [boolean](https://spacetelescope.github.io/understanding-json-schema/reference/boolean.html)
- [numeric types](https://spacetelescope.github.io/understanding-json-schema/reference/numeric.html)
- [string](https://spacetelescope.github.io/understanding-json-schema/reference/string.html)

![](https://spacetelescope.github.io/understanding-json-schema/_images/octopus.svg)