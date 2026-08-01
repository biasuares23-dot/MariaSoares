# Laboratório – Sessão 6
## Mini-CTF Defensivo Linux

### Objetivo
Auditar um servidor Linux, identificar vulnerabilidades, aplicar medidas de contenção, reforçar a configuração de segurança e validar as alterações efetuadas.

## Ambiente utilizado
TryHackMe — Linux Incident Surface

---

# Fase 1 – Identificação e Triagem

## 1. Análise de Rede e Portas

### Comando executado

```bash
ss -tuln
```

### Resultado

Foram identificados os seguintes serviços ativos:

- Porta **22/TCP** – SSH
- Porta **80/TCP** – HTTP
- Porta **631** – CUPS (localhost)
- Porta **5901** – VNC (localhost)
- Porta **53** – DNS local
- Porta **5353** – mDNS

**Análise:**

Verificou-se que apenas os serviços SSH (22) e HTTP (80) estavam expostos à rede. Os restantes serviços encontravam-se limitados ao localhost.

<img width="1032" height="802" alt="image" src="https://github.com/user-attachments/assets/21d659ab-8ed4-4438-bd50-a2a695ae1ae1" />

---

## 2. Análise com Nmap

### Comando

```bash
nmap -sV localhost
```

### Resultado

O comando nmap -sV localhost não pôde ser executado porque a ferramenta não se encontrava instalada na máquina disponibilizada e a instalação não foi possível devido à indisponibilidade dos repositórios. A identificação inicial dos serviços foi realizada com o comando 'ss -tuln'.

A configuração da firewall implementou uma política de "negação por defeito" (deny incoming), permitindo apenas o acesso remoto através da porta 22/TCP. Esta medida reduz significativamente a superfície de ataque do servidor.

<img width="1030" height="750" alt="image" src="https://github.com/user-attachments/assets/e907be7e-73fd-44a4-93d9-89ebabce223d" />

---

## 3. Auditoria de Contas

### Comando

```bash
sudo cat /etc/shadow | awk -F: '($2==""){print $1}'
```

### Resultado

O comando não apresentou qualquer utilizador.

**Análise**

Não foram identificadas contas sem palavra-passe configurada.

<img width="915" height="250" alt="image" src="https://github.com/user-attachments/assets/e7f49bb6-7430-4b10-8fee-83b7493efb75" />

---

## 4. Chaves SSH

### Comando

```bash
cat ~/.ssh/authorized_keys
```

### Resultado

Foram identificadas múltiplas chaves públicas SSH autorizadas no ficheiro authorized_keys. A existência de chaves de utilizadores ou sistemas externos deve ser validada, removendo chaves antigas ou não autorizadas para reduzir risco de acesso persistente.

<img width="920" height="497" alt="image" src="https://github.com/user-attachments/assets/e3800565-998c-44f4-82b6-400949ca6f26" />

---

# Fase 2 – Contenção

## Ativação da Firewall UFW

### Comandos executados

```bash
sudo ufw default deny incoming
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

### Resultado

- Firewall ativada.
- Política padrão alterada para bloquear ligações de entrada.
- Apenas a porta SSH (22/TCP) permaneceu autorizada.

**Análise**

A ativação da firewall reduziu a superfície de ataque do servidor, permitindo apenas o acesso remoto por SSH.

<img width="973" height="358" alt="image" src="https://github.com/user-attachments/assets/447fe40c-515e-4a15-8cf2-656663f73ff9" />

<img width="828" height="262" alt="status verbose" src="https://github.com/user-attachments/assets/2ba5bf3b-7304-4245-957c-9cf27639ec9d" />

---

# Fase 3 – Remediação

**Verificação do login do utilizador root** 
Comando executado
'grep -n "PermitRootLogin" /etc/ssh/sshd_config'

**Desativação da autenticação por palavra passe**
Comando excutado
'grep -n "PasswordAuthentication" /etc/ssh/sshd_config'

Foi confirmado que a autenticação por palavra-passe está desativada.

Esta configuração aumenta a segurança do serviço SSH, pois impede ataques de força bruta baseados em passwords, obrigando a utilização de autenticação por chave SSH.
**Verificação da autenticação por chave pública**
Comando executado
'grep -n "PubkeyAuthentication" /etc/ssh/sshd_config'

OBS.: A auditoria automática com Lynis não foi executada devido à indisponibilidade da ferramenta e limitações de rede do laboratório.

<img width="913" height="423" alt="image" src="https://github.com/user-attachments/assets/aa366c26-f13b-451c-be77-3bed14028b24" />




