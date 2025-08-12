# Projeto-linux
Este projeto implementa um servidor web Nginx em um ambiente Linux com monitoramento automatizado que gera notificações no Discord.

Objetivo
- Configurar um servidor ANginx em linux
- Criar uma pagina html simples
- Desenvolver um script de monitoramento
- Configurar o Nginx para reiniciar automaticamente em caso de falha

Ambiente utilizado
SO: Ubuntu Server 22.04 na VirtualBox

Passo a passo de implementacao

1- Preparar ambiente linux

Instalar e iniciar o servidor linux

2- Instalar e configurar o Nginx

sudo apt install nginx -y

sudo systemctl enable nginx

sudo systemctl start nginx

3-Criar página HTML:

echo "< html > Parte de personalizacao do site </ html >" | sudo tee /var/www/html/index.html

4- Criar Script de monitoramento

sudo nano /usr/local/bin/monitoramento.sh

<img width="812" height="613" alt="Script" src="https://github.com/user-attachments/assets/3848de29-87fa-4b0a-8f71-fd9c1ad82786" />


5- Dar permissoes e criar o log

sudo chmod +x /usr/local/bin/monitoramento.sh

sudo touch /var/log/monitoramento.log

sudo chmod 666 /var/log/monitoramento.log

6-Automatizar execucao com Cron

sudo crontab -e

* * * * * /usr/local/bin/monitoramento.sh
   
7- Configurar o Nginx para reiniciar automaticamente

sudo systemctl edit nginx

[Service]
Restart=always
RestartSec=5

8- Salvar e recarregar o nginx

sudo systemctl daemon-reexec

sudo systemctl restart nginx

Logs


<img width="684" height="149" alt="disc" src="https://github.com/user-attachments/assets/61840cf3-157c-49f2-806b-4c8423c57467" />

<img width="815" height="611" alt="logs" src="https://github.com/user-attachments/assets/1ecf737f-51d4-4f56-8c2a-6f8a74677591" />

    
