# MEMORY.md — Padrões e Preferências (apenas DM)

## Contato Pessoal (apenas DM)
- **Email pessoal:** [privado — configurado localmente no .env]
- Este campo existe aqui (não no USER.md) para não vazar em grupos.

## Preferências do Estevão
- **Escrita:** Direto, sem enrolação. Evitar texto que soa como IA.
- **Tom no DM:** Mais informal, parceiro de negócios. Pode ter humor seco.
- **Formato preferido:** Bullets e tabelas para comparações, prosa para
  análises, código para implementação.
- **Aprovação obrigatória:** Qualquer email de saída, qualquer ação pública.
- **Linguagem:** PT-BR padrão. Inglês apenas quando contexto exige.

## Projetos — Estado Atual

### BodyBase
- Waitlist ativa. Backend em desenvolvimento (Express/Node/Coolify).
- CRM: Google Sheets (ID: 14DiB69LZ-sdinbSPvImj3TPaMYB0-TirDWIzVRWdMoM)
- Email do Clawd para BodyBase: estevao.antunes.rocha@gmail.com
- Sócio: <PREENCHER nome>
- Instagram: @bodybasehealth

### Superbem Barato
- Operacional. Container Coolify ativo, sync a cada 15min.
- Maria (WhatsApp) via n8n + Supabase.
- Não duplicar automações que o n8n já faz.

## Regras de Notificação
- Lead BodyBase novo = crítico, imediato.
- Erros de produção (bodybaseback, Superbem) = crítico, imediato.
- Resto segue os tiers do AGENTS.md.

## Lições Operacionais
- Superbem: Mac launchd desativado, usar container Coolify.
- BodyBase backend: Coolify (não Railway). API em api.bodybaselab.com.
- Não reenviar conteúdo que já foi entregue por cron.
- BodyBase usa Clerk para auth, Stripe para pagamentos, Resend para email.
