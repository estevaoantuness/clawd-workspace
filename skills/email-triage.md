# SKILL: Email Triage

Processa o inbox de estevao.antunes.rocha@gmail.com.
Nenhum email chega ao Estevão sem passar por aqui primeiro.

## Fluxo principal

```
Email novo chega
      ↓
Classificar tipo e urgência
      ↓
┌─────────────────────────────────────────┐
│ Spam / irrelevante → arquivar, silêncio │
│ Newsletter → resumir no digest diário   │
│ Normal → resumo no topic inbox          │
│ Urgente → draft em rascunhos            │
│ Crítico → imediato + draft              │
└─────────────────────────────────────────┘
```

## Classificação de emails

### Crítico (notificar imediato + gerar draft)
- Lead BodyBase de investidor ou parceiro estratégico
- Resposta direta a email que o Estevão enviou
- Pagamento, contrato, proposta comercial
- Qualquer assunto com prazo explícito em menos de 24h
- Erro de sistema reportado por usuário

### Urgente (gerar draft, postar em rascunhos)
- Lead BodyBase qualificado (lead comum, personal trainer, atleta, executivo)
- Email de parceiro ou fornecedor pedindo resposta
- Imprensa ou mídia com interesse na BodyBase
- Pergunta direta sobre preço ou acesso antecipado

### Normal (resumo no topic inbox, sem draft)
- Follow-up de contato já registrado no CRM
- Update de serviço que usa (Vercel, Supabase, Stripe, Coolify)
- Email de networking sem urgência
- Resposta automática de qualquer sistema

### Digest (acumular, enviar no lote das 8h)
- Newsletters (qualquer)
- Notificações de redes sociais
- Recibos e confirmações de compra
- Relatórios automáticos de ferramentas

### Silêncio (arquivar sem notificar)
- Spam confirmado
- Promoção ou oferta genérica
- Email sem relação com BodyBase, Superbem ou vida do Estevão
- Notificações de serviços que ele não usa

## Formato do resumo no topic `inbox`

```
📧 [Urgência] De: [Nome] <email>
Assunto: [Assunto]
Resumo: [1-2 frases do que o email pede ou informa]
Ação sugerida: [responder / arquivar / encaminhar / aguardar]
```

## Formato do draft no topic `rascunhos`

```
📝 DRAFT — [Nome do destinatário]
Para: email@exemplo.com
Assunto: Re: [Assunto original]

[Corpo do email em PT-BR, tom BodyBase, direto]

---
"envia" → mando agora
"edita: [sua edição]" → ajusto e mando
ignore → descarto
```

## Regras específicas BodyBase

- Qualquer email mencionando "biomarcadores", "saúde", "lista de espera",
  "waitlist", "acesso antecipado", "BodyBase" → urgente ou crítico
- Tom do draft segue o tom de voz da BodyBase (ver bodybase.md):
  científico, acessível, direto, sem linguagem de IA
- Assinar como "Estevão / BodyBase" (ghostwriting — não mencionar Clawd)
- Nunca prometer datas de lançamento específicas
- Para leads: registrar no CRM (skills/crm-pipeline.md) antes de gerar draft

## Regras específicas Superbem

- Emails sobre Superbem → postar resumo no topic `superbem`
- Raramente gerar draft — operação é mais offline
- Alertas operacionais do Superbem → tier crítico se afeta funcionamento

## Extração de CRM

Para todo email de pessoa física (não automático, não sistema):
1. Verificar se remetente existe no CRM Sheets
2. Se não existe e é relevante → criar entrada automaticamente
3. Se existe → atualizar `ultimo_contato` e adicionar nota

## Processamento em lote vs tempo real

- Checar inbox a cada 15 minutos durante horário ativo (07h–22h BRT)
- Críticos: processar imediatamente
- Resto: acumular e processar em lote a cada hora
- Fora do horário: acumular tudo, processar às 07h junto com digest matinal

## O que nunca fazer

- Nunca enviar email sem aprovação explícita do Estevão
- Nunca responder em nome do Estevão sem draft aprovado
- Nunca revelar que é uma IA nos emails
- Nunca prometer features que não existem no produto
