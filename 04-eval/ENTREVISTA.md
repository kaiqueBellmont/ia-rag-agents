# Perguntas de entrevista sênior sobre avaliação (responda em inglês, em voz alta)

Para cada pergunta, a resposta forte cobre os pontos listados. Treine nas sextas e use /feynman para o Claude apontar lacunas.

1. How do you evaluate a RAG system end to end?
   Separar recuperação e geração; golden set; recall@k, MRR, nDCG; fidelidade, relevância, correção; análise de erros por etapa; custo e latência; intervalo de confiança.
2. How do you build a golden set when you have no labels?
   Perguntas escritas por quem conhece o domínio, geração sintética revisada por humano, tipos variados (factual, multi-trecho, sigla, sem resposta), versionamento, evitar vazamento, tamanho vs poder estatístico.
3. How do you know a 3-point improvement is real?
   Bootstrap, comparação pareada no mesmo conjunto, tamanho da amostra, variância do juiz e da temperatura, repetir execuções.
4. What are the risks of LLM-as-judge and how do you mitigate them?
   Vieses de posição, verbosidade e autopreferência; rubrica e saída estruturada; calibração com rótulos humanos (kappa); juiz diferente do gerador; amostragem humana contínua.
5. Recall@k is high but answers are bad. Why?
   Falha na geração: contexto longo (lost in the middle), chunk com informação parcial, prompt, ranking ruim dentro do top-k, fidelidade vs correção.
6. How do you evaluate an agent?
   Sucesso por estado final verificável, acerto de tool calls, eficiência da trajetória, custo e latência, confiabilidade em k execuções, juiz de trajetória, casos adversariais.
7. Agents are non-deterministic. How do you avoid flaky evals?
   k execuções por tarefa, temperatura baixa, estado final determinístico, mocks de tools, limiares com intervalo de confiança.
8. How do you run evals in CI without burning money?
   Subconjunto rápido no PR e completo à noite; cache de chamadas; juiz menor onde a calibração permitir; portas de bloqueio com tolerância.
9. Offline vs online evaluation?
   Offline é reprodutível e barato; online reflete uso real (feedback, amostragem de traces, A/B, drift); o ciclo traces ruins → golden set liga os dois.
10. How do you catch regressions when changing the model or the prompt?
   Harness com hash de configuração, comparação pareada, análise por tipo de pergunta, shadow ou canary antes da virada.
11. How do you evaluate security against prompt injection?
   Suíte adversarial versionada, taxa de sucesso de ataque, tool poisoning, equilíbrio entre bloqueio e falsos positivos.
12. How do you communicate evaluation results to a non-technical stakeholder?
   Uma tabela, ganho em termos de negócio, custo por consulta, limites e incerteza, recomendação clara.
13. What are the limits of your own golden set?
   Cobertura, viés de quem escreveu, tamanho, desatualização, risco de overfit; como você planeja renová-lo.

14. How do you detect hallucinations in production?
   Faithfulness por afirmação contra as fontes, citação obrigatória, checagem contra resultados de tools, abstenção quando não há suporte, amostragem com revisão humana.
15. How do you evaluate tool calling?
   Tool esperada vs escolhida, argumentos validados por schema, taxa de chamadas inválidas e redundantes, recuperação após erro de tool, efeito colateral correto no estado final.
16. How would you A/B test a new prompt or model?
   Métrica primária, guardrails (custo, latência, segurança), randomização, tamanho de amostra, comparação pareada offline antes, rollout gradual e rollback.
17. Pairwise comparison vs absolute scoring?
   Pareada é mais confiável para diferenças sutis (randomize a ordem); nota absoluta serve para limiares e monitoramento contínuo.
18. Why isn't semantic similarity enough to evaluate answers?
   Resposta correta com outras palavras recebe nota baixa; erro sutil (número, negação) recebe nota alta; serve só como triagem barata.
19. How do you use human review efficiently?
   Rotear casos de baixa confiança do juiz ou discordância, amostragem contínua, devolver rótulos para calibrar o juiz e renovar o golden set.
20. How would you evaluate a multi-agent system?
   Resultado ponta a ponta primeiro, depois correção de delegação e handoffs, custo total; primeiro questionar se multiagente é necessário.
21. Ragas, DeepEval, LangSmith or Langfuse: how do you choose?
   Construir à mão primeiro, depois medir a concordância do framework com rótulos humanos; integração com CI, custo, lock-in e onde os dados ficam.
22. Design the evaluation strategy for a support agent that issues refunds.
   Tarefas com estado final verificável, argumentos de tool validados, suíte adversarial (injeção para forçar reembolso), aprovação humana acima de um limite, monitoramento online e custo por ticket.

23. An agent fails in production. How do you debug it?
   Achar o trace, localizar o primeiro passo errado, classificar a falha (loop, tool errada, argumento inventado, contexto perdido), replay e fork a partir de um checkpoint, transformar em caso do dataset, rodar o experimento completo para checar regressão, deploy gradual.
24. How do you turn production failures into better evals?
   Amostrar traces ruins (feedback, juiz online, revisão humana), adicionar ao dataset com o resultado esperado, criar um evaluator específico, validar a correção offline e só então publicar.
