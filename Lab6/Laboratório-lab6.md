<img width="1032" height="802" alt="image" src="https://github.com/user-attachments/assets/21d659ab-8ed4-4438-bd50-a2a695ae1ae1" />
Foi executado o comando ss -tuln para identificar os serviços em escuta no servidor. Observou-se que as portas TCP 22 (SSH) e 80 (HTTP) estavam acessíveis, bem como serviços locais de DNS e outros serviços auxiliares, como CUPS (631) e mDNS (5353), que seriam posteriormente avaliados quanto à sua necessidade.


O comando nmap -sV localhost não pôde ser executado porque a ferramenta não se encontrava instalada na máquina disponibilizada e a instalação não foi possível devido à indisponibilidade dos repositórios. A identificação inicial dos serviços foi realizada com o comando ss -tuln.
<img width="1030" height="750" alt="image" src="https://github.com/user-attachments/assets/e907be7e-73fd-44a4-93d9-89ebabce223d" />
