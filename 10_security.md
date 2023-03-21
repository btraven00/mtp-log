# security observations

In https://renkulab.io/gitlab/omb_benchmarks/omni_srt_clustering/method_clustermap/-/blob/master/.gitlab-ci.yml

```
  before_script:     - docker login -u gitlab-ci-token -p $CI_JOB_TOKEN http://$CI_REGISTRY
```
plain http is being used. the gitlab token is traveling in plain text. is this happening internally in renkulab gitlab infra?
