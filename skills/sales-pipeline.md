# SKILL: Inbound Sales Pipeline — BodyBase

Processa leads que chegam por qualquer canal e os move pelo funil.

## Canais de entrada

| Canal | Trigger | Ação imediata |
|-------|---------|---------------|
| Waitlist (bodybaseback webhook) | Lead novo via site | Registrar CRM + qualificar + draft |
| Gmail | Email direto pedindo acesso | Registrar CRM + draft urgente |
| Telegram topic `bodybase` | Estevão menciona lead manualmente | Registrar CRM + sugerir próximo passo |

## Qualificação automática

Ao receber um lead, inferir do contexto disponível:

```
Score de qualificação (0-100):

Perfil de alta performance (+40):
  - Atleta, personal trainer, piloto, médico, executivo → +40
  - Estudante de saúde/esporte → +25
  - Profissional genérico → +10

Intenção clara (+35):
  - Perguntou sobre preço ou acesso → +35
  - Comentou sobre problema específico (biomarcadores, energia, sono) → +25
  - Só entrou na lista → +10

Fonte de qualidade (+25):
  - Indicação de membro → +25
  - LinkedIn / email direto → +20
  - Instagram / orgânico → +15
  - Origem desconhecida → +5
```

## Draft de boas-vindas por perfil

Adaptar o template base conforme o perfil do lead:

**Para atleta/personal trainer:**
```
Assunto: Bem-vindo(a) à lista BodyBase, [Nome]

[Nome],

Obrigado por entrar na lista. Você está entre os primeiros a testar
a BodyBase antes do lançamento — exatamente o perfil que queremos.

Vamos te ajudar a entender o que está limitando sua performance
com base em dados reais, não suposições.

Em breve você recebe seu acesso.

Estevão
BodyBase
```

**Para executivo / foco cognitivo:**
```
Assunto: Bem-vindo(a) à lista BodyBase, [Nome]

[Nome],

Obrigado por entrar na lista.

A maioria dos executivos que chegam aqui vêm pelo mesmo caminho:
fazem tudo certo na rotina, mas ainda sentem que dá pra melhorar.
Os dados dos seus biomarcadores costumam mostrar exatamente o quê.

Em breve você recebe seu acesso prioritário.

Estevão
BodyBase
```

**Para lead genérico:**
```
Assunto: Bem-vindo(a) à lista BodyBase, [Nome]

[Nome],

Você está na lista. Acesso por ordem de chegada.

Qualquer dúvida, responda este email.

Estevão
BodyBase
```

## Notificação no topic `bodybase`

Para cada lead novo, postar:
```
🏋️ Lead novo — [Nome]
Perfil: [inferido]
Origem: [canal]
Score: [XX]/100
Ação: draft em rascunhos / aguardando mais info
```

## Follow-up automático

Se lead ficou sem resposta por X dias após contato inicial:

| Status | Dias sem resposta | Ação |
|--------|------------------|------|
| Novo | 3 dias | Alerta no topic bodybase |
| Contatado | 7 dias | Sugerir follow-up no topic bodybase |
| Qualificado | 14 dias | Alerta prioritário — possível desistência |

**Draft de follow-up (suave):**
```
[Nome],

Só passando para ver se teve alguma dúvida sobre a BodyBase.

Estou à disposição.

Estevão
```

## Atualização do CRM

Após cada interação com lead:
1. Atualizar `status` no Sheets
2. Adicionar nota com resumo da interação
3. Recalcular score
4. Atualizar `ultimo_contato`
5. Definir `next_action`

## O que nunca fazer

- Nunca prometer data de lançamento
- Nunca dar desconto sem aprovação do Estevão
- Nunca revelar número de pessoas na waitlist
- Nunca comparar diretamente com concorrentes pelo nome
