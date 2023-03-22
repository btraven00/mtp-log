# 1. the problem of indirection

I think other ways of referencing/importing datasets could be tried.

- store pointers to canonical dataset: the "data" stored in a repo could be a
  link to a dataset (so that the first step in any benchmark would be to "renku-import" it)
- no changes on parent should not produce any update
- we could just be oblivious to any "lineage" manipulation that the object
  database makes on the imported datasets.
- git subtrees
- recursive git-lfs fetch for the files?
- files could/should be checksummed
- use snapshots?


# 2. data pipeline validation

there are projects like [pandera](https://pandera.readthedocs.io/en/stable/index.html)

another library in R [data.validator](https://github.com/Appsilon/data.validator)
