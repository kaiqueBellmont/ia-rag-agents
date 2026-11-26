# GUIA: Bancos vetoriais (semana 2, 21/12 a 27/12)

## Como seguir
1. Monte um conjunto de 100 mil+ trechos (público em português ou seu corpus ampliado). Guarde a verdade-terreno exata (FAISS Flat).
2. FAISS: Flat → IVF (nlist, nprobe) → HNSW (M, efConstruction, efSearch). Gere a curva recall × latência.
3. pgvector: carregar, criar índice hnsw e ivfflat, EXPLAIN ANALYZE, mesma curva.
4. Qdrant: coleção, payload, filtro seletivo, quantização escalar.
5. Quantização: SQ, PQ e binária; meça memória e recall.
6. Tabela final: recall@10, latência p50 e p95, memória, tempo de construção, comportamento com filtros.

## Conceitos para dominar
- Por que busca exata não escala; trade-off recall × latência × memória.
- IVF: clusters e nprobe. HNSW: camadas, M, efConstruction, efSearch.
- Filtros antes vs depois da busca e por que o recall cai com filtro muito seletivo.
- Memória: n × d × 4 bytes (float32); int8 divide por 4; binária por 32.
- pgvector vs banco dedicado: operação, consistência, escala, custo.
- Atualização e remoção em índices ANN.

## Exercícios sem IA
- Leia o paper do HNSW e explique construção e busca por camadas no papel.
- Calcule a memória para 50M vetores de 1024 dimensões em float32, int8 e binário (aprox. 205 GB, 51 GB e 6,4 GB).

## Pronto quando
- Tabela dos 3 bancos com os números acima, texto de quando escolher cada um, desenho do HNSW feito à mão.

## Armadilhas
- Medir recall sem verdade-terreno exata; reportar latência média em vez de p95; criar índice ivfflat antes de carregar os dados; não aquecer cache; comparar em condições diferentes.

## Entrevista sênior (uma resposta forte cobre...)
1. pgvector ou banco dedicado? Escala, filtros, operação, equipe, custo, consistência com os dados relacionais.
2. Estime a memória de 50M vetores de 1024 dim e como reduzir. Conta acima, quantização, Matryoshka, índice em disco.
3. Como o filtro afeta o recall do HNSW? Grafo filtrado desconecta; estratégias de filtro e índices de payload.
4. Como escolher efSearch e M? Curva recall × latência medida com verdade-terreno.
5. Como lidar com atualização e remoção? Marcação, reconstrução, compactação.

## Leituras
Paper do HNSW; documentação do pgvector, Qdrant e FAISS.
