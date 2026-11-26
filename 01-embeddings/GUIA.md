# GUIA: Embeddings (semana 1, 14/12 a 20/12)

## Como seguir
1. Gere embeddings locais do corpus e salve em disco (nunca recompute sem necessidade).
2. Escreva cosseno e k-NN em numpy em lote (matriz normalizada, produto matricial, argpartition). Só depois compare com scikit-learn em pytest.
3. Escreva suas funções de recall@k e MRR e rode no golden set.
4. Benchmark de 3 modelos: mesmo corpus, mesmas perguntas, mesmos prefixos de instrução. Registre dimensão, latência, custo por milhão de tokens, recall@5 e MRR.
5. Teste Matryoshka (truncar e renormalizar) se o modelo suportar.
6. Responda o checkpoint com 3 exemplos reais do seu golden set.

## Conceitos para dominar
- Cosseno, produto escalar e distância euclidiana; por que coincidem em vetores normalizados.
- InfoNCE, negativos no batch e negativos difíceis.
- Assimetria query-documento e prefixos de instrução.
- Dimensão × custo × qualidade; limite de tokens e truncamento.
- Por que falha com nomes próprios, códigos e siglas (subword, semântica vs lexical), negação e números.

## Exercícios sem IA
- Cosseno e k-NN do zero. Derive no papel por que cos = produto escalar quando normalizado. Explique InfoNCE no papel.

## Pronto quando
- Testes passam contra scikit-learn; tabela com 3 modelos; texto do checkpoint com 3 exemplos; síntese escrita sem consulta.

## Armadilhas
- Esquecer de normalizar; comparar modelos com prefixos diferentes; escolher o modelo no mesmo conjunto que usa para reportar; golden set pequeno demais.

## Entrevista sênior (uma resposta forte cobre...)
1. Por que cosseno e não L2? Equivalência normalizada, magnitude, comportamento do índice.
2. Como escolher modelo de embedding para um domínio novo? Benchmark próprio com golden set; MTEB só como ponto de partida; idioma, custo, latência, dimensão, limite de tokens.
3. Quando a busca vetorial falha e o que fazer? Termos exatos e códigos; híbrido com BM25, reranker, metadados.
4. O que é Matryoshka e quando usar? Truncar dimensões com perda controlada para economizar memória e latência.
5. Como trocar o modelo de embedding em produção? Reembedar tudo, índice versionado, shadow, comparação no golden set antes da virada.

## Leituras
MTEB, BEIR, documentação do modelo escolhido.
