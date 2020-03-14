---
title: "Splitting strings by regular expression in C++"
date: 2020-01-26T09:37:44+01:00
tags: ["c++", "code"]
---

In many cases, strings are not the right data structure, but in some
they are.  I need to parse a command line into words, and as I'm
writing C++, I went shopping for the equivalent of `split` as it is
available in every other language.  Not so in C++.  The question,
obviously, has been asked a hundred times on Stackoverflow, and there
is a [canonical blog
post](http://www.martinbroadhurst.com/how-to-split-a-string-in-c.html)
describing 10 different ways how to split a string.  I like [this
solution from
Stackoverflow](https://stackoverflow.com/a/28142357/928528) best and
adapted it so that I don't get empty elements in my result vector:

```c++
#include <regex>

vector<string>
split(const string& s) {
  vector<string> elems;

  regex re("\\s+");

  sregex_token_iterator iter(s.begin(), s.end(), re, -1);
  sregex_token_iterator end;

  while (iter != end) {
    if (iter->length()) {
      elems.push_back(*iter);
    }
    ++iter;
  }

  return elems;
}
```

