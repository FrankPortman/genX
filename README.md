# genX
Robust push/pull/graph generator primitives for Python

## Introduction
Python supports (lazy) generator expressions and functions:

```python
nums = (i for i in range(100))
evens = (i for i in nums if i % 2 == 0)
squared_evens = (i * i for i in evens)

print(list(squared_evens)[0:5])
```
![image](https://user-images.githubusercontent.com/2442871/43431199-8473fac2-9421-11e8-89e6-1ba82b301bf0.png)

This is useful because we only iterate over each value a single time while maintaining composability and expresiveness of the individual components. At the end of this code block all of the generators have been depleted.

There are, however, some limitations. First consider the case of `evens` needing to be passed into 2 expressions rather than just one.

```python
nums = (i for i in range(100))
evens = (i for i in nums if i % 2 == 0)
squared_evens = (i * i for i in evens)
cubed_evens = (i * i * i for i in evens)

print(list(squared_evens)[0:5])
print(list(cubed_evens))
```
![image](https://user-images.githubusercontent.com/2442871/43431554-35340b9e-9423-11e8-8a63-e415c6c70552.png)

Oops! The expressions are lazy but not smart enough to save each iteration for both `squared_evens` and `cubed_evens`. There are some workarounds of course but you start to lose out on the composability benefits depending on what you do.

Another limitation is that traditional generator expressions rely on a `pull` model, i.e. you are `pull`ing results from some sort of iterable. They cannot easily address a `push` model, or when you want to `push` a value or iterable of values into a lazily linked computational chain.

This library attempts to address all 3 use cases with as intuitive of a syntax as regular Python generators.

Implementation is heavily inspired by the concept of [transducers](https://clojure.org/reference/transducers) (most notably from Clojure). This is a somewhat opionated implementation of just some of the concepts of transducers + some directed graph niceties. For heavy duty applications you may want to consider a proper flow-based DAG such as Luigi.

Other projects doing something similar:
- https://github.com/cognitect-labs/transducers-python
- https://github.com/sixty-north/python-transducers
