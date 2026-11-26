# GUIA: Produção (semana 10, 15/02 a 21/02)

## Como seguir
1. Instrumente RAG e agente com Langfuse ou OpenTelemetry (traces, custo, latência) e crie o fluxo traces ruins → golden set.
2. Evals em CI com GitHub Actions: subconjunto no PR, completo à noite, porta de bloqueio por métrica.
3. ACL e multi-tenant: metadado de permissão no índice e teste que prova que A não recupera documento de B.
4. Ingestão incremental por hash de documento; remoção e versionamento de chunks.
5. Docker Compose completo e deploy na nuvem que você já usa; tabela de custo por consulta antes e depois de cache.

## Conceitos para dominar
- Observabilidade de LLM; avaliação online e drift.
- Filtro de permissão antes vs depois da busca; vazamento entre tenants.
- Reindexação incremental, dados desatualizados, PII.
- Prompt caching, cache de resultados, limites de taxa, custo por consulta.

## Exercícios sem IA
- O teste de isolamento entre usuários.

## Pronto quando
- Um PR que piora recall é bloqueado no CI; o isolamento é provado por teste; há custo por consulta registrado.

## Armadilhas
- Logar dados sensíveis nos traces; filtro de permissão aplicado depois da busca; cache sem invalidação.

## Entrevista sênior (uma resposta forte cobre...)
1. Como levar este RAG para produção e o que monitorar? Traces, custo, latência p95, qualidade online, drift, alertas, rollback.
2. Como garantir que um usuário não veja documento de outro? Filtro de permissão na consulta, testes de isolamento, auditoria.
3. Como manter o índice atualizado? Ingestão incremental, hash, versionamento, remoção.
4. Como reduzir custo sem perder qualidade? Caching, modelos menores onde medido, reranker seletivo, truncamento Matryoshka.

## Leituras
Documentação do Langfuse e do OpenTelemetry, GitHub Actions.

## Serving e custo de LLM (cai em entrevista sênior)
- Tokens e custo: custo por consulta = entrada × preço + saída × preço (some embedding, reranker e juiz). Meça p50 e p95.
- Streaming para latência percebida; timeouts e cancelamento.
- Batching: agrupar requisições (por tamanho ou tempo) para throughput, ao custo de latência por requisição.
- Cache: prompt caching para prefixos longos, cache de resultados para consultas repetidas, com invalidação.
- Retry com backoff e jitter, circuit breaker e fallback para outro modelo ou provedor.
- Roteamento ou cascata de modelos: modelo pequeno primeiro, grande quando a confiança é baixa; decida com o golden set.
- Rate limits e filas; limites por usuário e por tenant.
- Saída estruturada com schema validado e uma tentativa de reparo.
Entrevista: "Design an LLM gateway that handles retries, caching, batching and model fallback" e "Como reduzir o custo em 50% sem perder qualidade?".
