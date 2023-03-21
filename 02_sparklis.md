# docker pull sferre/sparklis:latest

much fun ensues...

(CORS problem to reach the repo at localhost). 

let's try to build sparklis locally

sudo pacman -Sy ocaml-findlib
sudo pacman -Sy camlp5

opam init
eval $(opam env)
opam --version
// arch opam doesnt't seem to like passing lists of packages. so, one by one
opam install core
opam install csv
opam install lwt
opam install js_of_ocaml
opam install js_of_ocaml-lwt
opam install lwt_ppx


Now run a server

go:
simplehttpserver

We need to run also a CORS local proxy:

npm install -g local-cors-proxy
lcp --proxyUrl http://localhost:7878


Now open:


udo pacman -Sy ocaml-findlib
sudo pacman -Sy camlp5
http://localhost:8010/proxy/query
