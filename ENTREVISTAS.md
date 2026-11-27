# Playbook de entrevistas (AI Engineer sênior)

Princípio: entrevistas sênior escolhem poucos temas e vão fundo em falhas e trade-offs. Profundidade nos temas Tier 1, decisão nos Tier 2, conhecimento nos Tier 3. Toda resposta leva um número seu.

## 1. Temas por prioridade
Tier 1 (cai em quase toda entrevista; estude até explicar falhas e trade-offs)
- RAG ponta a ponta e depuração: ingestão, chunking, híbrido (BM25 + densa com RRF), reranker, query rewriting, filtros e permissões.
- Avaliação: golden set, métricas de recuperação e de geração, juiz LLM e seus vieses, regressão em CI, avaliação online. (módulo 04-eval)
- Embeddings e busca vetorial: cosseno vs produto escalar, ANN (HNSW, IVF), filtros, memória, quando pgvector basta.
- System design de LLM: custo, latência, escala, falhas, monitoramento.
Tier 2 (cai com frequência; saiba decidir e justificar)
- Agentes: workflow vs agente, design de tools, modos de falha, contexto e memória, segurança.
- Produção: observabilidade, evals no CI, custo por consulta, ACL, ingestão incremental.
- Serving de LLM: tokens e custo, streaming, batching, cache, retry e fallback, roteamento de modelos, rate limits.
- RAG vs fine-tuning vs contexto longo vs prompt.
Tier 3 (conheça e saiba dizer quando usar)
- MCP, LangGraph e frameworks, SDKs de agentes (OpenAI Agents SDK, Claude Agent SDK), multiagente, GraphRAG, fine-tuning de embeddings, ColBERT.
Base para todos: SDKs de provedores (Anthropic e OpenAI) atrás de uma porta LLM sua: tool use, saída estruturada, streaming, caching, erros e retry.

## 2. Roteiro de system design (use sempre a mesma ordem)
1. Requisitos e restrições (usuários, volume, latência, custo, privacidade, permissões).
2. Dados e ingestão (fontes, parsing, atualização, PII).
3. Recuperação (chunking, índice, híbrido, filtros, reranker).
4. Geração (prompt, citações, abstenção, saída estruturada).
5. Avaliação (golden set, métricas, juiz, CI, online).
6. Custo e latência (tokens, cache, modelo menor onde medido, p95).
7. Falhas e segurança (alucinação, injeção de prompt, timeouts, fallback).
8. Monitoramento e evolução (traces, drift, feedback, ciclo traces → golden set).

## 3. Depurar um RAG que responde errado (isole as etapas)
1. A resposta certa existe no corpus? (ingestão/parsing)
2. O trecho certo foi recuperado no top-k? (recall@k do gabarito)
3. Estava bem posicionado? (MRR, reranker)
4. O contexto montado estava completo e sem ruído? (chunk parcial, ordem, tamanho)
5. O modelo usou o contexto? (fidelidade, prompt, abstenção)
6. A saída foi validada? (schema, citações)
"O LLM está alucinando" costuma ser o diagnóstico errado: a falha quase sempre está antes, na recuperação.

## 3b. Depurar um agente que falha
1. Ache o trace da execução ruim e anote entrada e estado.
2. Localize o primeiro passo errado (tool, argumento, contexto, resultado de tool), não o último.
3. Classifique a falha: loop, tool errada, argumento inventado, contexto perdido, parada precoce, injeção.
4. Replay e fork a partir de um checkpoint anterior ao erro (time travel no LangGraph, ou reexecução do passo N no seu agente).
5. Transforme em caso do dataset com o estado final esperado.
6. Rode o experimento completo para checar regressão; só então faça o deploy.
Ferramentas para citar com segurança porque você as praticou: LangSmith (tracing, datasets, experimentos, anotação) e Langfuse ou OpenTelemetry. Saiba comparar.

## 4. Decisões de arquitetura (sempre diga quando NÃO usar)
- RAG vs fine-tuning: RAG para conhecimento que muda e precisa de fonte; fine-tuning para estilo, formato e comportamento; os dois se combinam.
- RAG vs contexto longo: contexto longo para poucos documentos e consultas raras; RAG quando o corpus é grande, muda ou o custo por consulta importa.
- Agente vs workflow: workflow quando o caminho é conhecido (mais barato, previsível, testável); agente quando o caminho depende dos resultados.
- pgvector vs banco dedicado: pgvector quando já se usa Postgres e a escala é moderada; dedicado para escala grande, filtros pesados e operação separada.
- Reranker: quando o recall no top-50 é bom e a precisão no top-5 não; pague a latência só então.
- Modelo grande vs pequeno: meça no seu golden set; use roteamento ou cascata (pequeno primeiro, grande quando a confiança é baixa).

## 5. Números e fórmulas de bolso
- Memória de vetores: n × d × 4 bytes (float32); int8 divide por 4; binária por 32. 50M × 1024 ≈ 205 GB, 51 GB e 6,4 GB.
- Custo por consulta = tokens de entrada × preço de entrada + tokens de saída × preço de saída (some embedding, reranker e juiz).
- KV cache ≈ 2 × camadas × cabeças KV × dimensão da cabeça × tamanho da sequência × bytes × tamanho do batch. Por isso contexto longo e batch grande custam memória.
- Atenção cresce de forma quadrática com o tamanho da sequência; por isso contexto longo custa latência.
- RRF: soma de 1/(k + posição) entre as listas (k costuma ser 60).
- Temperatura baixa para extração e avaliação; saída estruturada com schema validado; prompt caching para prefixos longos repetidos.
- Reporte latência em p50 e p95, nunca só média.

## 6. Coding ao vivo (a triagem costuma ser de engenharia de software com recuperação e serving)
Treino cronometrado de 45 min aos sábados, sem IA (lista em coding-drills/README.md). Use /drill.

## 7. Histórias de produção (monte 5; treine em inglês nas sextas)
Formato: contexto → decisão → alternativas descartadas → trade-off → métrica antes e depois → o que deu errado → o que faria diferente.
- História 1: seu trabalho real com extração e QA de documentos em escala.
- História 2: o RAG e o relatório de ablação do seu repositório.
- História 3: o agente (agente × pipeline fixo, com custo).
- História 4: um incidente ou falha que você diagnosticou.
- História 5: um trade-off de custo ou latência que você decidiu com número.
Use /story para o Claude fazer o papel de entrevistador.

## 8. Perguntas para fazer ao entrevistador
- Como vocês avaliam o sistema hoje e o que o golden set cobre?
- Qual o maior problema de qualidade ou custo no momento?
- Como é o ciclo de uma mudança de prompt ou modelo até a produção?
- Quem decide quando uma regressão bloqueia o deploy?

## 9. Erros comuns
- Respostas genéricas de "demo" (RAG = vetor + LLM) sem falhas, métricas ou trade-offs.
- Citar frameworks sem saber o que fazem por baixo.
- Afirmar melhoria sem número nem intervalo de confiança.
- Não dizer quando uma técnica não vale a pena.
- Ignorar custo, latência, permissões e segurança.

## 10. Se o cronograma atrasar, corte nesta ordem
1. Server MCP stdio na mão e OAuth conceitual (semana 9).
2. LangGraph além da comparação básica (semana 8).
3. Opcionais: fine-tuning de embeddings, ColBERT, reranker próprio, GraphRAG.
Nunca corte: golden set, harness de avaliação, ablações, avaliação de agentes, coding ao vivo, histórias.
