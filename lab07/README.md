## Tarefa de análises feitas no Cypher

## Exercício 1

Calcule o Pagerank do exemplo da Wikipedia em Cypher:

~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/network/pagerank/pagerank-wikipedia.csv' AS line
MERGE (no1:page {name:line.source})
MERGE (no2:page {name:line.target})
CREATE (no1)-[:liga]->(no2)

CALL gds.graph.create('grafo_pagerank','page','liga')

CALL gds.pageRank.stream('grafo_pagerank')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).name AS name, score
ORDER BY score DESC, name ASC
~~~

## Exercício 2

Departing from a Drug-Drug graph created in a previous lab, whose relationship determines drugs taken together, apply a community detection in it to see the results:

~~~cypher
CALL gds.graph.create('comunidade','droga','uso_conjunto')

CALL gds.louvain.stream('comunidade')
YIELD nodeId, comunidadeId

RETURN gds.util.asNode(nodeId).id AS nid, comunidadeId
ORDER BY nid DESC
~~~
