# 🧱 Arquitetura do Sistema de Atendimento Multiagente

Este documento descreve a arquitetura técnica do sistema de **equipe de agentes de atendimento via WhatsApp**, integrando **IA e humanos** em um único fluxo operacional, com governança, escalabilidade e controle de estado.

---

## 🎯 Objetivo da Arquitetura

Criar uma estrutura onde:

- IA e humanos trabalham juntos
- Conversas não se perdem
- Apenas um agente atende por vez
- O sistema decide quando escalar para humano
- O estado da conversa é preservado
- A operação seja escalável e auditável

---

## 🧩 Visão Geral dos Componentes

A arquitetura é baseada em **orquestração central no n8n**, com serviços especializados ao redor.


Cliente (WhatsApp)
↓
WhatsApp API
↓
n8n
↙ ↓ ↘
Chatwoot IA Banco de Dados
(Humano) (Estado)


---

## 🧠 Papel de Cada Componente

### 📡 WhatsApp API
- Canal oficial de comunicação com o cliente
- Recebe e envia mensagens
- Encaminha eventos para o n8n

---

### ⚙️ n8n (Orquestrador Central)
Responsável por:

- Receber eventos de mensagem
- Decidir se IA ou humano atende
- Executar regras de negócio
- Garantir anti-conflito de agentes
- Controlar estado da conversa
- Fazer handover automático

📌 **O n8n é o cérebro do sistema.**

---

### 🤖 Agente de IA (OpenAI)
- Primeiro nível de atendimento
- Coleta informações iniciais
- Responde dúvidas comuns
- Avalia quando escalar para humano
- Atua sempre sob regras do n8n

---

### 👤 Chatwoot (Atendimento Humano)
- Interface para agentes humanos
- Assunção manual da conversa
- Histórico completo do atendimento
- Organização por agentes e filas

📌 O Chatwoot **não decide** — ele executa decisões tomadas pelo fluxo.

---

### 🗄️ PostgreSQL
- Persistência de dados
- Histórico de conversas
- Logs de handover
- Apoio à auditoria

---

### ⚡ Redis
- Estado da conversa em tempo real
- Controle de agente ativo
- Bloqueio de múltiplos atendimentos simultâneos
- Flags de controle (IA ativa / humano ativo)

---

## 🔄 Fluxo de Atendimento (Resumo)

1. Cliente envia mensagem no WhatsApp  
2. Evento chega ao n8n  
3. n8n verifica estado da conversa  
4. IA atende ou humano assume  
5. Redis garante agente único  
6. Chatwoot exibe a conversa 
7. Estado é atualizado continuamente  

---

## 🧠 Decisões Arquiteturais Importantes

- n8n como orquestrador (não Chatwoot)
- IA nunca atua sem regras explícitas
- Redis separado do banco principal
- Um agente por conversa (anti-conflito)
- Arquitetura preparada para escalar agentes

---

## 📈 Escalabilidade

A arquitetura permite:

- Adicionar novos agentes humanos
- Criar novos agentes de IA especializados
- Expandir regras sem quebrar fluxos
- Integrar novos canais no futuro

---

## 🧠 Conclusão Arquitetural

Esta arquitetura não é apenas um fluxo de automação.

É um **sistema distribuído de atendimento**, com:

- Orquestração
- Memória
- Decisão
- Escalonamento
- Governança

---

📌 Este documento descreve exclusivamente a arquitetura da **equipe de agentes de atendimento**.
Outras automações do ecossistema são documentadas em repositórios separados.
