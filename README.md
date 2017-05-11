# Memory model for OCaml (herd formalisation)

This repo contains the memory model for Multicore OCaml, formalised
using [herdtools7](https://github.com/herd/herdtools7/). Unless you're
into axiomatic memory models, you might prefer to read the
[high-level overview on the Multicore OCaml wiki](https://github.com/ocamllabs/ocaml-multicore/wiki/Memory-model).


To play with it, you'll need to install herd and generate the litmus
tests:

    opam install herdtools7
    ./generate-litmus-tests

The litmus tests (both the manually written and auto-generated ones)
are in `LISA` format, which is a simple assembly language that's part
of `herdtools7`. For OCaml, we use the `[a]` and `[n]` attributes on
reads and writes to distinguish atomic and nonatomic accesses
(i.e. `Atomic.get/set` and ordinary `ref` cells). (See the
`ocaml.bell` file where these are defined).

You can run the model with

    ./run-model

which will print the results of simulation with the contents of the
`litmus` directory. There are some alternative modles in `alt-models`,
in varying states of decay. It might be interesting to compare them
with `ocaml.cat`:

    diff -u <(./run-model) (./run-model alt-models/ocaml-corr.cat)

If you're interested in a particular litmus test, you can use `herd`
to visualise a particular execution:

    herd7 -web -bell ocaml.bell -model ocaml.cat \
      -show prop -evince litmus/auto/W+RWC+pona+poan+ponn.litmus
