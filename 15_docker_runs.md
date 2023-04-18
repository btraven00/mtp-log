# typeguard

there's a backward-incompatible change. should update

added to requirements:

typeguard<3.0.0

# renku version

should upgrade to latest renku on all data + method images.

renku migrate on images, remove old pinnings etc.

# docker workflow

* the whole usage of docker build + update sounds a bit weird to me.
* to me it sounds a little bit more natural to build base images from docker-base (from renku project), and install any needed dependencies there
* instead of _cloning_ the repo each time, it seems better to add a `COPY . .` step, isn't it?

# my tests

For a given image, first build the "regular" docker image:

```
docker build -t renku-csf .
```

Then use the "extended" dockerfile:

```
docker build -t renku-csf-ext -f dockerfile-extend .
```

such dockerfile has:

```
FROM renku-csf
ENV DEBIAN_FRONTEND=noninteractive
USER root
RUN apt-get update && apt-get install -y \
  git-lfs \
  && rm -rf /var/lib/apt/lists/*

COPY . work
COPY .git/ ./work/.git/
RUN chown -R ${NB_USER} ./work
RUN mkdir /graph && chown -R ${NB_USER} /graph
USER ${NB_USER}

RUN pip install -U pip

ENTRYPOINT [ "/bin/bash" ]
```

Now for testing:

```
❯ docker run --rm -it -v "./graph:/graph" -w "/graph" -e OMNICLI_GRAPH_PATH=/graph renku-csf-ext
```


