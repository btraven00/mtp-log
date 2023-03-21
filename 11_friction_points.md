# workflow friction points

## 1. dataset origin

renku primitives are immutable, but when doing cross-project references it's not trivial to find the authoritative ancestor for a given dataset.

I think part of the "problem" is that the ZODB used is always references within
the boundaries of a single project, and each git repo has its own
lineage tree for provenance. Since methods import datasets, one has to traverse
`sameAs` relations (Almut code does this, and it works, but it seems messy).

On the other hand, there's no way to automatically propagate changes in the
dataset "downwards" to methods that use the dataset - or to check that a method
is using an outdated version of a dataset.

We can always rely on traversing the knowledge graph, but I suspect that the
dataset-ID inflatino will make this process cumbersome (and manual checks
against versions of a dataset need always to be done additionally).

An interesting side-problem is how to properly dereference a known dataset.
Files are specified in the Knowledge Graph as `hasPart` properties (and the
path seems to encode the project name), but this seems brittle as far as
dereferencing goes (where is the commit id?).


## 2. For a given method, dependencies on inputs (and pre-processing steps) seem loosely structured

* The only constrains so far seem to be on metrics and outputs (json schemas in `omniValidator`).
* It seems "hard" for method authors to navigate syntax (keywords, types)
* There seemst to be an abundance of ad-hoc validation checks (logic is encoded on string manipulation of keywords).

## 3. Data flow for an immutable dataset should be made explicit to git commits

Currently, we do not capture which commit is used for I/O - or which version of a software did produce a given output.

Further work on provenance should take this into account.
