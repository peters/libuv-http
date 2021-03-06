# SYNOPSIS
A simple http server backed by [`libuv`](https://github.com/joyent/libuv) 
and [`http-parser`](https://github.com/joyent/http-parser).

# EXAMPLE

```cpp
#include "http.h"

int main() {

  Server hs([](auto &req, auto &res) {
    
    res.setStatus(200);
    res.setHeader("Content-Type", "text/plain");
    res.setHeader("Connection", "keep-alive");
    res << req.method << " " << req.url << endl;
 
  });
  
  hs.listen("0.0.0.0", 8000);
}
```

# PERFORMANCE

Just for fun. Without any real statistical significance, here are 
some quick averages from apache ab, run on an old macbook air. Also
neat is that `libuv-http` uses about `400KB` of memory compared to 
Node.js' `10-15MB`.

### libuv-http
```
Requests per second:    16333.46 [#/sec] (mean)
```

### node.js
```
Requests per second:    3366.41 [#/sec] (mean)
```

# DEVELOPMENT

## REQUIREMENTS

- clang 3.5
- gyp
- leveldb
- libuv
- http-parser

## REQUIREMENTS (Windows)

```
git clone https://github.com/joyent/libuv.git deps/libuv
cd deps/libuv 
git checkout v0.11.29
cd..

git clone https://github.com/joyent/http-parser.git deps/http-parser
cd deps/http-parser  
git checkout v2.3
cd ..

gyp --depth=. --generator-output=build -Duv_library=shared_library
```

## DEBUGGING

### VALGRIND

```bash
valgrind --leak-check=yes --track-origins=yes --dsymutil=yes ./server
```

## BUILD

```bash
make
```

