# Exploring tuples from oxigraph

## Install server

I'm going to use this rust server - has nice bindings, and it's supposed to be fast.

```
cargo install oxigraph_server 
```

Takes a while to compile.

## Load triples

```
oxigraph_server --location OXIGRAPH_DATA load --file RAW/omni_clustering_data.trig 
1743432 triples loaded in 7s (218989 t/s) from RAW/omni_clustering_data.trig
```

## Serve

```
oxigraph_server --location OXIGRAPH_DATA serve 
Listening for requests at http://localhost:7878
```


