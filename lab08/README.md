# Modelo para Apresentação do Lab08 - PubChem e DRON com XQuery/XPath

## Questão 1

Construa uma comando SELECT que retorne dados equivalentes a este XPath
```
//individuo[idade>20]/@nome
```

Resolução
```
SELECT nome FROM individuo WHERE idade >20
```

## Questão 2

Qual a outra maneira de escrever esta query sem o where?

```
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')
 
for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
```
 
Resolução
 ```
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')
 
for $i in ($fichariodoc//individuo[idade>17])
return {data($i/@nome)}
 ```
 
## Questão 3

Escreva uma consulta SQL equivalente ao XQuery:

```
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')

for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
```

Resolução
 
```
SELECT nome FROM individuo WHERW idade>17
```

## Questão 4

Retorne quantas publicações são posteriores ao ano de 2011.

Resolução
```
let $publications:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')

for $p in ($publications/publications/publication)
let $ano := {data($p/year)}
where $ano > 2011
group by $ano
order by $ano
return {'Livros -- ano: ', $ano, 'quantidade: ', count($p), '&#xa;'}
```

## Questão 5

Retorne a categoria cujo <label> em inglês seja 'e-Science Domain'.

Resolução
```
let $publications:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')

for $c in ($publications/publications/categories/category)
let $l:= {data($c/label)}
where $l = 'e-Science Domain'
return $c
```

## Questão 6

Retorne as publicações associadas à categoria cujo <label> em inglês seja 'e-Science Domain'. A associação entre o label e a key da categoria deve ser feita na consulta.

Resolução
```
let $publications:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')

for $p in ($publications/publications/publication),
    $c in ($publications/publications/categories/category)

let $l:= {data($c/label)}

where $l = 'e-Science Domain' and $p/key=$c/@key

return {data($p/title), '&#xa;'}
```

# Tarefas com DRON e PubChem

## Questão 1

Liste todas as classificações que estão dois níveis abaixo da raiz

Resolução
 
```
let $dron:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017-dron/dron.xml')

for $n in ($dron/drug/drug/drug//@name)
return {data($n), '&#xa;'}
```

## Questão 2

Apresente todas as classificações de um componente a sua escolha (diferente de Acetylsalicylic Acid) que estejam hierarquicamente dois níveis acima. Note que no exemplo dado em sala foi considerado um nível hierárquico acima.

Resolução
 
```
let $dron:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017-dron/dron.xml')

for $d in ($dron//drug[drug/drug/@name='INSULIN GLARGINE'])
let $parent := $d/@name
group by $parent
order by $parent
return {data($parent), '&#xa;'}
```

## Questão 3

Dado o dataset integrado criado via acesso a serviços no Jupyter. Este notebook tem os nomes (e sinônimos) de componentes recuperados do PubChem, em conjunto com o ChEBI do mesmo.

O dataset está disponível em:

https://github.com/santanche/lab2learn/blob/master/data/pubchem/pubchem-chebi-synonyms.xml

Deve ser recuperado em XQuery no endereço:

https://raw.githubusercontent.com/santanche/lab2learn/master/data/pubchem/pubchem-chebi-synonyms.xml

### Questão 3.1

Liste todos os códigos ChEBI dos componentes disponíveis.

Resolução
 
```
let $sinonimos:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/pubchem/pubchem-chebi-synonyms.xml')

for $c in ($sinonimos/PC-DataSet/InformationList/Information/Synonym[1])

return {data($c), '&#xa;'}
```

### Questão 3.2
Liste todos os códigos ChEBI e primeiro nome (sinônimo) de cada um dos componentes disponíveis.

Resolução
```
let $sinonimos:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/pubchem/pubchem-chebi-synonyms.xml')

for $c in ($sinonimos/PC-DataSet/InformationList/Information/Synonym[1]),
    $s in ($sinonimos/PC-DataSet/InformationList/Information/Synonym[2])

return {data($c),',', data($s), '&#xa;'}
```

### Questão 3.3
Para cada código ChEBI, liste os sinônimos e o nome que aparece para o mesmo componente no DRON (se existir). Para isso faça um JOIN com o DRON.

Resolução
```
let $sinonimos:= doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/pubchem/pubchem-chebi-synonyms.xml')
let $dron := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017-dron/dron.xml')

for $s in ($sinonimos/PC-DataSet/InformationList/Information/Synonym[1]),
    $d in ($dron//drug)
where concat('http://purl.obolibrary.org/obo/CHEBI_',substring($s/text(), 7)) = $d/@id

return { data($d/@id),',', data($s), '&#xa;'}
```
