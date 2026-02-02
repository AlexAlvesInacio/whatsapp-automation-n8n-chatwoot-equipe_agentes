# 🧱 Stack de Infraestrutura — Equipe de Agentes de Atendimento

Este documento descreve a **infraestrutura técnica** utilizada para operar a equipe de agentes (IA + humanos) em produção.

O foco da stack é:
- Estabilidade
- Escalabilidade
- Controle de estado
- Separação clara de responsabilidades

---

## 🌐 Visão Geral da Infraestrutura

A solução opera em uma **VPS dedicada**, utilizando containers Docker para isolar serviços e facilitar manutenção, escalabilidade e monitoramento.

Todos os serviços são orquestrados de forma independente, porém integrados por APIs e eventos.

---

## 🖥️ Ambiente de Execução

- **Servidor:** VPS dedicada
- **Sistema Operacional:** Ubuntu 24.04 LTS
- **Provedor:** Hostinger
- **Gerenciamento de Containers:** EasyPanel

---

## 🐳 Camada de Containers (Docker)

Cada serviço roda em seu próprio container:

- n8n
- Chatwoot
- Evolution API (WhatsApp)
- PostgreSQL
- Redis

Isso garante:
- Isolamento
- Atualizações seguras
- Rollback rápido
- Menor risco operacional

---

## ⚙️ Serviços da Stack

### 🔄 n8n — Orquestração de Fluxos
Responsável por:
- Receber eventos do WhatsApp
- Executar regras de negócio
- Controlar fluxo de agentes
- Gerenciar handovers
- Integrar sistemas

---

### 💬 Chatwoot — CRM e Atendimento Humano
Responsável por:
- Interface de atendimento
- Gestão de conversas
- Atribuição de agentes humanos
- Histórico de mensagens

---

### 📡 Evolution API — Integração WhatsApp
Responsável por:
- Conexão com WhatsApp
- Envio e recebimento de mensagens
- Gestão de instâncias
- Eventos em tempo real

---

### 🧠 PostgreSQL — Persistência de Dados
Responsável por:
- Armazenar histórico de atendimentos
- Registrar transferências
- Registrar estados e eventos
- Base para auditoria

---

### ⚡ Redis — Controle de Estado e Lock
Responsável por:
- Lock de conversas
- Controle de agente ativo
- Evitar concorrência
- Garantir agente único por conversa

---

## 🔐 Segurança e Confiabilidade

- Serviços isolados por container
- Variáveis sensíveis protegidas por ambiente
- Sem dados críticos expostos em código
- Possibilidade de backup automático

---

## 📈 Escalabilidade

A stack permite:
- Aumento do número de agentes
- Separação de ambientes (dev / prod)
- Inclusão de novos fluxos
- Expansão para IA mais avançada

---

## 🧠 Conclusão

Esta stack foi projetada para **operações reais**, não apenas testes.

Ela sustenta:
- Atendimento contínuo
- IA controlada
- Humanos com contexto
- Crescimento sem reescrita de sistema

📌 O valor da solução está na **arquitetura integrada**, não em uma ferramenta isolada.

