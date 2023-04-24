# dataset dependency

this document documents explorations around dataset dependency.


## problem

* the renku oid identifies datasets but has no concept of inter-project linkage
* lineage gets complicated (or impossible) to track files across different projects (origin dataset, filtered, methods that take it as input)
* [This issue
  (renku-python#706)](https://github.com/SwissDataScienceCenter/renku-python/issues/706#issuecomment-589156013)
captures a similar requirement, and some ideas are discussed there (explicit
vs. implicit datasets, permanent vs. ephemeral datasets)


## desired behavior

* It'd be nice to be able to declare "dataset" dependencies at the benchmark-project level, by referring to a single dataset origin (git).

## exploratory design

* there are 3 types of omnibenchmark projects: origin-dataset, filtered-dataset, and method-dataset:

  * origin-dataset: source of truth. generated from a script, imported via DOI/zenodo etc.
  * filtered-dataset: they run a filtering script, and store output in its own dataset.
  * methods: can take origin-dataset as input, or filtered-dataset. They can run against its current state of files, but they also MUST display a warning if the upstream dataset has been updated (via git revisions). each method stores output in its own method-dataset
  * metrics for the run are collected by taking HEAD revision from each method-dataset.

* We can define dataset dependencies, pointing to the source-of-truth. this is a renku dataset, but imported from git.
* `omni-cli` can **initialize** and **update** the project dataset. if workflow is run from an outdated dataset, a warning must be given. If run in "strict" mode (ie, from the pipeline), workflow must be aborted
* Any changes to the source dataset **MUST** trigger a run of the filtering datasets that depend on it. A DAG of the data pipeline should be easy to obtain.

## lineage

* I think it's not currently possible to track the lineage for projects that import a dataset (same-as is tricky, and convoluted).
* Annotating data imports with the original dataset repo commit might be a good heuristic (to ensure all filters and methods are running from the same revision of the dataset)


## implementation note

* omnibenchmark currently updates the inputs in here: https://github.com/omnibenchmark/omnibenchmark-py/blob/main/omnibenchmark/core/omni_object.py#L183
* this basically finds the dataset (by keyword) and updates stuff
* a simple modification is to annotate each activity run with the upstream dataset revision
