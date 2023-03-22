# dissecting omni gitlab runs

I'm trying to understand actual execution of flows.

I'm not sure how much it's inherited from renku tbh.

## stages

there seem to be two stages on all runs: `image_build` and `run_update`.

* `image_build`:  when not trigged by a pipeline.
* `run_udpate`: when triggered by a pipeline.

I think the key is that build does a docker-in-docker build of the repo Dockerfile, and pushes it to the registry. Still not clear on why the short-id tag.

And then `run_update` fetches the image (with updated dependencies), clones the
repo (probably to get all the renku metadata stuff), installs lfs and runs the entrypoint.

## improvements?

* I *suspect* code could use `COPY` in the build stage (rather than git clone
  in the run)? but maybe this makes the images too fat?
* omni could perhaps use its own base image derived from upstream (and install any
  common stuff in there). this will make updating to the same renku version at
once much easier, probably.
* `git lfs` could perhaps be installed in this base image.
* can we pass `ARG RENKU_VERSION=1.11.2` on the base image? (are args inherited?).

Example: https://renkulab.io/gitlab/omnibenchmark/iris_example/iris-random-forest/-/blob/master/Dockerfile

## open questions

* are the versions of `$stuff` relevant? where are they captured? the registry
  will have an expiry date. in theory, a build should be reproducible with
  `base-version` + `system-packages` + `python-packages` versioned.
* renku-version is probably relevant too.

## pointers

I think some improvements can be made to execution.
https://www.digitalocean.com/community/tutorials/how-to-build-docker-images-and-host-a-docker-image-repository-with-gitlab#step-3-%E2%80%94-updating-gitlab-ci-yaml-and-building-a-docker-image
