# GUIA: Agentes (semanas 7 e 8)

## Como seguir
Semana 7 (sem framework)
1. Loop de tool use à mão com 2 tools (buscar, ler trecho): stop_reason, tool_result, limite de passos.
2. Agente que reformula a query, decide quando parar e registra cada passo em log estruturado.
3. Reescreva as tools (nomes, descrições, erros que ensinam o modelo, validação Pydantic) e meça o efeito.
4. Passo de verificação da resposta contra os trechos; compactação do histórico; prompt caching.
5. Compare agente × RAG fixo no golden set (com 15 perguntas multi-hop): acerto, custo, chamadas, p95.
Semana 8 (framework e avaliação)
6. Reimplemente em LangGraph com checkpoint, aprovação humana e tracing no LangSmith ligado desde o início. Compare linhas de código, acerto, custo e depuração.
7. Avaliação de agentes (módulo 04-eval): dataset com estado final, pass^k, juiz de trajetória.
8. Segurança: instruções maliciosas plantadas em um documento; construa a defesa.
9. Sábado: depuração de agentes (veja a seção abaixo) com 3 execuções ruins do seu agente. Multiagente fica opcional: se sobrar tempo, leia sobre quando compensa e quando não.

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

## Depuração de agentes (LangGraph + LangSmith, e o equivalente sem framework)
Roteiro:
1. Reproduza: ache o trace da execução ruim (thread ou run) e anote entrada e estado.
2. Ache o primeiro passo errado, não o último: escolha de tool, argumento, contexto ou resultado de tool.
3. Classifique: loop, tool errada, argumento inventado, contexto perdido, parada precoce, injeção de prompt.
4. Replay e fork: com um checkpointer no grafo, use get_state_history para achar o checkpoint antes do erro, re-execute a partir dele (replay) e depois altere estado ou prompt e siga outro caminho (fork).
5. Vire teste: inclua o caso no dataset com o estado final esperado.
6. Rode o experimento completo para checar regressão e só então faça o deploy.
Ferramentas a praticar: tracing do LangSmith com LangGraph; datasets e experimentos (comparar duas versões lado a lado); Studio local (langgraph dev); anotação de traces e filas de revisão.
Cuidados: interrupts (aprovação humana) são disparados de novo no time travel; em subgrafo padrão só se volta a partir do nível pai.
Sem framework: logs estruturados por passo (entrada, tool, argumentos, resultado, tokens, latência) e um comando que reexecuta a partir do passo N com o estado salvo. Isso é o que Langfuse ou OpenTelemetry fazem na semana 10.
Antes de usar: leia a documentação atual do LangGraph e do LangSmith (recursos mudam).

## Entrevista: depuração
- "Um agente falha em produção. Como você investiga?" Trace, primeiro passo errado, classificação da falha, replay e fork a partir de um checkpoint, caso de teste, experimento de regressão, deploy gradual.
- "Por que o agente funciona às vezes e falha outras?" Não determinismo; rode k vezes, olhe a variância, fixe o estado final verificável, isole a etapa instável.

## SDKs de provedores (Anthropic e OpenAI) e porta LLM
Semana 7: escreva uma interface sua (porta LLM) com o que o agente precisa: gerar com tools, saída estruturada, streaming e uso de tokens. Segunda: adaptador com o SDK da Anthropic. Terça: adaptador com o SDK da OpenAI. Liste as diferenças que apareceram (formato de tool call e de resultado, parada, saída estruturada, streaming, contagem de tokens).
Checklist de recursos de SDK que você deve ter usado e saber explicar:
- Tool use (function calling) e o ciclo de resultado.
- Saída estruturada com schema validado.
- Streaming.
- Prompt caching (quando existe) e contagem de tokens/uso para custo.
- Parâmetros: temperatura, máximo de tokens, parada.
- Erros: rate limit, timeout, sobrecarga; retry com backoff.
- Batch (processamento assíncrono em lote), quando o provedor oferece.
Por que a porta: trocar de provedor, fallback, roteamento de modelos, juiz diferente do gerador e comparação de modelos no mesmo golden set sem reescrever o agente.
SDKs de agentes (OpenAI Agents SDK, Claude Agent SDK): conhecer e saber comparar com o LangGraph e com o seu loop; opcional, para o colchão da semana 12. Leia a documentação atual antes de usar.
Entrevista: "Como você evita lock-in de provedor?" Porta e adaptadores, harness de avaliação para comparar modelos, fallback, prompts versionados e testados por modelo.
