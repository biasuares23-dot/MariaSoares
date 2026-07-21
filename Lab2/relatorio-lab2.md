# Laboratório — Sessão 2
## Auditoria de Sistemas Linux e Análise Avançada de Logs
## Resultado da análise dos logs

Comando 'cd /var/log/'

<img width="932" height="173" alt="image" src="https://github.com/user-attachments/assets/de706c7c-ae77-495b-b951-9c69fc3129a2" />

comando 'grep "Failed password" auth.log'

<img width="828" height="136" alt="image" src="https://github.com/user-attachments/assets/5b814689-8f60-4fbd-8e1d-55559d30e432" />

Comando grep "Failed password" auth.log | awk '{print $11}' | sort | uniq-c | sort -nr

<img width="950" height="173" alt="image" src="https://github.com/user-attachments/assets/b098bc28-561f-4df9-a825-4ea5ceabf18d" />

## Conclusao 

Foi realizada a análise do ficheiro `/var/log/auth.log` para identificar possíveis tentativas de acesso não autorizado através de SSH.
Não foram encontrados IPs suspeitos associados a tentativas falhadas de autenticação.
