🔐 Laboratório de Segurança - Brute Force em MySQL e Mitigação com UFW
📌 Descrição

Este projeto demonstra, em ambiente controlado, uma simulação de ataque de força bruta em um serviço MySQL exposto, seguido da aplicação de medidas de mitigação para aumento da segurança.

O objetivo é evidenciar, na prática, como vulnerabilidades podem ser exploradas, permitindo acesso indevido ao sistema, e posteriormente corrigidas através de técnicas de hardening.

🧪 Ambiente do Laboratório
Máquina atacante: Kali Linux (Hyper-V)
Máquina alvo: Ubuntu Server (Hyper-V)
🔧 Ferramentas utilizadas
Nmap
Metasploit Framework
MySQL Server
UFW (Uncomplicated Firewall)
⚙️ Configuração do Cenário
🔓 Estado inicial (vulnerável)
Serviço MySQL ativo na porta 3306
Acesso externo permitido
Firewall permitindo conexões (ALLOW 3306)
Usuário e senha configurados no banco
🚨 Teste de Vulnerabilidade
🔍 1. Enumeração com Nmap
nmap -sS -sV 192.168.100.154

✔ Resultado:

Porta 3306 aberta
Serviço MySQL identificado
Versão detectada
📸 Evidência




💣 2. Ataque de força bruta com Metasploit
use auxiliary/scanner/mysql/mysql_login
set RHOSTS 192.168.100.154
run

✔ Resultado:

Diversas tentativas de login
Credenciais válidas encontradas
🔗 3. Acesso via sessão
sessions -i 6

✔ Resultado:

Sessão MySQL aberta
Acesso autenticado ao banco
📸 Evidência




🧠 4. Interação com o banco
show databases;

✔ Resultado:

Listagem de bancos
Confirmação de acesso indevido
🛡️ Mitigação
🔒 5. Verificação inicial
ufw status

✔ Resultado:

Porta 3306 liberada (ALLOW)
🔒 6. Bloqueio da porta
sudo ufw deny 3306/tcp

✔ Resultado:

Regra aplicada com sucesso
🔒 7. Verificação após bloqueio
ufw status

✔ Resultado:

Porta 3306 bloqueada (DENY)
📸 Evidência (Antes e Depois)




🛡️ 8. Validação da mitigação

Nova tentativa de ataque:

✔ Resultado:

Conexão falha
Nenhuma sessão ativa
Acesso externo bloqueado
📸 Evidência




🔄 Resultado Final
Situação	Resultado
Antes	Acesso não autorizado possível
Depois	Acesso bloqueado com sucesso
⚠️ Aviso Legal

Este projeto foi realizado em ambiente controlado, com fins exclusivamente educacionais.
Nenhuma atividade foi executada em sistemas de terceiros ou sem autorização.

🚀 Aprendizados
Identificação de serviços expostos
Enumeração com Nmap
Exploração de autenticação fraca
Uso do Metasploit
Acesso via sessão ativa
Hardening com UFW
Validação de mitigação
👨‍💻 Autor

Wladimir Eduardo
🔐 Entusiasta de Cibersegurança

📎 Próximos passos
Implementar Fail2Ban
Restringir acesso por IP
Melhorar política de senhas
Monitoramento de logs
Defesa em camadas