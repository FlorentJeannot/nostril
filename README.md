Nostril
=======

<img align="right" src=".graphics/nostril.png">

Nostril is the _Nonsense String Evaluator_: a Python module that infers whether a given medium-length string of characters is likely to be random gibberish or something meaningful.

*Author*:       [Michael Hucka](http://github.com/mhucka)<br>
*Repository*:   [https://github.com/casics/nostril](https://github.com/casics/nostril)<br>
*License*:      Unless otherwise noted, this content is licensed under the [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html) license.

☀ Introduction
-----------------------------

_Nostril_ is a Python 3 module that can be used to infer whether a given word or text string is likely to be nonsense or meaningful text.  Nostril takes a text string and returns `True` if it is probably nonsense, `False` otherwise (if it likely to be meaningful).  In the context of CASICS, the main use case is to decide whether strings returned by source code mining methods are likely to be (e.g.) program identifiers, or random characters or other non-identifier strings.

The approach implemented in Nostril uses [n-grams](https://en.wikipedia.org/wiki/N-gram) coupled with a custom [TF-IDF](https://en.wikipedia.org/wiki/Tf–idf) weighting scheme.  It is reasonably fast: once the module is loaded, on a 4 Ghz Apple OS X 10.12 computer, calling the evaluation function returns a result in 20-35 microseconds on average.

✺ Installing Nostril
-------------------

The following is probably the simplest and most direct way to install Nostril on your computer:
```
pip3 install git+https://github.com/casics/nostril.git
```

Alternatively, you can clone this repository and then run `setup.py`:
```
git clone https://github.com/casics/nostril.git
cd nostril
sudo python3 setup.py install
```

► Using Nostril
---------------

The basic usage is very simple.  Nostril provides (among other things) a function named `is_nonsense()`.  This function takes a text string as an argument and returns a Boolean value as a result.  Here is an example:

```python
from nostril import is_nonsense
result = is_nonsense('yoursinglestringhere')
```

⚠️ Limitations
--------------

Nostril is not fool-proof; **it _will_ generate some false positive and false negatives**.  This is an unavoidable consequence of the problem domain: without direct knowledge, even a human cannot recognize a real text string in all cases.  Nostril's default trained system puts emphasis on reducing false positives (i.e., reducing how often it mistakenly labels something as nonsense) rather than false negatives, so it will sometimes report that something is not nonsense when it really is.  With its default parameter values, on dictionary words (specifically, 218,752 words from `/usr/share/dict/web2`), the default version of `is_nonsense()` achieves greater than 99.99% accuracy.  In tests on real identifiers extracted from actual software source code, it achieves 99.94% to 99.96% accuracy; on truly random strings, it achieves 86% accuracy.  Inspecting the errors shows that most false positives really are quite ambiguous, to the point where most false positives are random-looking, and many false negatives could be plausible identifiers.

Finally, the algorithm does not perform well on very short text, and by default, Nostril imposes a lower length limit of 6 characters &ndash; strings have to be longer than 6 characters or else it will raise an exception.


📚 More information
-----------------

Please see the [docs](docs/README.md) subdirectory for more information.

⁇ Getting help and support
--------------------------

If you find an issue, please submit it in [the GitHub issue tracker](https://github.com/casics/nostril/issues) for this repository.

♬ Contributing &mdash; info for developers
------------------------------------------

It may be possible to improve Nostril's performance.  I would be happy to receive your help and participation if you are interested.  Please feel free to contact me directly, or jump right in and use the standard GitHub approach of forking the repo and creating a pull request.

Everyone is asked to read and respect the [code of conduct](CONDUCT.md) when participating in this project.

❤️ Acknowledgments
------------------

Funding for this and other CASICS work has come from the [National Science Foundation](https://nsf.gov) via grant NSF EAGER #1533792 (Principal Investigator: Michael Hucka).
