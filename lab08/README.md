# Título

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
