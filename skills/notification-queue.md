# SKILL: Notification Queue — Digest de Mensagens

Nenhuma notificação vai direto para o Estevão, exceto críticas.
Tudo passa pela fila e é entregue em lotes.

## Tiers de prioridade

| Tier | Exemplos | Entrega |
|------|----------|---------|
| 🔴 Crítico | Lead BodyBase novo, erro de produção, falha de pagamento | Imediato |
| 🟡 Alto | Email importante, tarefa urgente, cancelamento de assinante | Lote a cada 1h |
| 🟢 Médio | Update de CRM, log de sistema, contato qualificado | Lote a cada 3h |
| ⚪ Baixo | Newsletter triada, relatório de uso, digest informativo | 8h BRT (uma vez ao dia) |

## Regra principal

Antes de enviar qualquer mensagem ao Estevão, classificar o tier.
Se não for crítico, acumular na fila e enviar no próximo lote do tier.

## Formato do lote

Quando o lote dispara, agrupar por categoria em uma única mensagem:

```
📬 Lote [Hora] — N itens

📧 Emails (3)
• João Silva — resposta sobre parceria
• Startup X — interesse em BodyBase
• Newsletter Saúde — triada, arquivada

🏋️ CRM BodyBase (1)
• Lead novo qualificado: Maria, 28, personal trainer

⚙️ Sistema (1)
• Cron backup: ok (02:00 BRT)
```

## Estado da fila

Manter em memória (ou arquivo temporário `~/agent/data/queue.json`):

```json
{
  "alto": [],
  "medio": [],
  "baixo": [],
  "ultima_entrega": {
    "alto": "2026-02-27T10:00:00-03:00",
    "medio": "2026-02-27T09:00:00-03:00",
    "baixo": "2026-02-27T08:00:00-03:00"
  }
}
```

## Horários de entrega

- **Crítico:** disparar imediatamente, não enfileirar
- **Alto:** checar a cada hora — se tem itens, enviar e limpar
- **Médio:** checar a cada 3h (08h, 11h, 14h, 17h, 20h BRT)
- **Baixo:** enviar às 08h BRT com o digest do dia

## Digest diário (08h BRT)

Consolidar tudo de baixa prioridade + resumo do dia anterior:

```
☀️ Bom dia, Estevão — [Data]

📊 Ontem em resumo:
• Emails processados: N (N respondidos, N arquivados)
• Leads novos BodyBase: N
• Custo API Clawd: ~$X.XX

📬 Pendências de baixa prioridade (N):
• [item 1]
• [item 2]

Tenha um bom dia. ⚡
```

## Exceções — sempre imediato (ignorar fila)

- Qualquer erro de produção (bodybaseback, Superbem)
- Novo lead BodyBase (status: investidor ou parceiro)
- Falha de pagamento Stripe
- Cron crítico falhando por +2 execuções seguidas
- Mensagem direta do Estevão pedindo update

## Silêncio noturno

Das 22h às 07h BRT: segurar TUDO (incluindo Alto) exceto críticos reais.
Acumular na fila e entregar às 08h no digest matinal.
