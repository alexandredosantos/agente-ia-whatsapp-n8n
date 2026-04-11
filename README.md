# 🤖 Agente de Atendimento de IA no WhatsApp — Advocacia

**`n8n | WhatsApp | OpenAI | Redis | Supabase | Google Calendar | MCP`**

Agente conversacional completo desenvolvido para escritórios de advocacia, integrando **atendimento inteligente via WhatsApp**, transcrição de áudio, memória de conversa e **agendamento automático no Google Calendar** — tudo sem intervenção humana.

---

## 🎯 Objetivo

Automatizar o primeiro atendimento de clientes via WhatsApp com:

- Respostas automáticas via IA (OpenAI)
- Transcrição e interpretação de mensagens de **áudio**
- Agrupamento inteligente de mensagens enviadas em sequência (buffer Redis)
- Qualificação e registro automático de leads no banco de dados (Supabase)
- **Criação, listagem, reagendamento e cancelamento de consultas** no Google Calendar via MCP
- Pausa automática do bot quando o advogado intervém manualmente

---

## 🛠 Tecnologias

| Tecnologia | Uso no projeto |
|---|---|
| **n8n** | Orquestração de todo o fluxo |
| **Evolution API** | Integração com WhatsApp |
| **OpenAI (GPT + Whisper)** | Respostas da IA + transcrição de áudio |
| **Redis** | Buffer de mensagens por número |
| **Supabase (PostgreSQL)** | Banco de dados de leads |
| **Google Calendar (MCP)** | Agendamento de consultas |
| **Webhooks** | Gatilho de entrada das mensagens |

---

## 🔄 Fluxo completo da automação

```
Cliente envia mensagem (texto ou áudio)
        ↓
Webhook EVO recebe e extrai dados (nome, número, mensagem)
        ↓
Verifica se é mensagem do próprio advogado
  → Se sim: pausa o bot por X segundos (intervenção humana)
  → Se não: continua
        ↓
Filtra tipo de mensagem
  → Texto: extrai diretamente
  → Áudio: converte para .ogg → transcreve via Whisper (OpenAI)
        ↓
Empurra mensagem no buffer Redis (por número do cliente)
        ↓
Aguarda 15 segundos (agrupa mensagens enviadas em sequência)
        ↓
Coleta e limpa buffer → junta todas as mensagens em uma só
        ↓
Verifica se lead existe no Supabase
  → Se não existe: cria o lead automaticamente
        ↓
AI Agent processa com memória de conversa (histórico por sessão)
        ↓
Ferramentas disponíveis para o agente:
  • criar_evento       → agenda consulta no Google Calendar
  • listar_eventos     → consulta horários disponíveis
  • reagendar          → altera data/hora de consulta existente
  • deletar_evento     → cancela consulta
        ↓
Resposta dividida em partes (evita mensagens gigantes)
        ↓
Enviada sequencialmente via Evolution API no WhatsApp
```

---

## 🧠 Destaques técnicos

**Buffer Redis com janela de tempo**
Evita que o agente responda cada mensagem separadamente quando o cliente envia várias em sequência. Todas são agrupadas e processadas juntas após 15 segundos de inatividade.

**Transcrição de áudio com Whisper**
O agente aceita mensagens de voz. O áudio é convertido para `.ogg`, transcrito via OpenAI Whisper e processado como texto normal.

**Memória de conversa por sessão**
O AI Agent mantém histórico da conversa por número de telefone, permitindo contexto contínuo entre mensagens.

**Controle de intervenção humana**
Quando o advogado responde manualmente, o bot detecta a mensagem própria (`fromMe`) e se bloqueia automaticamente por um período configurável, evitando conflito entre humano e IA.

**Agendamento via MCP + Google Calendar**
O agente tem acesso direto ao Google Calendar através de ferramentas MCP, podendo criar, consultar, reagendar e cancelar consultas em tempo real durante a conversa.

---

## 🖼 Evidências do projeto

### Workflow principal
<p align="center">
  <img src="https://bbileeuopjogpsrvhagu.supabase.co/storage/v1/object/public/foto%20dr%20lucas/img1.jfif" width="700" />
</p>

### Conversa automatizada
<p align="center">
  <img src="https://bbileeuopjogpsrvhagu.supabase.co/storage/v1/object/public/foto%20dr%20lucas/img2.jfif" width="700" />
</p>

### Registro final do lead
<p align="center">
  <img src="https://bbileeuopjogpsrvhagu.supabase.co/storage/v1/object/public/foto%20dr%20lucas/img3.png" width="700" />
</p>

---

## 📁 Arquivo do workflow

O workflow completo está disponível para importação no n8n:

👉 [`Agente_para_Advocacia_-_Alexandre.json`](./Agente_para_Advocacia_-_Alexandre.json)

> **Como importar:** Abra o n8n → Menu → Import from File → selecione o arquivo `.json`  
> ⚠️ Você precisará configurar suas próprias credenciais: OpenAI, Redis, Supabase e Google Calendar.

---

## 📈 Resultados

- Atendimento 24/7 sem intervenção manual
- Redução de ~70% no tempo de resposta inicial
- Qualificação e registro automático de 100% dos leads
- Agendamentos realizados diretamente na conversa, sem formulários externos
- Zero conflitos entre bot e atendimento humano graças ao sistema de bloqueio automático

---

## 💡 Sobre o projeto

Desenvolvido por **Alexandre dos Santos Bezerra** como solução real de automação para escritórios de advocacia. O agente substitui o primeiro atendimento receptivo, qualifica o cliente e já agenda a consulta — tudo dentro do WhatsApp, sem que o cliente precise acessar nenhum outro sistema.

📧 bezerraalexandre800@gmail.com  
💼 [LinkedIn](https://www.linkedin.com/in/alexandre-dos-santos-a9aa7b292/)  
🐙 [GitHub](https://github.com/alexandredosantos)
