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

<img width="913" height="582" alt="image" src="https://github.com/user-attachments/assets/c9581e47-73a5-4d26-beb4-d7ffce13ce75" />

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

## Configuração do SSH

Foi analisado o ficheiro `/etc/ssh/sshd_config`.

Verificou-se que:

- `PasswordAuthentication` já se encontrava configurado como `no`, impedindo a autenticação por palavra-passe.
- A diretiva `PermitRootLogin` encontrava-se comentada (`#PermitRootLogin prohibit-password`), não tendo sido possível confirmar a alteração para `PermitRootLogin no` devido às limitações do ambiente do laboratório.

Estas configurações seguem as boas práticas de hardening do serviço SSH, reduzindo o risco de acessos não autorizados.

<img width="828" height="416" alt="629397554-1f0af690-61a7-4aba-80af-ccbeb9792fc6" src="https://github.com/user-attachments/assets/999fb5a8-7324-4edb-a8ff-38abd4d5087f" />

<img width="957" height="313" alt="image" src="https://github.com/user-attachments/assets/140cc6c0-a803-4eb8-98bf-5adfb05fac71" />


## Conclusão

Durante este laboratório foi efetuada uma auditoria inicial ao servidor Linux, identificando os serviços ativos, verificando a configuração das contas de utilizador e das chaves SSH autorizadas.

Foram aplicadas medidas de contenção através da ativação da firewall UFW, limitando as ligações de entrada apenas ao serviço SSH. Foi ainda analisada a configuração do serviço SSH, verificando-se que a autenticação por palavra-passe já se encontrava desativada.

Apesar das limitações do ambiente da máquina virtual, que impediram a utilização do Nmap e a conclusão de algumas alterações ao ficheiro `sshd_config`, foi possível aplicar e documentar as principais medidas de hardening previstas no laboratório.



