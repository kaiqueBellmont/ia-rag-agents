# GUIA: Fase 0 (26/11 a 13/12, 8h no total)

## Como seguir
1. Siga a tabela diária do plano (20 min em dia útil, 40 min no fim de semana). Registre cada dia em notes/diario.md.
2. Prioridade: ambiente (docker compose), corpus escolhido, 30 perguntas do golden set e o diagnóstico escrito de 27/11.
3. Não pule o diagnóstico: ele é sua linha de base. Em 11/12 você o refaz e compara.
4. Se um dia falhar, não compense: use o piso de 45 min ou siga em frente. O golden set é o item que não pode atrasar.

## Conceitos para dominar (explique sem consultar)
- Cosseno, produto escalar e normalização.
- Como um modelo de embedding é treinado (contrastive learning).
- Ideia do HNSW (camadas, vizinhos).
- Contextual Retrieval: o que propõe.
- Workflow vs agente.

## Exercícios sem IA
- Cosseno em numpy testado à mão com 5 casos.
- k-NN por força bruta sobre ~200 trechos.
- Recall@5 manual nas 20 perguntas.

## Pronto quando
- Repositório commitado, docker compose sobe, corpus baixado, golden set com 30 perguntas e fontes esperadas, diagnóstico antes e depois.

## Armadilhas
- Corpus que você não conhece (não saberá escrever boas perguntas).
- Perguntas só factuais: inclua sigla/código, multi-trecho e uma parte sem resposta.

## Leituras
Contextual Retrieval (Anthropic), Building effective agents (Anthropic), MTEB, paper do HNSW (ideia e algoritmo).
