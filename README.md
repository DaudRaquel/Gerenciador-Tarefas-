# 🗂️ TaskHub — Gerenciador de Tarefas Corporativo

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-Backend-lightgrey?logo=flask)
![MySQL](https://img.shields.io/badge/MySQL-Database-blue?logo=mysql)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-UI-38B2AC?logo=tailwindcss)
![Chart.js](https://img.shields.io/badge/Chart.js-Graphs-ff6384?logo=chartdotjs)
![Socket.IO](https://img.shields.io/badge/Realtime-Socket.IO-black?logo=socketdotio)
![License](https://img.shields.io/badge/License-MIT-green)

> Plataforma web para **criar, acompanhar e analisar tarefas** por **colaborador, setor e grupos**, com **notificações**, **chat integrado**, **anexos** e **dashboard interativo**.  
> **Status de integração:** atualmente **não há vínculo com o TOTVS RM** — os colaboradores são geridos **localmente (MySQL)**.

---

## 📚 Sumário

- [Sobre o Projeto](#-sobre-o-projeto)
- [Novidades (versão atual)](#-novidades-versão-atual)
- [Principais Funcionalidades](#-principais-funcionalidades)
- [Demonstração Visual](#-demonstração-visual)
- [Arquitetura (Mermaid)](#-arquitetura-mermaid)
- [API HTTP](#-api-http)
- [Eventos Socket.IO](#-eventos-socketio)
- [Stack Técnica](#-stack-técnica)
- [Configuração (.env)](#-configuração-env)
- [Como Rodar](#️-como-rodar)
- [Estrutura de Tabelas (resumo)](#️-estrutura-de-tabelas-resumo)
- [Boas Práticas de Segurança](#-boas-práticas-de-segurança)
- [Roadmap](#-roadmap)
- [Licença](#-licença)

---

## 💡 Sobre o Projeto

O **TaskHub** centraliza a operação diária entre setores: crie tarefas (individuais, em grupo ou colaborativas), **anexe arquivos**, converse por **chat**, receba **notificações** e acompanhe **KPIs e gráficos** com **Chart.js** (inclui `chartjs-plugin-datalabels` para rótulos legíveis). Interface **responsiva** com **TailwindCSS** e suporte a **tempo real** via **Socket.IO** (com **fallback por polling**).

---

## ✨ Novidades (versão atual)

- 🔑 **Login por Chapa + Senha** (credenciais do próprio sistema)
- ✉️ **Recuperação de senha por e-mail** (token temporário)
- 👥 **Tarefas com múltiplos responsáveis** e/ou **visibilidade por Departamento/Grupo**
- 📎 **Anexos** na criação e nos **retornos** (histórico com link)
- 🔔 **Notificações internas** (painel + ícone) e **e-mails de evento**
- 💬 **Chat corporativo**: **salas por grupo** e **DMs**, presença online, envio de mídia e **“chamar atenção” (poke)**
- ⚡ **Tempo real com Socket.IO** + **fallback por polling** (garante entrega das mensagens)
- 🔍 **Busca global** (ID, título, descrição, departamento)
- 🗒️ **Notas internas** por tarefa
- 📊 **Dashboard (aba “Dados”)** com filtros de **Ranking (Mais/Menos Entregas)**, **Funcionário** e **Grupo/Departamento**, e **gráficos on-click** que abrem modal com as tarefas

---

## 🚀 Principais Funcionalidades

### Autenticação & Conta
- Cadastro (Chapa, CPF, e-mail, senha)  
- Login por **Chapa + Senha**  
- Esqueci minha senha (token + redefinição segura)

### Tarefas
- Tipos: **Individual**, **Grupo Simples** (uma tarefa compartilhada), **Colaborativa** (uma por responsável, agrupadas por `id_group`)  
- Status: **Não Iniciado**, **Pendente**, **Em Retorno**, **Concluído**  
- **Anexos** em criação e nos **retornos**  
- Filtros por **status**, **responsável** e **grupo** + busca por texto

### Chat & Notificações
- **Salas de grupo** e **mensagens privadas (DM)**  
- Emojis, imagens/arquivos, presença online, **poke**  
- Notificações internas e e-mails transacionais

### Dashboard & KPIs
- Filtros: **Ranking**, **Funcionário**, **Grupo/Departamento**  
- **Gráficos interativos** (abre modal com lista de tarefas ao clicar)  
- Rótulos legíveis com **Datalabels**

### Navegação
- **Tarefas** (cadastro/edição/listas)  
- **Dados** (dashboard e KPIs)  
- **Chat**  
- **Notificações**  
- **Sair**

---

## 🖼️ Demonstração Visual

| Funcionalidade | Captura | Descrição |
|---|---|---|
| **Login e Boas-Vindas** | ![Boas-Vindas](https://github.com/RaquelDaud180/Gerenciador-Tarefas-/blob/main/boasvindas.png) | Saudação horária personalizada |
| **Dashboard (Setores e Colaboradores)** | ![Dashboard](https://github.com/RaquelDaud180/Gerenciador-Tarefas-/blob/main/dashboard.png) | Visão geral por setor/colaborador |
| **Cards por Status** | ![Cards](https://github.com/RaquelDaud180/Gerenciador-Tarefas-/blob/dd83d092e687c8f3ffa587b597860062fc5904c0/Imagem%20do%20WhatsApp%20de%202025-08-02%20%C3%A0(s)%2020.58.41_6532e51a.jpg) | Destaque por status e setor |
| **Retornos/Notificações** | ![Retornos](https://github.com/RaquelDaud180/Gerenciador-Tarefas-/blob/dd83d092e687c8f3ffa587b597860062fc5904c0/Imagem%20do%20WhatsApp%20de%202025-08-02%20%C3%A0(s)%2020.56.24_53f55fc5.jpg) | Histórico com anexos e mudanças |

---

## 🧭 Arquitetura (Mermaid)

```mermaid
flowchart LR
  A[Frontend (HTML • Tailwind • JS • Chart.js)] <--> B[Flask API]
  A <-- Socket.IO / Polling --> C[Realtime (Socket.IO)]
  B <--> D[(MySQL)]
  B --> E[SMTP (e-mails)]
  B --> F[Storage de Anexos]

  subgraph App Server
    B
    C
  end
```

---

## 🔌 API HTTP

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/` | Página inicial (login / boas-vindas) |
| `GET` | `/task_manager` | Interface principal de tarefas |
| `POST` | `/login` | Autenticação por chapa + senha |
| `POST` | `/registrar-conta` | Criação de conta |
| `POST` | `/recuperar-senha` | Gera e envia token de recuperação |
| `GET, POST` | `/nova-senha/<token>` | Form + submissão de nova senha |
| `POST` | `/salvar-email` | Atualiza e-mail do usuário |
| `GET` | `/colaboradores` | Lista de colaboradores |
| `GET` | `/search_collaborators` | Busca (auto-complete) de colaboradores |
| `POST` | `/create_collaborator` | Cria colaborador |
| `DELETE` | `/delete_collaborator/<colab_id>` | Exclui colaborador |
| `POST` | `/create_task` | Cria tarefa (inclui anexos) |
| `POST` | `/change_status/<task_id>` | Altera status da tarefa |
| `DELETE` | `/delete_task/<task_id>/` | Exclui tarefa |
| `POST` | `/save_return` | Adiciona **retorno** (texto + anexos) |
| `GET` | `/download_return/<path:filename>` | Baixa anexo de retorno |
| `GET` | `/download_anexo/<int:tarefa_id>` | Baixa anexo da tarefa |
| `GET` | `/get_task_details` | Detalhes da tarefa (para modal) |
| `POST` | `/save_internal_note` | Salva nota interna da tarefa |
| `POST` | `/update_status` | Atualiza status (alternativa) |
| `GET` | `/get_data` | Dados do dashboard/KPIs |
| `GET` | `/get_notifications/<chapa>` | Lista notificações do usuário |
| `POST` | `/mark_as_read` | Marca notificações como lidas |
| `GET` | `/chat/history` | Histórico de sala (grupo) |
| `GET` | `/chat/private_history` | Histórico de DM |
| `POST` | `/delete_all` | Administração: remover tudo |
| `POST` | `/api/validate` | Validação de colaborador (API interna) |
| `POST` | `/logout` | Encerra sessão |

---

## 📡 Eventos Socket.IO

| Evento | Direção | Payload | Descrição |
|---|---|---|---|
| `connect` | servidor⇐cliente | – | Conexão estabelecida |
| `disconnect` | servidor⇐cliente | – | Desconexão |
| `identify` | cliente⇒servidor | `{chapa}` | Identifica usuário conectado |
| `join` | cliente⇒servidor | `{room}` | Entrar em sala de grupo |
| `leave` | cliente⇒servidor | `{room}` | Sair de sala |
| `send_message` | cliente⇒servidor | `{room, text, [media]}` | Mensagem em sala |
| `private_message` | cliente⇒servidor | `{to, text, [media]}` | Mensagem privada (DM) |
| `poke_user` | cliente⇒servidor | `{to}` | “Chamar atenção” (notifica destino) |

> **Fallback por polling**: se WebSocket/Socket.IO não estiver disponível, o cliente usa requisições periódicas para manter o histórico sincronizado.

---

## 🧱 Stack Técnica

- **Backend:** Python (Flask), Flask-SocketIO, APScheduler, SMTP (e-mails)  
- **Banco de Dados:** MySQL  
- **Frontend:** HTML, TailwindCSS, JavaScript, Chart.js (+ `chartjs-plugin-datalabels`)  
- **Tempo real:** Socket.IO com fallback por polling  
- **Armazenamento de arquivos:** diretório configurável para anexos

---

## ⚙️ Configuração (.env)

```bash
# Flask
FLASK_ENV=production
SECRET_KEY=<chave-super-secreta>

# App
BASE_URL=https://seu-dominio-ou-ip
UPLOAD_FOLDER=/caminho/para/anexos

# MySQL
DB_HOST=localhost
DB_PORT=3306
DB_USER=<usuario>
DB_PASSWORD=<senha>
DB_NAME=<nome_do_banco>

# SMTP (e-mails transacionais)
SMTP_HOST=<smtp.exemplo.com>
SMTP_PORT=465
EMAIL_ADDRESS=<no-reply@exemplo.com>
EMAIL_PASSWORD=<senha>

# (Opcional) Controle de acesso
AUTHORIZED_USERS=<chapa1,chapa2,...>

# (Opcional) Cores por departamento (JSON)
DEPARTMENT_COLORS={"Financeiro":"#0ea5e9","RH":"#22c55e","TI":"#a78bfa"}
```

> **Nunca** versione credenciais. Use `.env`/secret manager.

---

## ▶️ Como Rodar

```bash
# 1) Ambiente
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate
pip install -r requirements.txt

# 2) Configurar variáveis
cp .env.example .env  # ou crie .env com as variáveis acima

# 3) Banco de dados
# Crie o schema e as tabelas (ver resumo abaixo)

# 4) Executar (desenvolvimento)
flask run

# Produção (exemplos):
# gunicorn -k eventlet -w 1 "app:app"
# ou:
# python -m eventlet -m app
```

---

## 🗄️ Estrutura de Tabelas (resumo)

> Ajuste conforme seu schema atual.

- `colaboradores` — (id, chapa, nome, email, ramal, equipe_id, criado_em)  
- `equipes` — (id, nome)  
- `tarefas` — (id, titulo, descricao, tipo, status, criador_id, id_group, criado_em, atualizado_em)  
- `tarefas_responsaveis` — (tarefa_id, colaborador_id)  
- `tarefas_departamentos` — (tarefa_id, equipe_id)  
- `retornos` — (id, tarefa_id, autor_id, mensagem, criado_em)  
- `anexos` — (id, referencia_id, tipo_ref, nome_arquivo, caminho, criado_em)  
- `mensagens` — (id, room, autor_id, texto, midia_url, criado_em)  
- `mensagens_privadas` — (id, de_id, para_id, texto, midia_url, criado_em)  
- `notificacoes` — (id, usuario_id, tipo, payload, lida, criado_em)  
- `recovery_tokens` — (id, usuario_id, token, expira_em, usado)

**Índices sugeridos**  
- `tarefas(status)`, `tarefas(criado_em)`, `tarefas(id_group)`  
- `tarefas_responsaveis(tarefa_id, colaborador_id)`  
- `tarefas_departamentos(tarefa_id, equipe_id)`  
- `mensagens(room, criado_em)`, `mensagens_privadas(de_id, para_id, criado_em)`

---

## 🔐 Boas Práticas de Segurança

- **Sanitizar uploads** (extensão/tamanho) e armazenar fora de diretórios públicos quando possível  
- **Hash de senhas** forte (ex.: `bcrypt`/`argon2`) e expiração de **tokens**  
- **Rate-limit** em endpoints sensíveis (login, recuperação, chat)  
- **Logs estruturados** + auditoria de mudanças de status  
- **CORS/CSRF** conforme o cenário de implantação

---

## 🗺️ Roadmap

- 📌 **Kanban** arrastável por status  
- 🧩 **Menções (@usuario)** e **notificações por evento**  
- 🔐 **Papéis/Permissões** (admin/gestor/colaborador)  
- 📊 **Relatórios (PDF/Excel)** e métricas de produtividade  
- 🔗 (Opcional) **Integrações externas** via API/ETL

