# TOOLS.md — Configurações de Ambiente do Clawd

## Segredos e Config
- `.env` canônico: ~/.agent/.env
- Variáveis: ANTHROPIC_API_KEY, GMAIL_CLIENT_ID, GMAIL_CLIENT_SECRET,
  GMAIL_REFRESH_TOKEN, TELEGRAM_BOT_TOKEN, GOOGLE_SHEETS_ID

## Atribuição
- Ao deixar texto permanente (comentários, notas), prefixe com "⚡ Clawd:"
  exceto quando ghostwriting explicitamente pedido.

## Email
- **Conta do Clawd:** estevao.antunes.rocha@gmail.com
- **Monitorar inbox de:** estevao.rocha.antunes@gmail.com (com permissão OAuth)
- **Provider:** Gmail API (OAuth2)

## Google Sheets — CRM BodyBase
- **Sheets ID:** 14DiB69LZ-sdinbSPvImj3TPaMYB0-TirDWIzVRWdMoM
- **URL:** https://docs.google.com/spreadsheets/d/14DiB69LZ-sdinbSPvImj3TPaMYB0-TirDWIzVRWdMoM
- **Aba CRM:** "CRM" (primeira aba)
- **Colunas:** id | nome | email | telefone | origem | tipo | status | notas | ultimo_contato | criado_em
- **Credenciais:** Google Service Account (mesma do bodybaseback)

## Telegram
- **Bot token:** em ~/.agent/.env → TELEGRAM_BOT_TOKEN
- **Grupo principal:** Clawd HQ

| Tópico        | Thread ID      |
|---------------|----------------|
| inbox         | <PREENCHER>    |
| bodybase      | <PREENCHER>    |
| rascunhos     | <PREENCHER>    |
| superbem      | <PREENCHER>    |
| financeiro    | <PREENCHER>    |
| knowledge     | <PREENCHER>    |
| sistema       | <PREENCHER>    |
| direto        | <PREENCHER>    |

## GitHub
- **Usuário:** estevaoantuness
- **Repos principais:**
  - bodybaseback: https://github.com/estevaoantuness/bodybaseback (backend Express)
  - bodybaselp: frontend LP
- **Contexto BodyBase:** https://github.com/estevaoantuness/bodybaseback/blob/main/docs/clawd-bodybase.md

## Paths
- Logs: ~/agent/data/logs/ (all.jsonl)
- SQLite: ~/agent/data/logs.db
- Memory diária: ~/agent/memory/YYYY-MM-DD.md
- Heartbeat state: ~/agent/memory/heartbeat-state.json

## API tokens
Armazenados em ~/.agent/.env. Nunca expor ou logar.

## Voz
- Entrada: suporte a áudio via transcrição automática
- Saída: texto por padrão. Só voz se explicitamente pedido.
