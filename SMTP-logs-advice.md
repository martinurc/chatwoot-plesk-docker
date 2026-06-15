## Email config en docker

1. Dónde buscar el Log de Errores de Email

Como Chatwoot procesa los envíos de correo en segundo plano, la clave no está en el log general de la web (rails), 
sino en el contenedor del Worker (sidekiq), o directamente en el archivo de registro de producción de la aplicación.

Tienes dos formas de inspeccionarlo desde la terminal de tu servidor:

### Método A: Ver el log en vivo de Sidekiq (El más rápido)

    docker compose -f httpdocs/docker-compose.yaml -p httpdocs logs sidekiq | grep -i "Mailer"

### Método B: Inspeccionar el archivo production.log por dentro

ATENCIÓN: En las imágenes Docker modernas de Chatwoot, los logs no se escriben en archivos de texto dentro del contenedor para no inflar el disco; se redirigen directamente a la salida estándar (stdout).
Ruby on Rails escribe absolutamente todo lo que pasa en la app dentro de un archivo de texto.
Puedes hacerle un tail directo entrando al contenedor:

     docker compose -f httpdocs/docker-compose.yaml -p httpdocs exec rails tail -n 100 log/production.log

(Si ves un error que dice Net::SMTPAuthenticationError, es que la contraseña o el usuario están mal.
Si dice Net::OpenTimeout, es que Plesk está bloqueando el puerto 587 de salida)

Por qué Hostalia acepta el email pero no llega?

Cuando un servidor SMTP corporativo acepta un correo de un VPS autohospedado pero luego el correo no aparece ni en la bandeja de SPAM, el motivo es el bloqueo estricto por SPF / DKIM / DMARC a nivel de cabeceras.

Vamos a revisar las tres variables que te faltan en el .env para asegurarte de que Hostalia no catalogue el envío como "Phishing" o suplantación de identidad:

### 1. ¿Añadiste la variable MAILER_SENDER_EMAIL?

Si no la pusiste tras el último mensaje, hazlo ya. Si Chatwoot no la tiene, envía el correo usando una dirección por defecto como no-reply@chatwoot.com. Al recibirlo Hostalia, ve que te autenticas como send@siguemedia.com pero pretendes ser chatwoot.com, y lo tira a la basura silenciosamente.

### 2. La variable MAILER_INBOUND_EMAIL_DOMAIN

Chatwoot necesita saber qué dominio gestiona los correos de la plataforma para armar los identificadores únicos de los mensajes (Message-ID). Si no está configurada, genera IDs de correo extraños que los filtros antispam modernos bloquean en el acto.
