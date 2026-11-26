# GUIA: Agentes (semanas 7 e 8)

## Como seguir
Semana 7 (sem framework)
1. Loop de tool use à mão com 2 tools (buscar, ler trecho): stop_reason, tool_result, limite de passos.
2. Agente que reformula a query, decide quando parar e registra cada passo em log estruturado.
3. Reescreva as tools (nomes, descrições, erros que ensinam o modelo, validação Pydantic) e meça o efeito.
4. Passo de verificação da resposta contra os trechos; compactação do histórico; prompt caching.
5. Compare agente × RAG fixo no golden set (com 15 perguntas multi-hop): acerto, custo, chamadas, p95.
Semana 8 (framework e avaliação)
6. Reimplemente em LangGraph com checkpoint e aprovação humana. Compare linhas de código, acerto, custo e depuração.
7. Avaliação de agentes (módulo 04-eval): dataset com estado final, pass^k, juiz de trajetória.
8. Segurança: instruções maliciosas plantadas em um documento; construa a defesa.
9. Multiagente só em nível conceitual: quando compensa e quando não.

## Conceitos para dominar
- Workflows (encadeamento, roteamento, paralelização, orquestrador-trabalhadores, avaliador-otimizador) vs agentes; ReAct.
- Design de tools; engenharia de contexto (o que entra e sai, compactação, memória curta e longa).
- Guardrails: orçamento de passos, custo e tempo; validação de saída.
- Modos de falha: loops, tool errada, parar cedo, alucinar parâmetros, injeção indireta.
- Quando um workflow fixo vence um agente.

## Exercícios sem IA
- O loop de tool use inteiro; o teste de injeção e a defesa; a página sobre multiagente.

## Pronto quando
- Tabela agente × fixo × LangGraph com custo; métricas de agente no harness; guardrails testados; 10 trajetórias ruins classificadas.

## Armadilhas
- Agente onde um workflow bastaria; tools com descrições vagas; sem limite de passos; avaliar só a resposta final.

## Entrevista sênior (uma resposta forte cobre...)
1. Quando usar agente em vez de pipeline fixo? Incerteza do caminho, custo e latência aceitáveis, necessidade de adaptação; senão workflow.
2. Como projeta boas tools? Poucas, nomes claros, descrições com exemplos, erros acionáveis, idempotência, privilégio mínimo.
3. Como gerencia contexto em tarefas longas? Compactação, memória externa, resultados de tool enxutos, caching.
4. Como protege um agente contra injeção de prompt? Separar dados de instruções, privilégio mínimo, aprovação humana em ações sensíveis, testes adversariais.
5. Por que seu agente falha? Taxonomia por trajetória: loop, tool errada, parada precoce, parâmetro inventado.
6. Quando multiagente compensa? Paralelismo e contextos separados, ao custo de tokens e coordenação.

## Leituras
Building effective agents, Writing effective tools for agents, o post sobre o sistema multiagente de pesquisa, engenharia de contexto para agentes (todos da Anthropic), documentação do LangGraph.
