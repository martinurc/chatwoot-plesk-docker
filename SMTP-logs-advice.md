## Email config en docker

1. Dónde buscar el Log de Errores de Email

Como Chatwoot procesa los envíos de correo en segundo plano, la clave no está en el log general de la web (rails), 
sino en el contenedor del Worker (sidekiq), o directamente en el archivo de registro de producción de la aplicación.

Tienes dos formas de inspeccionarlo desde la terminal de tu servidor:

### Método A: Ver el log en vivo de Sidekiq (El más rápido)

    docker compose -f httpdocs/docker-compose.yaml -p httpdocs logs sidekiq | grep -i "Mailer"

### Método B: Inspeccionar el archivo production.log por dentro

Ruby on Rails escribe absolutamente todo lo que pasa en la app dentro de un archivo de texto.
Puedes hacerle un tail directo entrando al contenedor:

     docker compose -f httpdocs/docker-compose.yaml -p httpdocs exec rails tail -n 100 log/production.log

(Si ves un error que dice Net::SMTPAuthenticationError, es que la contraseña o el usuario están mal.
Si dice Net::OpenTimeout, es que Plesk está bloqueando el puerto 587 de salida)
