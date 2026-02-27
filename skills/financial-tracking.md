# SKILL: Financial Tracking — BodyBase

Rastreia receita da BodyBase via Stripe. Confidencial — apenas DM ou topic `financeiro`.

## Fonte de dados

**Stripe API** (já configurado no bodybaseback)
- Endpoint: `api.bodybaselab.com`
- Env: STRIPE_SECRET_KEY (no Coolify do bodybaseback)

## Métricas a rastrear

```
MRR (Monthly Recurring Revenue)
  = soma de todas assinaturas ativas × preço mensal

ARR = MRR × 12

Assinantes ativos = count de subscriptions com status 'active'

Churn mensal = cancelamentos no mês / assinantes início do mês

Novo MRR = novos assinantes no mês × R$97

MRR perdido = cancelamentos no mês × R$97

Net MRR = Novo MRR - MRR perdido
```

## Consultas Stripe (via API)

```
# Assinantes ativos agora
GET /v1/subscriptions?status=active&limit=100

# Novos este mês
GET /v1/subscriptions?created[gte]=<início_mês>&status=active

# Cancelados este mês
GET /v1/subscriptions?canceled_at[gte]=<início_mês>

# Eventos de pagamento
GET /v1/events?type=payment_intent.succeeded
```

## Relatório mensal (1º de cada mês, apenas DM)

```
📊 BodyBase — [Mês/Ano]

MRR: R$ XXX
ARR: R$ XXX
Assinantes: XXX
Novos: +XX
Cancelamentos: -XX
Churn: X.X%

vs. mês anterior:
MRR: [↑/↓ X%]
Assinantes: [↑/↓ XX]
```

## Alertas imediatos (DM)

- Primeiro pagamento processado → notificar
- Cancelamento → notificar com motivo (se disponível)
- Falha de pagamento recorrente → notificar
- MRR cair mais de 10% em uma semana → alertar

## Regras de confidencialidade

- **Nunca** mencionar valores em grupos, topics públicos ou emails
- Análises direcionais ok em grupos: "receita crescendo", "churn estável"
- Valores específicos: apenas DM ou topic `financeiro`
- Não incluir dados financeiros em logs que vão para outros topics
