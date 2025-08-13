# Projeto-linux
Este projeto implementa um servidor web Nginx em um ambiente Linux com monitoramento automatizado que gera notificações no Discord.

Objetivo
- Configurar um servidor Nginx em linux
- Criar uma pagina html simples
- Desenvolver um script de monitoramento
- Configurar o Nginx para reiniciar automaticamente em caso de falha

Ambiente utilizado
SO: Ubuntu Server 22.04 na VirtualBox

Passo a passo de implementacao

1- Preparar ambiente linux

Instalar e iniciar o servidor linux

Apos iniciar verifique se esta tudo atualizado

sudo apt update && sudo apt upgrade -y

2- Instalar e configurar o Nginx

sudo apt install nginx -y

sudo systemctl enable nginx

sudo systemctl start nginx

3-Criar página HTML:

echo "< html > Parte de personalizacao do site </ html >" | sudo tee /var/www/html/index.html

4- Criar Script de monitoramento em bash


acessar o local da criacao do script - sudo nano /usr/local/bin/monitoramento.sh

<img width="812" height="613" alt="Script" src="https://github.com/user-attachments/assets/3848de29-87fa-4b0a-8f71-fd9c1ad82786" />

esse script funciona da seguinte maneira no topo do codigo é a definicao das variaveis como site, webhook e caminho das logs , apos isso entra a funcao enviar alerta que recebe um texto mensagem="$1" e usa o curl para fazer o post no webhook. em seguida entra a verificacao do site em que o curl acessa o site e mostra o resultado com o http_code, em sequencia entra a mensagem em caso do site estar tudo ok http_code = 200 exibe a mensagem que o site esta funcionando normalmente caso contrario exibe que o site esta fora do ar.

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

Verificacao de Logs

cat /var/log/monitoramento.log

<img width="684" height="149" alt="disc" src="https://github.com/user-attachments/assets/61840cf3-157c-49f2-806b-4c8423c57467" />

<img width="815" height="611" alt="logs" src="https://github.com/user-attachments/assets/1ecf737f-51d4-4f56-8c2a-6f8a74677591" />

    
