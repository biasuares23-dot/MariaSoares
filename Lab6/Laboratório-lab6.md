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

Foram encontradas quatro chaves públicas autorizadas.

**Análise**

As chaves pertencem ao ambiente da TryHackMe e não foram identificados indícios de chaves suspeitas.

<img width="927" height="532" alt="image" src="https://github.com/user-attachments/assets/14bf8bb1-d97f-4aa6-a093-12495ed85959" />

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

<img width="828" height="416" alt="image" src="https://github.com/user-attachments/assets/1f0af690-61a7-4aba-80af-ccbeb9792fc6" />

> Inserir print do `sudo ufw status verbose`.

---

# Fase 3 – Remediação

## Configuração do SSH

Verificou-se que:

- `PasswordAuthentication` já se encontrava definida como **no**.
- `PermitRootLogin` encontrava-se comentada, sendo necessária a sua revisão para cumprir as boas práticas de segurança.














# Laboratório – Sessão 6





## Fase 3 – Remediação

### Hardening do SSH

- PermitRootLogin no
- PasswordAuthentication no

<img width="828" height="416" alt="image" src="https://github.com/user-attachments/assets/1f0af690-61a7-4aba-80af-ccbeb9792fc6" />


---

## Validação

Execução do Lynis

(print)

Hardening Index

---

## Conclusão

O sistema apresentou diversas configurações inseguras que foram corrigidas através da ativação da firewall, endurecimento da configuração SSH e aplicação das recomendações de segurança. Após a execução do Lynis verificou-se uma melhoria da postura de segurança do servidor.




