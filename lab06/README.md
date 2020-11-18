# PTarefa de Cypher e o FDA Adverse Event Reporting System (FAERS)
## Exercício 1

Escreva uma sentença em Cypher que crie o medicamento de nome `Metamizole`, código no DrugBank `DB04817`.

### Resolução
```
create (:Drug {drugbank: "DB04817", name: "Metamizole"})
```

## Exercício 2

Considerando que a Dipyrone e Metamizole são o mesmo medicamento com nomes diferentes, crie uma aresta com o rótulo `:SameAs` que ligue os dois.

### Resolução
```
MATCH (d1:Drug)
MATCH (d2:Drug)
WHERE d1.drugbank = "DB04817" AND d1.name <> d2.name and d2.drugbank = "DB04817"
create (d1)-[:SameAs]->(d2)
```

## Exercício 3

Use o `DELETE` para excluir o relacionamento que você criou (apenas ele).

### Resolução
```
match (:Drug {drugbank:"DB04817"})-[e:SameAs]->(:Drug {drugbank:"DB04817"})
delete e
```

## Exercício 4

Faça a projeção em relação a Patologia, ou seja, conecte patologias que são tratadas pela mesma droga.

### Resolução
```
MATCH (p1:Pathology)<-[a]-(d:Drug)-[b]->(p2:Pathology)
MERGE (p1)<-[r:Projection]->(p2)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
```

## Exercício 5

Construa um grafo ligando os medicamentos aos efeitos colaterais (com pesos associados) a partir dos registros das pessoas, ou seja, se uma pessoa usa um medicamento e ela teve um efeito colateral, o medicamento deve ser ligado ao efeito colateral.

### Resolução

Criando os nós de Pessoa
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
CREATE (:Person {code: line.idperson})
```
```
CREATE INDEX ON :Person(code)
```

Criando os nós de Pessoa
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
CREATE (:Person {code: line.idperson})
```

## Exercício 6

Que tipo de análise interessante pode ser feita com esse grafo?

Proponha um tipo de análise e escreva uma sentença em Cypher que realize a análise.

### Resolução
```
teste
```
