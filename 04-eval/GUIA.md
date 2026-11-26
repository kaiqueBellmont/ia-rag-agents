# GUIA: Avaliação de RAG e de agentes (semana 4, e reforços nas semanas 5-6, 8 e 10)

Este módulo é transversal: o harness nasce na semana 4 e é usado em todas as seguintes.

## Como seguir
Semana 4 (RAG)
1. Implemente à mão recall@k, precision@k, MRR e nDCG, com testes em casos numéricos pequenos.
2. Juiz LLM com rubrica e saída estruturada (fidelidade, relevância, correção).
3. Rotule à mão 20 respostas, calcule a concordância (kappa) contra o juiz e ajuste a rubrica.
4. Bootstrap para intervalo de confiança e comparação pareada entre configurações.
5. Harness em CLI: configuração em YAML, resultado em JSON por execução com hash da configuração, custo e latência por pergunta.
6. Rode o baseline completo e salve como v0. Classifique os 15 piores casos por etapa.
Semanas 5 e 6: toda ablação roda pelo harness; nenhuma conclusão sem IC.
Semana 8 (agentes)
7. Dataset de tarefas com estado final verificável; rode cada tarefa 5 vezes e meça a variância.
8. Métricas: sucesso da tarefa, acerto de tool calls, eficiência da trajetória (passos, redundância), custo, confiabilidade em k execuções (pass^k).
9. Juiz de trajetória e simulador de usuário para tarefas conversacionais.
Semana 10
10. Evals em CI (subconjunto rápido no PR, completo à noite) e avaliação online: amostrar traces, feedback do usuário, drift; traces ruins viram perguntas novas do golden set.

## Conceitos para dominar
- Offline vs online. Recuperação vs geração. Golden set: cobertura, perguntas sem resposta, vazamento, versionamento.
- Métricas de recuperação e suas limitações; fidelidade (groundedness) vs correção vs relevância.
- LLM-as-judge: vieses de posição, verbosidade e autopreferência; rubrica; calibração com humanos; juiz diferente do gerador.
- Estatística: tamanho de amostra, bootstrap, comparação pareada, ruído do juiz e da temperatura.
- Agentes: estado final, tool-call accuracy, trajetória, pass@k vs pass^k, custo por tarefa.
- Segurança como avaliação: suíte adversarial versionada.

## Exercícios sem IA
- As quatro métricas e o bootstrap à mão. Taxonomia de erros dos 15 piores casos. Rotulagem humana dos 20 casos do juiz.

## Pronto quando
- O harness reproduz o mesmo resultado com a mesma configuração (hash), publica IC, tem kappa medido e discutido, e roda no CI com porta de bloqueio.

## Armadilhas
- Juiz do mesmo modelo que gerou a resposta; olhar só a média; mudar o golden set sem versionar; ajustar o sistema pergunta a pergunta (overfit); tratar um ganho dentro do ruído como melhoria.

## Leituras
Textos de Hamel Husain sobre evals, documentação do RAGAS, BEIR, artigos sobre avaliação de agentes (tool use e trajetória).

## Técnicas por prioridade (para entrevista)
Essencial: golden dataset e avaliação offline; recall@K, precision@K e MRR; faithfulness e groundedness; answer relevance e correção; context precision e recall; LLM-as-a-judge com rubrica; detecção de alucinação; task success e outcome evaluation (estado final verificável); tool selection e tool calling; trajectory evaluation; regression testing; adversarial testing; online evaluation e observabilidade; custo e latência.
Importante: comparação pareada; A/B testing; human-in-the-loop.
Conceitual: nDCG (saiba explicar e quando usar); multi-agent evaluation (delegação, handoffs, quando evitar multiagente).
Baixa: semantic similarity (só triagem barata; saiba por que não basta).

## Frameworks (construa à mão primeiro, depois compare)
- Ragas: métricas prontas de RAG. Semana 4 (sábado): rode nas mesmas perguntas e veja onde diverge das suas métricas.
- DeepEval: testes no estilo pytest para LLMs e agentes. Semana 12: candidato a rodar no CI.
- Langfuse: tracing, datasets e custo, open source e autohospedado. Semana 10.
- LangSmith: tracing, datasets e experimentos no ecossistema LangChain. Saiba comparar com o Langfuse (stack, onde os dados podem ficar).
- TruLens: fora do plano (sobrepõe Ragas e DeepEval).
Resposta forte em entrevista: "usei X, comparei com a minha implementação, medi a concordância com rótulos humanos e adotei no CI porque...".
