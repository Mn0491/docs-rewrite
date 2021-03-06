---
ID: 32454
post_title: Padatious
author: Kathy Reid
post_excerpt: ""
layout: page
permalink: http://mycroft.ai/?page_id=32454
published: false
---
# Padatious

Padatious is a [machine-learning](https://en.wikipedia.org/wiki/Machine_learning), [neural-network](https://en.wikipedia.org/wiki/Artificial_neural_network) based *intent parser*.

It is an alternative to the Adapt intent parser (@TODO: link to Adapt).

Padatious has a number of key benefits:

* With Padatious, **Intents** are easy to create
* The machine lerning model in Padtious requires a relatively small amount of data
* Machine learning models need to be *trained*. The model used by Padatious is quick and easy to train.
* Intents run independently of each other

@TODO I don't understand this one in detail - why does Intents running independently matter - why is this a benefit? Is it that there's no dependence - ie the Weather intent can be different to the Bushire risk intent?

* With Padatious, you can easily extract entities and then use these in **Skills**. For example, "Find the nearest gas station" -> `{ "place":"gas station"}`

## System generated documentation

[System generated documentation for the Padatious codebase, generated by Sphinx and hosted at ReadTheDocs](http://padatious.readthedocs.io/).

## What is an **Intent**? What is an **Intent parser**?

In speech recognition and voice assistance, an **intent** is the task the user *intends* to accomplish. A user can accomplish the same task in multiple ways. The role of the **intent parser** is to extract from the user's speech key data elements that specify their intent in more detail. This data can then be passed to other services, such as **Skills** to help the user accomplish their intended task.

*Example*: Julie wants to know about today's weather in her current location, which is Melbourne, Australia.

* `hey mycroft, what's today's weather like?`
* `hey mycroft, what's the weather like in Melbourne?`
* `hey mycroft, weather`

Each of these examples has very similar *intent*. The role of Padatious is to determine intent programmatically.

In the example above, we would extract data elements like:

* **weather** - we know that Julie wants to know about the weather, but she has not been specific about the type of weather, such as *wind*, *precipitation*, *snowfall* or the risk of *fire danger* from bushfires. Melbourne, Australia rarely experiences snowfall, but falls under bushfire risk every summer.
* **location** - Julie has stipulated her location as Melbourne, but she does not state that she means Melbourne, Australia. How do we distinguish this from Melbourne, Florida, United States?
* **date** - Julie has been specific about the *timeframe* she wants weather data for - today. But how do we know what today means in Julie's timezone. Melbourne, Australia is between 14-18 hours ahead of the United States. We don't want to give Julie yesterday's weather, particularly as Melbourne is renowned for having changeable weather.

## Creating **Intents**

Padatious uses a series of example sentences to train a machine learning model to identify an intent.

The examples are stored in a **Skill's** `vocab[lang]` directory, in files ending in the file extension `.intent`. For example, if you were to create a *tomato* **Skill** to respond to questions about a *tomato*, you would create the file

`vocab/en-us/what.is.intent`

This file would contain examples of questions asking what a *tomato* is.

* `What would you say a tomato is?`
* `What's a tomato?`
* `Describe a tomato`
* `What defines a tomato`


### API Example

Here's a simple example of how to use Padatious:

**program.py**:
```python
from padatious.intent_container import IntentContainer

container = IntentContainer('intent_cache')
container.load_file('hello', 'hello.intent')
container.load_file('goodbye', 'goodbye.intent')
container.train()

data = container.calc_intent('Hello there!')
print(data.name)
```

**hello.intent**:
```
Hi there!
Hello.
```

**goodbye.intent**:
```
See you!
Goodbye!
```

You can then run this in Python using:

```bash
python3 program.py
```

### Installing Padatious

#### Prerequisites

Padatious is designed to be run in Linux. Padatious requires the following native packages to be installed:

 - [`FANN`](https://github.com/libfann/fann) (with dev headers)
 - Python development headers
 - `pip3`
 - `swig`

To install these packages on a Ubuntu system, run this command:

```
sudo apt-get install libfann-dev python3-dev python3-pip swig
```

Next, install Padatious via `pip3`:

```
pip3 install padatious
```
Padatious also works in Python 2 if you are unable to upgrade.