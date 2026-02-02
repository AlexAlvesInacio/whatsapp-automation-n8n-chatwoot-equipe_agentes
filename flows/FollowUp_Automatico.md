# 🔄 Flow — Gestão da Equipe de Agentes de Atendimento

Este documento descreve o **fluxo principal de funcionamento da equipe de agentes (IA + humanos)** implementado no n8n.

O foco deste flow é **controle de atendimento**, **roteamento inteligente** e **garantia de agente único por conversa**.

---

## 🎯 Objetivo do Flow

- Garantir que cada conversa tenha **apenas um agente ativo**
- Alternar corretamente entre **IA e humano**
- Evitar conflitos de resposta
- Preservar estado da conversa
- Permitir escalabilidade do atendimento

---

## 🧩 Visão Geral do Fluxo

Fluxo executado sempre que:

- Uma nova mensagem chega via WhatsApp
- O estado da conversa muda
- O agente é transferido

Componentes principais:
- n8n
- Chatwoot
- WhatsApp API
- Redis (estado)
- PostgreSQL (histórico)

---

## 🚦 Etapa 1 — Recebimento da Mensagem

1. O WhatsApp dispara um evento
2. O n8n recebe a mensagem
3. A conversa é identificada
4. O estado atual é consultado

📌 Nenhuma mensagem é processada sem validação de estado.

---

## 🧠 Etapa 2 — Decisão de Agente

O sistema avalia:

- Existe agente humano ativo?
- Existe lock de atendimento?
- A conversa está em estado inicial?

### Possíveis decisões:
- Direcionar para IA
- Manter humano ativo
- Bloquear execução
- Solicitar handover

---

## 🤖 Etapa 3 — Atendimento por IA

Quando a IA é responsável:

- A IA responde mensagens iniciais
- Coleta informações básicas
- Classifica intenção
- Pode solicitar escalonamento

A IA **não assume** conversas com humano ativo.

---

## 👤 Etapa 4 — Atendimento Humano

Quando um humano assume:

1. O estado da conversa muda para

