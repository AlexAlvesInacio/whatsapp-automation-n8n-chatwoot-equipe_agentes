# 📜 Regras de Negócio — Equipe de Agentes de Atendimento

Este documento descreve as **regras de negócio que governam o funcionamento da equipe de agentes (IA + humanos)** no sistema de atendimento via WhatsApp.

O objetivo é garantir **controle, organização, escalabilidade e previsibilidade** no atendimento.

---

## 🎯 Princípios Gerais

1. Cada conversa deve ter **apenas um agente ativo**
2. IA e humano **nunca atendem simultaneamente**
3. O sistema decide — o agente executa
4. O estado da conversa é sempre preservado
5. Toda decisão deve ser auditável

---

## 🤖 Regra 1 — Entrada de Mensagem

**Quando uma mensagem chega pelo WhatsApp:**

1. O n8n recebe o evento
2. O sistema verifica o estado da conversa
3. Define quem é o agente ativo:
   - IA
   - Humano
4. Direciona a mensagem corretamente

📌 Nenhuma mensagem é respondida sem validação de estado.

---

## 🧠 Regra 2 — Atuação da IA

A IA atua quando:

- Não existe agente humano ativo
- A conversa está em estado inicial
- Não houve handover manual

A IA pode:

- Coletar informações
- Responder dúvidas simples
- Classificar intenção
- Sugerir escalonamento

A IA **não pode**:

- Assumir conversa já atribuída a humano
- Alterar estado sem autorização do fluxo
- Encerrar atendimento crítico sozinha

---

## 👤 Regra 3 — Assunção por Agente Humano

Um agente humano assume a conversa quando:

- O sistema detecta intenção complexa
- A IA sinaliza necessidade de escalonamento
- O agente assume manualmente via Chatwoot

Ao assumir:

1. A IA é automaticamente desativada
2. O estado da conversa muda para `humano_ativo`
3. Apenas o agente humano pode responder

---

## 🔒 Regra 4 — Controle de Agente Único (Anti-Conflito)

Para evitar conflitos:

- Cada conversa possui um **lock ativo**
- O lock é controlado via Redis
- Enquanto o lock existir:
  - Nenhum outro agente responde
  - Nenhuma IA interfere

📌 Isso evita respostas duplicadas ou conflitantes.

---

## 🔁 Regra 5 — Persistência de Estado

O sistema mantém:

- Estado atual da conversa
- Agente responsável
- Histórico de handovers
- Logs de decisão

Persistência ocorre em:

- Redis → estado em tempo real
- PostgreSQL → histórico e auditoria

---

## 🔄 Regra 6 — Retorno da IA (Opcional)

A IA pode voltar a atuar quando:

- O humano encerra o atendimento
- O estado da conversa é resetado
- Não há lock humano ativo

📌 Essa regra é opcional e configurável por fluxo.

---

## 🚫 Regra 7 — Bloqueios e Segurança

O sistema bloqueia automaticamente:

- Dupla resposta (IA + humano)
- Dois humanos no mesmo atendimento
- Execução fora do fluxo definido
- Respostas sem estado válido

---

## 📊 Regra 8 — Observabilidade e Auditoria

Todas as ações relevantes geram:

- Logs de estado
- Registro de handover
- Histórico de agente
- Timestamp de decisões

Isso permite:

- Auditoria
- Debug
- Evolução do sistema
- Análise de performance

---

## 🧠 Conclusão

Estas regras transformam o atendimento em um **sistema governado por lógica**, não por improviso.

O resultado é:

- Atendimento organizado
- Escalonamento previsível
- IA sob controle
- Humanos com contexto
- Sistema pronto para escalar

📌 Este documento descreve exclusivamente as regras da **equipe de agentes de atendimento**.
