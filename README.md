How to add a bit of cognition into a robot
==========================================



Pre-requisites
--------------

All the code is in Python. It has been tested on Ubuntu 16.04. It should work on any recent enough 
Linux, probably on macOS as well, and maybe on Windows.

To make it easy to install the different bits, make sure `pip` is available on your system. On Ubuntu/Debian:

```sh
$ sudo apt install python-pip
```

## The knowledge base: minimalkb

We are going to use the very simple [minimalKB knowledge base](https://github.com/severin-lemaignan/minimalkb):

```sh
$ sudo apt install python-rdflib # to be able to load OWL ontology
$ pip install --user minimalkb
```

Then:

```sh
$ minimalkb
minimalKB 0.9
2017-02-17 12:04:24,445: Starting to serve at port 6969...
2017-02-17 12:04:24,446: Reasoner (simple RDFS) started. Classification running at 5Hz
2017-02-17 12:04:24,447: Knowledge lifespan manager started. Running at 2Hz
```

Good, we have a symbolic knowledge base running. Let's use it.

Open a new terminal, and:

```sh
$ pip install --user pykb
```

Then, head to [pykb documentation](http://pykb.readthedocs.io/en/latest/) and play with the examples.

![oro-view](https://raw.githubusercontent.com/severin-lemaignan/oro-view/master/doc/oroview.jpg)

You can also try to install [oro-view](https://github.com/severin-lemaignan/oro-view) for a fancy interactive visualisation of your knowledge base.

## Dialogue processing

Using the [dialogs](https://github.com/severin-lemaignan/dialogs) toolkit, it is quite simple to (verbally!) interact with the knowledge base.

![dialogs](https://raw.githubusercontent.com/severin-lemaignan/dialogs/master/doc/dialogs_module_simple_small.png)

First install it:

```sh
$ pip install --user dialogs
```

Then, let's start a dialogue between the robot and `HUMAN`:
```sh
$ dialogs --debug HUMAN
```

Type on the command line your sentences, followed by `Enter`. In debug mode (`--debug`), the details of the processing is printed out.

You can play a bit with the system, but do not expect too much intelligence! The parser is limited, and it ability to answer questions really depends on what knowledge is available in the knowledge base.

Some examples of interactions

| Input                 | Response          | Changes to the knowledge base |
|-----------------------|-------------------|-------------------------------|
| Hello                 | Hello             |                               |
| Robots are machines   | Ok. Robots are machines. What are a robot and a machine? | `Robot rdfs:subClassOf Machine` |
| I'm a robot           | Alright. I'm a robot. What's a robot? | `HUMAN rdf:type Robot` |
| Give me a robot       | | `wLZcQ rdf:type Give`, `wLZcQ performedBy myself`, `HUMAN desires wLZcQ`, `wLZcQ actsOnObject HUMAN`, `wLZcQ receivedBy HUMAN` |

## What next?

- we can provide the knowledge base with some initial knowledge, in the form of an [ontology](https://en.wikipedia.org/wiki/Ontology_(information_science)). Check how to create an ontology with [Protege](http://protege.stanford.edu/products.php) or download the [oro common-sense ontology](http://kb.openrobots.org) for robots (save the XML file as `commonsense.owl`). You can then load this initial knowledge into `minimalkb` by simply running `minimalkb commonsense.owl`.
- we need to keep feeding the knowledge base with newly acquired knowledge: track faces, detect objects, keep up-to-date what is where and where is what.
- write a robot controller that can take of the knowledge base. For instance, [pyRobots](https://github.com/severin-lemaignan/pyrobots)
