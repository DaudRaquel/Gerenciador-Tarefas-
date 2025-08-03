🗂️ TaskHub - Gerenciador de Tarefas Corporativo
Sistema completo de gerenciamento de tarefas com autenticação via RM (TOTVS), dashboards inteligentes, controle por equipe e colaborador, envio de notificações por e-mail e interface interativa desenvolvida com Flask e Tailwind CSS.

💻 Tecnologias Utilizadas
Backend

Python 3.10+ com Flask
Banco de Dados:
MySQL (armazenamento principal)
Integração Oracle via cx_Oracle (TOTVS RM)


Email SMTP com autenticação para notificações automáticas
Sessões com Flask Session para controle de login

Frontend

HTML5 + Tailwind CSS (design responsivo e moderno)
JavaScript puro (interações e chamadas dinâmicas)
Chart.js (gráficos no dashboard)
Login com saudação horária personalizada


🔐 Autenticação Inteligente

Login via Chapa + CPF (verificação com RM)
Cadastro automático no banco MySQL se não existir
Validação de CPF com 11 dígitos
Sessões seguras para acesso às funcionalidades


🎯 Funcionalidades Principais
👥 Gestão de Colaboradores

Cadastro automático a partir do RM
Sugestões de autocomplete para chapas/nomes
Detecção e exibição por setor
Envio de e-mail de boas-vindas

📝 Gestão de Tarefas

Criação de tarefas por título, data, responsável e descrição
Upload de arquivos (anexos)
Status da tarefa: Não Iniciado, Pendente, Em Retorno, Concluído
Retorno (feedback) editável pelo responsável
Notificações visuais e por e-mail sobre status e retornos
Exclusão com confirmação (autorizados)

📊 Dashboards e Organização

Saudação dinâmica com nome do usuário logado
Cards e cores personalizadas por departamento
Lista de tarefas filtradas por setor e status
Cards de colaboradores com avatar baseado no nome


✉️ Notificações e E-mails

Envio automático de e-mail ao criar tarefa
E-mail com detalhes da tarefa e links
Atualização de status gera notificação e e-mail
Retornos adicionados também geram alertas
Adição de sino de notificação visual


🖼️ Demonstração Visual



Funcionalidade
Captura
Descrição



Login e Boas-Vindas

Interface com saudação horária personalizada


Dashboard

Tarefas por setor com destaque por status


Cards de Colaboradores

Avatar automático e visualização por setor


Sino de Notificação

Indicador visual de notificações


Lista de Tarefas

Exibição por equipe e com ações rápidas



🔍 Dica: Certifique-se de que o repositório RaquelDaud180/Gerenciador-Tarefas- esteja público e que os arquivos estejam na raiz com os nomes exatos listados.


📂 Estrutura da Aplicação

app/
static/
css/ (estilos Tailwind CSS)
js/ (scripts JavaScript)
images/ (imagens do projeto)


templates/
login.html
dashboard.html
tasks.html


__init__.py


config/
database.py (conexão MySQL e Oracle)
email.py (configuração SMTP)


requirements.txt (dependências Python)
README.md
run.py (execução do Flask)
