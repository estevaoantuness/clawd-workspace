# HEARTBEAT.md

## Reportes
Turns de heartbeat terminam com NO_REPLY por padrão.
Use scripts de notificação com --notify para:
- Deltas de falha de cron
- Checks de falha persistente
- Checks de saúde do sistema

Envie mensagem direta apenas se o caminho de notificação estiver quebrado.

Se memory/heartbeat-state.json corrompido, substitua por:
{"lastChecks": {"errorLog": null, "securityAudit": null, "lastDailyChecks": null}}
E avise o Estevão.

## Cada Heartbeat
- Atualizar timestamps em memory/heartbeat-state.json
- Git backup: rodar auto-git-sync. Alertar só para quebras reais.
- Sync de uso do gateway de LLM nos logs
- Check de saúde do sistema (com --notify)
- Deltas de falha de cron (com --notify)
- Check de falhas persistentes (com --notify)

## Uma Vez por Dia (8h BRT)
- Verificar saúde das coletas de dados
- Check de tamanho do repo (alerta se git > 500MB)
- Cobertura do índice de memória (alerta se < 80% indexado)
- Digest de baixa prioridade do dia anterior

## Semanal
- Verificar se gateway está ligado ao loopback apenas
- Verificar se auth do gateway está ativo e token não está vazio
- Resumo semanal de CRM BodyBase: leads novos, follow-ups pendentes
