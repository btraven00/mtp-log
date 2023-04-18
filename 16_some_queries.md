# a few queries to the local graph

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?s ?p ?o WHERE {
  ?s ?p <https://localhost/activities/8628234b7bad4fddaa590f7f5ae178e9> .
} LIMIT 200
```

# filter + xsd

```
SELECT Distinct ?TypeLabel Where { 
?a foaf:name ?name . 
?a rdf:type ?Type . 
?Type rdfs:label ?TypeLabel . 
FILTER (?name="South"^^xsd:string)
}
```

# generation, activity

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX prov: <http://www.w3.org/ns/prov#>

SELECT DISTINCT ?s ?o WHERE {
  ?s prov:activity ?o .
} LIMIT 200
```

# qualified generation

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX prov: <http://www.w3.org/ns/prov#>

SELECT * WHERE {
 ?s ?p <https://localhost/activities/8628234b7bad4fddaa590f7f5ae178e9/generations/3d10c006252e4610bd4e8a152df51e39> .
} LIMIT 10
```
