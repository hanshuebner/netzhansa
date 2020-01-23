---
title: "C++ debugging output"
description: "Using a nullstream for debug output"
date: 2020-01-23T05:37:54+01:00
draft: true
tags: [C++, code]
---

I like to write debugging information to an ostream instead of calling
a logging function.

```
 retry:
  if (write(_fd, request.buffer(), request.length()) != request.length()) {
    if (errno == EAGAIN) {
     _debug << "retry" << endl;
      goto retry;
    }
    throw system_error(errno, generic_category(), "Could not write to serial port");
  }
```

By default, I don't want to see debugging information, so I initialize
my debugging ostream to a stream that sinks all data written to it:

```
#pragma once

#include <iostream>

class nullstream : public std::ostream {
public:
  nullstream() : std::ostream(nullptr) {}
  nullstream(const nullstream &) : std::ostream(nullptr) {}
};

template <class T>
const nullstream &operator<<(nullstream &&os, const T &value) { 
  return os;
}
```

I can then set the ostream to cerr or another stream if I want to see
the debugging information.  A downside of the approach is that the
debug information will be formatted even if not debugging, so it is
not suitable for high performance code.
