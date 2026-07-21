#Laboratorio-sessao 3
##Hardening de Redes Linux e Configuração de Firewalls
´´´
### Passo 1

<img width="851" height="407" alt="image" src="https://github.com/user-attachments/assets/29dbfe6d-d7a0-448c-b8d3-8e050cfd272e" />

Usei o comando ´sudo ufw status´ para fazer a verficaçao do estado do firewall

### Passo 2

<img width="706" height="312" alt="entrada e saida" src="https://github.com/user-attachments/assets/27d4be98-c9e0-481c-a56a-e3aa8a7f7673" />

Nessa captura de ecra executei os comandos `sudo ufw default deny incoming` e `sudo ufw default allow outgoing`.
A primeira configuração bloqueia todas as ligações de entrada que não estejam explicitamente autorizadas, aumentando a segurança do sistema. A segunda permite todas as ligações de saída, garantindo que o computador pode comunicar normalmente com outros sistemas e aceder a serviços externos. Esta configuração segue uma política de segurança defensiva, permitindo apenas o tráfego de entrada necessário e reduzindo a exposição a acessos não autorizados.

### Passo 3

<img width="662" height="341" alt="passo 3" src="https://github.com/user-attachments/assets/71ad5289-871e-4ebf-8ee8-65695bf95b39" />

Usei o comando `sudo ufw allow 22/tcp`para permitir ligaçoes SSH pela porta 22.

### Passo 4

<img width="865" height="393" alt="passo 4" src="https://github.com/user-attachments/assets/6406c72d-a4e1-4408-9fa0-9a404af8ee4d" />

Comando sudo iptables -A INPUT -s 203.0.113.50 -j DROP que bloqueia todo o tráfego proveniente do endereço ip especifcado.

### Passo 5

<img width="948" height="681" alt="passo5" src="https://github.com/user-attachments/assets/64ab5946-1837-4ff7-b86b-b1b66a1e4978" />

### outputs dos comandos ufw status verbose e iptables -L -v

<img width="961" height="421" alt="status" src="https://github.com/user-attachments/assets/c51b2a35-82f3-4c40-a0d3-77802def8f95" />

Usei o comando sudo ufw status verbose depois de ativar o UFW que nao tinha feito no inicio.

O resultado mostrou que o firewall se encontra ativo:
Política padrão: bloquear todas as ligações de entrada (deny incoming);
Permitir ligações de saída (allow outgoing);
Permitir acesso SSH através da porta 22/TCP.

A regra da porta 22/TCP foi mantida para garantir a administração remota segura do sistema, enquanto outros acessos de entrada permanecem bloqueados por padrão.


Ao executar o comando sudo iptables -L -v obtive os seguintes resultados:
A cadeia INPUT apresenta a política padrão DROP, indicando que ligações de entrada são bloqueadas por defeito.
Existe uma regra específica que bloqueia todo o tráfego proveniente do endereço IP 203.0.113.50 através da ação DROP.
Foi criada uma exceção para permitir ligações SSH através da porta 22/TCP.
As restantes cadeias são geridas pelo UFW e refletem as políticas configuradas anteriormente.

Esta configuração segue uma abordagem de hardening, onde o tráfego é negado por padrão e apenas os serviços necessários são autorizados.

<img width="967" height="450" alt="image" src="https://github.com/user-attachments/assets/ce50e61e-bc08-4e7c-af4a-b373631b9321" />

<img width="938" height="567" alt="image" src="https://github.com/user-attachments/assets/05e9cb5d-6d4d-4dc8-b7dd-8e7c8aa076f6" />
