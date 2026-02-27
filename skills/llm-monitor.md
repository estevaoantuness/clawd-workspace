# SKILL: LLM Usage Monitor

Rastreia todas as chamadas de API feitas pelo Clawd. Custo, tokens, provider, tarefa.

## Onde armazenar

SQLite local: `~/agent/data/logs.db`
JSONL backup: `~/agent/data/logs/llm-calls.jsonl`

## Schema

```sql
CREATE TABLE llm_calls (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  ts DATETIME DEFAULT CURRENT_TIMESTAMP,
  provider TEXT NOT NULL,        -- 'anthropic' | 'openai'
  model TEXT NOT NULL,           -- 'claude-sonnet-4-5' etc
  task_type TEXT,                -- 'email_triage' | 'crm_update' | 'chat' | etc
  input_tokens INTEGER,
  output_tokens INTEGER,
  duration_ms INTEGER,
  estimated_cost_usd REAL,
  context TEXT,                  -- resumo da tarefa (sem dados sensíveis)
  session_id TEXT
);
```

## Custo estimado por modelo (atualizar quando mudar)

| Modelo | Input (M tokens) | Output (M tokens) |
|--------|-----------------|------------------|
| claude-opus-4-6 | $15 | $75 |
| claude-sonnet-4-6 | $3 | $15 |
| claude-haiku-4-5 | $0.80 | $4 |
| gpt-4o | $5 | $15 |

## Logging (fire-and-forget)

Todo subagente e chamada de API deve logar ao terminar. Nunca bloquear
a tarefa principal para logar. Se o log falhar, continue silenciosamente.

## Consultas úteis (responder quando Estevão perguntar)

```sql
-- Gasto total esta semana
SELECT SUM(estimated_cost_usd) as total
FROM llm_calls
WHERE ts >= datetime('now', '-7 days');

-- Gasto por modelo este mês
SELECT model, COUNT(*) as calls,
       SUM(input_tokens + output_tokens) as total_tokens,
       SUM(estimated_cost_usd) as cost_usd
FROM llm_calls
WHERE ts >= datetime('now', 'start of month')
GROUP BY model ORDER BY cost_usd DESC;

-- Tarefas mais caras
SELECT task_type, COUNT(*) as calls, SUM(estimated_cost_usd) as cost
FROM llm_calls
GROUP BY task_type ORDER BY cost DESC LIMIT 10;
```

## Relatório semanal (heartbeat)

Durante o heartbeat semanal, calcular e postar no topic `sistema`:
- Total gasto na semana (USD)
- Modelo mais usado
- Tarefa mais cara
- Comparação com semana anterior (delta %)

Nunca postar valores em canais que não sejam `sistema` ou DM.
