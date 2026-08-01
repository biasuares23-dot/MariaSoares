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

O comando ss -tuln permitiu identificar os serviços ativos e respetivas portas em escuta no sistema. A análise revelou que apenas os serviços SSH (22/TCP) e HTTP (80/TCP) estavam acessíveis externamente, enquanto os restantes serviços estavam limitados ao endereço localhost, reduzindo a exposição da superfície de ataque.

<img width="1032" height="802" alt="image" src="https://github.com/user-attachments/assets/21d659ab-8ed4-4438-bd50-a2a695ae1ae1" />

---

## 2. Análise com Nmap

### Comando

```bash
nmap -sV localhost
```

### Resultado

O comando nmap -sV localhost não pôde ser executado porque a ferramenta não se encontrava instalada na máquina disponibilizada e a instalação não foi possível devido à indisponibilidade dos repositórios. Devido à impossibilidade de instalar o Nmap por indisponibilidade dos repositórios, a enumeração inicial dos serviços foi realizada através do comando ss -tuln, permitindo identificar os serviços ativos localmente.

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
A ausência de contas sem palavra-passe reduz o risco de acessos não autenticados ou exploração de contas abandonadas, contribuindo para uma melhor postura de segurança do sistema.

<img width="915" height="250" alt="image" src="https://github.com/user-attachments/assets/e7f49bb6-7430-4b10-8fee-83b7493efb75" />

---

## 4. Chaves SSH

### Comando

```bash
cat ~/.ssh/authorized_keys
```

### Resultado

Foram identificadas chaves públicas SSH autorizadas no ficheiro authorized_keys. Embora a utilização de chaves seja uma prática recomendada, cada chave deve estar associada a um utilizador autorizado. Chaves antigas, desconhecidas ou pertencentes a antigos administradores podem representar risco de acesso persistente ao sistema.

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

A regra de permissão da porta 22/TCP garantiu a continuidade do acesso administrativo remoto através de SSH, evitando o bloqueio do próprio administrador durante a aplicação das medidas de contenção. Esta precaução é importante durante alterações de segurança, pois uma configuração incorreta da firewall poderia impedir o acesso ao servidor.

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

O comando não apresentou uma configuração explícita de PasswordAuthentication no ficheiro principal. Foi necessário validar a configuração efetiva do serviço SSH para confirmar se a autenticação por palavra-passe estava realmente desativada.

**Verificação da autenticação por chave pública**
Comando executado
'grep -n "PubkeyAuthentication" /etc/ssh/sshd_config'

OBS.: A auditoria automática com Lynis não foi executada devido à indisponibilidade da ferramenta e limitações de rede do laboratório.

<img width="913" height="423" alt="image" src="https://github.com/user-attachments/assets/aa366c26-f13b-451c-be77-3bed14028b24" />

# Conclusão  
A auditoria realizada permitiu identificar os serviços ativos, avaliar a exposição do servidor e aplicar medidas de endurecimento da configuração. A ativação da firewall UFW, a restrição das portas disponíveis e a análise da configuração SSH contribuíram para reduzir a superfície de ataque. Algumas ferramentas previstas no laboratório, como Nmap e Lynis, não puderam ser utilizadas devido às limitações do ambiente TryHackMe, tendo sido aplicados métodos alternativos de análise.


