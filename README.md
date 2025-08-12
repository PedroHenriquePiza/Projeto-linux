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

2- Instalar e configurar o Nginx
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx

3-Criar página HTML:
echo "<html><h1>Projeto Linux</h1><p>Servidor ativo</p></html>" | sudo tee /var/www/html/index.html

4- Criar Script de monitoramento
sudo nano /usr/local/bin/monitoramento.sh

#!/bin/bash

URL="Ip do site"
WEBHOOK_URL="Link do Webhook"
LOG_FILE="/var/log/monitoramento.log"

enviar_alerta() {
    local mensagem="$1"
    curl -H "Content-Type: application/json" \
         -X POST \
         -d "{\"content\": \"$mensagem\"}" \
         "$WEBHOOK_URL"
}

HTTP_CODE=$(curl -s -o /dev/null -w "%{http_code}" "$URL")

if [ "$HTTP_CODE" -eq 200 ]; then
    echo "$(date '+%Y-%m-%d %H:%M:%S') - Site online." >> "$LOG_FILE"
else
    echo "$(date '+%Y-%m-%d %H:%M:%S') - Site fora do ar Codigo: $HTTP_CODE" >> "$LOG_FILE"
    enviar_alerta "O site $URL esta fora do ar Codigo: $HTTP_CODE"
fi

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

    
