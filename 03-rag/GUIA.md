# GUIA: RAG (semanas 3, 5 e 6)

## Como seguir
Semana 3 (base e ingestão)
1. Ingestão: parsing de PDF/HTML/Markdown, tabelas, metadados (fonte, seção, página). Veja onde seu parser quebra.
2. Chunkers: fixo, recursivo e por estrutura; indexe os três no pgvector.
3. Geração com citação, abstenção ("não sei") e saída validada com Pydantic.
4. Golden set até 60 (10 sem resposta, 10 multi-trecho), 5 perguntas novas por dia.
Semana 5 (recuperação, usando o harness do módulo 04-eval)
5. Uma técnica por vez contra o baseline v0: BM25 → híbrido com RRF → reranker → HyDE/multi-query/decomposição.
Semana 6 (contexto e relatório)
6. Contextual retrieval, parent-document, contexto longo vs RAG, abstenção. Congele a configuração final.
7. Relatório de ablação com tabela e análise de erros por pergunta.

## Conceitos para dominar
- Falhas por etapa: parser, chunk, busca, geração.
- Chunking: tamanho, overlap, estrutura; compromisso precisão × contexto.
- BM25 e por que ajuda com siglas; RRF: soma de 1/(k + posição) entre listas (k costuma ser 60).
- Bi-encoder vs cross-encoder; custo do reranker.
- HyDE, multi-query, decomposição, step-back.
- Contextual retrieval, parent-document/small-to-big, lost in the middle.
- GraphRAG e RAG agêntico em nível conceitual.

## Exercícios sem IA
- Os três chunkers e a recuperação; RRF à mão; análise de erros do relatório.

## Pronto quando
- Tabela de ablação com IC, configuração final só com o que melhorou, erro explicado para cada pergunta errada.

## Armadilhas
- Mudar duas coisas por vez; ajustar o prompt pergunta a pergunta (overfit no golden set); ignorar custo e latência; reranker sobre top-k pequeno demais.

## Entrevista sênior (uma resposta forte cobre...)
1. Desenhe um RAG para 10 milhões de documentos. Ingestão incremental, índice particionado, híbrido, reranker, ACL, avaliação, custo.
2. O RAG responde mal: como depura? Separar recuperação de geração, taxonomia de erros por etapa, olhar os trechos recuperados.
3. Como escolhe chunking? Experimento medido no seu corpus, não regra geral.
4. Quando adicionar reranker? Recall alto no top-50 e precisão baixa no top-5; latência e custo aceitáveis.
5. Como trata tabelas e PDFs ruins? Parsing por layout, OCR, chunk por estrutura, metadados.
6. RAG ou contexto longo? Custo, latência, atualização, precisão em corpus grande.

## Leituras
Contextual Retrieval (Anthropic), RRF (Cormack et al.), Lost in the Middle.
