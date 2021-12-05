# VFPL

**NOTE: VFPL is very much work in progress and several major features are still missing**

VFPL is formal, politely verbose programming language for building next-gen reliable applications.

It does not aim to be the most expressive and simple, but attempts to scale no matter how large the codebase and ambitions of the programmer
are. It's main target is the large enterprise audience, but it is a good fit for smaller programs as well.

VFPL is imperative and statically and strongly typed (although the current implementation is dynamic). It is also null safe and has 4
different null values (`absent`, `null`, `undefined` and `novalue`), because not every absence is created equal.

The hello world looks like this:

```vfpl
please call println with the argument "Hello, World!" as x.
please go to sleep.
```

VFPL is not case sensitive, but there are several [conventions](./best-practices.md) around naming.

Every function must explicitly return a value, `absent` is the recommended value if there is none returned, but the other null values can be
used as well.

The program must also always be terminated by a `please go to sleep.`.