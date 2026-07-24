# Laboratório sessao - 5
## Análise de Vulnerabilidades em Linux e Ferramentas de Auditoria

Execução de um exame de auditoria técnica automatizada para identificar desvios de
conformidade em relação aos standards de segurança recomendados (CIS
Benchmarks)

### Execuçao dos comandos

Executei o comando sudo apt update && sudo apt install lynis -y para atualizar a arvore de pacotes e instalar o lynis.

<img width="1308" height="860" alt="image" src="https://github.com/user-attachments/assets/7ef98fa5-d8bf-48c1-bc98-0a055d1f28e6" />

Executei o comando sudo lynis audit system para iniciar a auditoria completa do sistema operativo.

<img width="1285" height="818" alt="image" src="https://github.com/user-attachments/assets/11910ef7-9c92-48f0-a653-7c71c09fb757" />

### Resultados da Auditoria

A auditoria de segurança foi realizada com a ferramenta Lynis, que analisou a configuração do sistema Linux.

Resultados obtidos:
- **Hardening Score:** 60
- **Warnings:** 1
- **Suggestions:** 50

<img width="1262" height="852" alt="image" src="https://github.com/user-attachments/assets/02a58e4c-3123-4903-94f9-d7e0f8d80103" />

O Hardening Score de 60 indica que o sistema possui algumas medidas básicas de segurança implementadas, mas ainda existem diversas recomendações para melhorar a sua proteção e conformidade com as boas práticas de segurança.

### Sugestões críticas e medidas corretivas

<img width="1303" height="832" alt="image" src="https://github.com/user-attachments/assets/192779e2-f4e8-4cdf-83f6-46894fa8557d" />

1. Install fail2ban to automatically ban hosts that commit multiple authentication errors (DEB-0880)

**Área:** Authentication

**Descrição:**
O Lynis recomenda a instalação do Fail2ban para monitorizar tentativas de autenticação falhadas e bloquear automaticamente os endereços IP que realizem múltiplas tentativas de acesso. Esta medida reduz o risco de ataques de força bruta aos serviços de autenticação, como o SSH.

**Correção recomendada (Cisofy):**
Instalar e configurar o Fail2ban para proteger os serviços expostos. Após a instalação, devem ser definidas regras para monitorizar os ficheiros de log e bloquear temporariamente os endereços IP que excedam o número permitido de tentativas de autenticação.


2. Configure password hashing rounds in /etc/login.defs (AUTH-9230)

**Área:** Authentication

**Descrição:**
O Lynis recomenda aumentar o número de rounds utilizados na geração do hash das palavras-passe. Um número maior de rounds torna o processo de quebra de palavras-passe mais difícil e aumenta a segurança das credenciais armazenadas.

**Correção recomendada (Cisofy):**
Editar o ficheiro `/etc/login.defs` e configurar um valor adequado para os parâmetros de hashing das palavras-passe, seguindo as recomendações de segurança. Esta configuração reforça a proteção contra ataques de força bruta e dicionário.
