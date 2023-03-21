# some good queries

```
❯ curl --silent -X 'GET' -H 'accept: application/json' 'https://renkulab.io/knowledge-graph/projects/almut.lue/test-data-import/datasets' | jq
[
  {
    "_links": [
      {
        "rel": "details",
        "href": "https://renkulab.io/knowledge-graph/datasets/1863b68527034bf6b38d6d89d6f28c59"
      },
      {
        "rel": "initial-version",
        "href": "https://renkulab.io/knowledge-graph/datasets/1863b68527034bf6b38d6d89d6f28c59"
      },
      {
        "rel": "tags",
        "href": "https://renkulab.io/knowledge-graph/projects/almut.lue/test-data-import/datasets/update_dataset/tags"
      }
    ],
    "sameAs": "https://renkulab.io/datasets/a554389a7f714b2a8c0f80e39c873d58",
    "identifier": "1863b68527034bf6b38d6d89d6f28c59",
    "versions": {
      "initial": "1863b68527034bf6b38d6d89d6f28c59"
    },
    "title": "update_dataset",
    "name": "update_dataset",
    "images": []
  },
  {
    "_links": [
      {
        "rel": "details",
        "href": "https://renkulab.io/knowledge-graph/datasets/c67ed125d30644a9b8e799915d7bb7fa"
      },
      {
        "rel": "initial-version",
        "href": "https://renkulab.io/knowledge-graph/datasets/a6eed8efb1c240c68cab72cec26b3316"
      },
      {
        "rel": "tags",
        "href": "https://renkulab.io/knowledge-graph/projects/almut.lue/test-data-import/datasets/update_dataset/tags"
      }
    ],
    "derivedFrom": "https://renkulab.io/datasets/a6eed8efb1c240c68cab72cec26b3316",
    "identifier": "c67ed125d30644a9b8e799915d7bb7fa",
    "versions": {
      "initial": "a6eed8efb1c240c68cab72cec26b3316"
    },
    "title": "update_dataset",
    "name": "update_dataset",
    "images": []
  }
]
```


```
❯ curl --silent -X 'GET' -H 'accept: application/json' 'https://renkulab.io/knowledge-graph/datasets/1863b68527034bf6b38d6d89d6f28c59' | jq
{
  "_links": [
    {
      "rel": "self",
      "href": "https://renkulab.io/knowledge-graph/datasets/1863b68527034bf6b38d6d89d6f28c59"
    },
    {
      "rel": "initial-version",
      "href": "https://renkulab.io/knowledge-graph/datasets/1863b68527034bf6b38d6d89d6f28c59"
    },
    {
      "rel": "tags",
      "href": "https://renkulab.io/knowledge-graph/projects/almut.lue/test-data-import/datasets/update_dataset/tags"
    }
  ],
  "identifier": "1863b68527034bf6b38d6d89d6f28c59",
  "name": "update_dataset",
  "title": "update_dataset",
  "url": "https://renkulab.io/datasets/1863b68527034bf6b38d6d89d6f28c59",
  "sameAs": "https://renkulab.io/datasets/a554389a7f714b2a8c0f80e39c873d58",
  "versions": {
    "initial": "1863b68527034bf6b38d6d89d6f28c59"
  },
  "published": {
    "creator": [
      {
        "name": "Almut Luetge",
        "email": "almut.lue@gmail.com"
      }
    ]
  },
  "created": "2023-03-15T12:25:02Z",
  "hasPart": [
    {
      "atLocation": "data/update_dataset/test.txt"
    }
  ],
  "project": {
    "_links": [
      {
        "rel": "project-details",
        "href": "https://renkulab.io/knowledge-graph/projects/almut.lue/test-data-import"
      }
    ],
    "path": "almut.lue/test-data-import",
    "name": "test_data_import",
    "visibility": "public"
  },
  "usedIn": [
    {
      "_links": [
        {
          "rel": "project-details",
          "href": "https://renkulab.io/knowledge-graph/projects/almut.lue/test-data-import"
        }
      ],
      "path": "almut.lue/test-data-import",
      "name": "test_data_import",
      "visibility": "public"
    }
  ],
  "keywords": [],
  "images": []
}
```

notes:

- 1. this query replaces the graphql endpoint
- 2. this query seems to be quite slow - will not scale to many files!!!
- 3. do *not* use swagger example api - it's urlencoding the namespace.

❯ time curl --silent -X 'GET' -H 'accept: application/json' 'https://renkulab.io/knowledge-graph/projects/omnibenchmark/omni_clustering/adjusted-rand-index/files/data%2Fascend-clustering%2Fascend-clustering_filter_m3drop_f2a4d_trapnelltcc__k_9__seed_87619_cluster_res.json/lineage' | jq > lineage.json
curl --silent -X 'GET' -H 'accept: application/json'   0.02s user 0.00s system 0% cpu 11.089 total
jq > lineage.json  0.03s user 0.00s system 0% cpu 11.103 total

