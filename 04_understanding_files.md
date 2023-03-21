# what is in a file?

Trying to understand the relationship between:

- renku (view of a) dataset
- files in a dataset
- versions of a dataset

From some chats:

"identifier is more important than hasPart to univocally point to each dataset (version). For listing/discovery, renku has renku dataset commands wich might be relevant (list and import), within a renku project, and omnibenchmark has an extra layer for that for across-repos-queries, i.e. renku_commands/datasets or management/data_commands . (Just to say, renku import works with identifiers directly, I think renku dataset import https://renkulab.io/datasets/<ID> might work) "


## Test example

(kindly made by almut to aid in understanding relations).

### 1. Create new project `test-data-updates`

* https://renkulab.io/gitlab/almut.lue/test-data-updates
* there's a new dataset "update_dataset", linked a file `test.txt`

### 2. Create 2nd project `test-data-import`

* https://renkulab.io/gitlab/almut.lue/test-data-import
* imports the first dataset: `update_dataset`

Note: renku is using something derived from ZODB, the ancient zope object database.
If we peek under the hood, the "objects" in .renku/metadata contain interesting metadata:

https://renkulab.io/gitlab/almut.lue/test-data-import/-/blob/master/.renku/metadata/datasets-provenance-tails

this looks like a BTree of the provenance of files. it seems that each import adds an entry to the tree - but this seems to be "project"-centric.

the question remains: what is the project-agnostic high level view of a "Dataset"?


the `datasets` file:

* [original](https://renkulab.io/gitlab/almut.lue/test-data-updates/-/blob/master/.renku/metadata/datasets)
* [imported](https://renkulab.io/gitlab/almut.lue/test-data-import/-/blob/master/.renku/metadata/datasets)

some things to consider:

renku_data_value != renku_oid

there's an extra attribute in the tree, renku_reference = true

there's something called a `renku_oid`:

@renku_data_type": "renku.domain_model.dataset.Dataset"

* orig:   33fc9c5a9a705e879c6f78701711047e503895b497fda5c3887682b2427a0f66
* import: 0ea9916cc3262a4f10ffc3bad1b22b187a04833d997565e993af2ca1c23e11b0

but te 'provenance-tails' tree structure seem to "mutate" the id for the original provenance when importing, see:


original:
```
"@renku_data_value": [
  "/datasets/9e5253e1f16940ff8d3025f1acea31ad",
{"@renku_data_type": "renku.domain_model.dataset.Dataset",
 "@renku_oid": "33fc9c5a9a705e879c6f78701711047e503895b497fda5c3887682b2427a0f66",            "@renku_reference": true}]
```

the imported one:

```
"@renku_data_value": [
  "/datasets/1863b68527034bf6b38d6d89d6f28c59",
  {
	"@renku_data_type": "renku.domain_model.dataset.Dataset",
	"@renku_oid": "0ea9916cc3262a4f10ffc3bad1b22b187a04833d997565e993af2ca1c23e11b0", // <<- this is the import oid above.
	"@renku_reference": true
  },
  "/datasets/751bbcf3f9174b1b8d04b70cca8b05f8",
  {
	"@renku_data_type": "renku.domain_model.dataset.Dataset",
	"@renku_oid": "b67737b1f13036dc298a4875fb32ed507be2f192c8720358ac9fe32fe3cef6fd",
	"@renku_reference": true
}
```


The "ids" of the dataset have changed:

a.) Data generation: 3a22a854de9e47a38390ed0eed9e3097
b.) Adding file: a554389a7f714b2a8c0f80e39c873d58
c.) Importing into new project:  1863b68527034bf6b38d6d89d6f28c59
d.) Update file in original project: 9e5253e1f16940ff8d3025f1acea31ad


This is interesting! (and puzzling, and a little bit messy). This is a partial tree

a.) is an empty node
b.) derives from a.; it has a file.
c.) is its own origin (new project), but has a sameAs relationship to b.; it also has initial relation to itself.
d.) is marked as derived from c., but lists a. as initial

b-d. keep the same `hasPart` attribute (a partial data/@/test.txt descriptor, that afaik needs the project to be dereferenced)


Tehre's some code in `renku.core.dataset` that might be useful (add datadir_files_to_dataset)

* import_dataset
* calculate_total_size
* importer.is_latest_version()


* Dataset providers are very cool! api, cloud, dataverse, doi, git...
* Is obm limited to renku/git dataset providers? is it useful to import by something else?
* It seems to me some of the dataset "problems" could perhaps be "solved" by using a different dataset provider - canonical repo, sthing else? has this avenue been explored in the context of omnibenchmark?

