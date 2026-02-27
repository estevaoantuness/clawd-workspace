# SKILL: CRM Pipeline Completo — BodyBase

Estende o Google Sheets CRM com discovery automático, scoring e nudges.

## Google Sheets CRM

**ID:** 14DiB69LZ-sdinbSPvImj3TPaMYB0-TirDWIzVRWdMoM
**Aba principal:** CRM

### Colunas completas (adicionar `score` e `next_action` ao Sheets):

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | string | UUID gerado |
| nome | string | Nome completo |
| email | string | Email principal |
| telefone | string | Com DDD |
| origem | string | waitlist / email / indicacao / linkedin / instagram |
| tipo | string | lead / parceiro / investidor / imprensa |
| status | string | novo / contatado / qualificado / descartado |
| score | integer | 0–100 (calculado, ver abaixo) |
| notas | string | Histórico livre |
| next_action | string | Próxima ação sugerida |
| ultimo_contato | date | YYYY-MM-DD |
| criado_em | date | YYYY-MM-DD |

## Score de Relacionamento (0–100)

Calcular ao atualizar cada contato:

```
score = (recência × 40) + (frequência × 35) + (qualificação × 25)

recência:
  - Contato < 7 dias → 100
  - Contato < 30 dias → 70
  - Contato < 90 dias → 40
  - Contato > 90 dias → 10

frequência:
  - 5+ interações → 100
  - 3–4 interações → 70
  - 1–2 interações → 40
  - 0 interações → 0

qualificação:
  - tipo = 'investidor' → 100
  - tipo = 'parceiro' → 80
  - tipo = 'lead' qualificado → 60
  - tipo = 'lead' novo → 30
  - tipo = 'imprensa' → 50
```

## Discovery Automático via Gmail

Quando processar email novo de remetente desconhecido:
1. Verificar se email já existe no Sheets
2. Se não existe E parece relevante (não spam):
   - Criar linha com status `novo`
   - Tentar inferir nome do assinante
   - Classificar tipo pelo conteúdo do email
   - Calcular score inicial
3. Notificar no topic `bodybase`: "Novo contato detectado: [nome] — [tipo]"

## Nudge Semanal

Toda segunda-feira às 8h BRT, analisar o Sheets e postar no topic `bodybase`:

```
📊 CRM BodyBase — Semana de XX/XX

Leads novos: N
Contatos sem resposta há +7 dias: N

⚠️ Follow-up necessário:
• [Nome] — [tipo] — último contato: X dias atrás
• [Nome] — [tipo] — último contato: X dias atrás

Score médio dos leads qualificados: XX/100
```

## Pipeline de email para lead novo (waitlist)

Quando lead novo entra via waitlist (webhook bodybaseback):
1. Registrar no Sheets (origem: 'waitlist', status: 'novo')
2. Calcular score inicial
3. Gerar rascunho de email de boas-vindas personalizado
4. Postar rascunho no topic `rascunhos` para aprovação
5. Aguardar aprovação do Estevão antes de enviar

### Template de email de boas-vindas (adaptar por perfil):

```
Assunto: Bem-vindo(a) à lista BodyBase, [Nome]

[Nome],

Obrigado por entrar na lista. Você está entre as primeiras pessoas
a testar a BodyBase antes do lançamento.

Em breve você vai receber seu acesso para analisar seus biomarcadores
e descobrir sua idade biológica real.

Qualquer dúvida, responda este email.

Estevão
BodyBase
```

## Aprovação no Telegram

Formato padrão do rascunho no topic `rascunhos`:

```
📧 RASCUNHO — [Nome do destinatário]
Para: email@exemplo.com
Assunto: [Assunto]

[Corpo do email]

---
Responda "envia", edite o texto, ou ignore para descartar.
```

## Dados sensíveis

- Nunca postar email, telefone ou score em grupos. Apenas em DM ou `rascunhos`.
- No topic `bodybase`, usar apenas: nome, tipo, status, days since contact.
