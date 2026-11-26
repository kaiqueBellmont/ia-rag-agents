# GUIA: MCP (semana 9, 08/02 a 14/02)

## Como seguir
1. Leia a spec em modelcontextprotocol.io (ciclo de vida, transportes, primitivas).
2. Escreva um server stdio na mão (initialize, tools/list, tools/call) sem SDK.
3. Refaça com o SDK Python: busca do RAG como tool, documentos como resources.
4. Client mínimo seu: lista e chama as tools.
5. Conecte ao Claude Code, observe como ele escolhe a tool e ajuste as descrições. Faça a versão HTTP com token.
6. Segurança: server malicioso seu contra seu client (tool poisoning, injeção via resultado de tool).

## Conceitos para dominar
- JSON-RPC, ciclo de vida, transportes stdio e HTTP.
- Tools, resources e prompts; sampling, roots, elicitation.
- Autenticação (OAuth em nível conceitual, token na prática).
- Tool poisoning, injeção via resultado de tool, confused deputy.

## Exercícios sem IA
- Server stdio sem SDK; server malicioso contra seu client.

## Pronto quando
- Seu RAG responde via Claude Code por MCP, a versão HTTP exige token, e você descreve um ataque de tool poisoning e a defesa.

## Armadilhas
- Descrições de tool vagas (o modelo escolhe mal); resultado de tool não sanitizado; segredos no server stdio.

## Entrevista sênior (uma resposta forte cobre...)
1. O que é MCP e quando construir um server? Padronizar acesso a dados e ações para vários clientes; vale quando mais de um cliente ou agente usará a capacidade.
2. Tools vs resources vs prompts? Ação do modelo, contexto controlado pela aplicação, templates do usuário.
3. stdio vs HTTP? Local e simples vs remoto, multiusuário, com autenticação.
4. Riscos de segurança? Tool poisoning, injeção indireta, privilégio excessivo, confused deputy.

## Leituras
Spec do MCP, documentação do SDK Python.
