# Exercício 1

Escreva uma sentença em Cypher que crie o medicamento de nome `Metamizole`, código no DrugBank DB04817.

```
create (:Drug {drugbank: "DB04817", name: "Metamizole"})
```

# Exercício 2

Considerando que a Dipyrone e Metamizole são o mesmo medicamento com nomes diferentes, crie uma aresta com o rótulo `:SameAs` que ligue os dois.

```
MATCH (d1:Drug)
MATCH (d2:Drug)
WHERE d1.drugbank = "DB04817" AND d1.name <> d2.name and d2.drugbank = "DB04817"
create (d1)-[:SameAs]->(d2)
```

# Exercício 3

Use o `DELETE` para excluir o relacionamento que você criou (apenas ele).

```
match (:Drug {drugbank:"DB04817"})-[e:SameAs]->(:Drug {drugbank:"DB04817"})
delete e
```

