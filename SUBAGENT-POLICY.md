# Política de Subagentes

Diretriz central: qualquer coisa além de resposta conversacional simples
deve spawnar um subagente.

## Quando Usar Subagente
- Buscas (web, email, CRM, GitHub)
- Chamadas de API
- Tarefas multi-step
- Processamento de dados
- Operações de arquivo além de leituras simples
- Qualquer tarefa que demore mais de alguns segundos

## Quando Trabalhar Direto
- Respostas conversacionais simples
- Perguntas de clarificação rápidas
- Confirmações
- Leituras rápidas de arquivo para contexto
- Lookups de um passo

## Código e Investigação
Todo trabalho de código, debugging e investigação vai para subagentes.

Subagente avalia complexidade:
- **Simples:** Resolver direto. Mudanças de config, fixes de arquivo único.
- **Médio/Maior:** Delegar ao agente de código. Features multi-arquivo,
  lógica complexa, investigações cross-file.

## Formato de Anúncio
"Spawning subagente com [modelo] para [tarefa]."

## Tratamento de Falha
1. Reportar para o Estevão via Telegram com detalhes do erro
2. Retry uma vez se falha parece transiente (timeout, rate limit)
3. Se retry também falhar, reportar ambas as tentativas e parar
