# GUIA: Capstone e entrevistas (semanas 11 e 12)

## Como seguir
Semana 11
1. Desenhe a arquitetura e registre as decisões (o que entrou, o que ficou de fora e por quê).
2. Integre ingestão, RAG final (configuração da semana 6), agente, API FastAPI, server MCP, harness e CI, deploy.
3. Rode a avaliação final, congele os números e corrija os 3 piores erros.
4. README com arquitetura, resultados, limites e como rodar; grave a demo de 3 minutos.
Semana 12
5. Aprofundamento em avaliação de produção: A/B simulado com comparação pareada e intervalo de confiança, suíte adversarial (red team) no CI, e escolha justificada de framework (Ragas, DeepEval, LangSmith ou Langfuse). Fine-tuning de embeddings, ColBERT e reranker próprio são opcionais.
6. 10 respostas escritas de system design de LLM, cada uma com diagrama e plano de avaliação.
7. Simulação de entrevista técnica completa em inglês (60 min) com o Claude; corrija lacunas. Atualize CV e LinkedIn com projeto e números.

## Pronto quando
- Um recrutador consegue entender o projeto em 3 minutos e rodá-lo; você explica cada componente e o número que o justifica.

## Armadilhas
- README sem números; escopo grande demais; demo sem mostrar avaliação.

## Perguntas de system design para treinar
1. RAG para 10 milhões de documentos com permissões por usuário.
2. Assistente de suporte com agente e ações sensíveis.
3. Pipeline de avaliação contínua para um produto com LLM.
4. Redução de custo de 50% sem perder qualidade.
5. Migração de modelo de embedding em produção sem downtime.
6. Pesquisa híbrida em um corpus com muitos códigos e siglas.
7. Extração de dados de documentos escaneados com validação (use sua experiência de document processing).
8. Segurança de agentes com ferramentas externas.
9. Multiagente: quando e como.
10. Observabilidade e resposta a incidentes em um sistema RAG.
