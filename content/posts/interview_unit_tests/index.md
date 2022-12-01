---
title: "Unit Tests for Python Interviews"
date: 2022-12-01T15:17:08-05:00
---

I'm interviewing again for the first time in over a decade. Thankfully the pandemic seems to have killed off the whiteboard coding interview. It was an insane way to interview programmers and I can't believe we deluded ourselves into thinking it was OK. I'm just remembering that I took my first job partly because the main coding interview was basically pairing on writing unit tests for buggy code and then fixing and refactoring it. It was such a better experience than any other interview!

Companies don't seem to have landed on a single remote interviewing platform. So far I've used HackerRank, Triplebyte, a Google Doc (!), and Google Colab. They're all pretty much fine (except a Google Doc, c'mon) but they all have their quirks. The one I'm grumpy about today is that Colab (and HackerRank half the time?) don't have a simple way to get a good code + test loop going.

Half the reason whiteboard coding feels bad for me is that I can't think without being able to test anymore. I can write a unit test faster than I can think through an edge case, so I just reach for them rather than switching my brain over. But Colab especially doesn't have a convenient flow for this.

Sure you can write
```Python
assert foo() == 1
```

but when that fails you get just a useless `AssertionError` with no context or the actual value.

So I made a small example [Colab notebook](https://colab.research.google.com/drive/1l5qmIjwV2wYM4nL0tnWLdJaVqgQIGoI5) that does just enough to set up a `unittest.TestCase` and call it, and work around some of the quirks in there.

Some commentary here:

```Python
import unittest
class AlexIsInterviewing(unittest.TestCase):
    def test_all(self):
        self.assertEqual(1, 2)
        with self.assertRaises(ValueError):
            raise ValueError()

# Kinda nice to be able to explicitly run a test this easily
AlexIsInterviewing().test_all()
# This is how I'd usually invoke this though
unittest.main(argv=[''], verbosity=2, exit=False)
```

and now we get a nice backtrace and `AssertionError: 1 != 2`

The other thing I reach for, especially for the kinds of problems that are used in interviews, is property testing, which, in Python, often means [Hypothesis](https://hypothesis.readthedocs.io/en/latest/).

I'm mostly calling that out because I had some additional issues getting proper failures printed in a notebook environment. With the latest Hypothesis, if multiple errors occur, it prints

```ExceptionGroup: Hypothesis found 2 distinct failures. (2 sub-exceptions)```

but without reporting the sub-exceptions. I'm working around this by just pinning to an old version (6.53.0) before Hypothesis [moved to use `ExceptionGroup` for this](https://hypothesis.readthedocs.io/en/latest/changes.html#v6-54-0). If anyone knows what's going on there, holler at me. It was not high on my list of things to debug. [Issue #3430](https://github.com/HypothesisWorks/hypothesis/issues/3430) looked so promising too!
