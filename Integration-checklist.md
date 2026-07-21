📋 The Complete Integration Checklist
Step 1: Chatwoot Setup Verification

    [ ] Generate an Account-Level API Token

        Do not use your personal user profile token.

        Go to Settings → Integrations → Application API or create an Agent Bot via API/Rails console to ensure it has bot permissions.

    [ ] Verify URL & IDs from Chatwoot

        Base URL: Must be strictly [https://lassociacio.siguemedia.com](https://lassociacio.siguemedia.com) (no trailing / at the end).

        Account ID: Found directly in your browser URL ([https://lassociacio.siguemedia.com/app/accounts/1/](https://lassociacio.siguemedia.com/app/accounts/1/)... → 1).

        Inbox ID: Go to Settings → Inboxes → click your website/chat inbox → check the URL (.../settings/inboxes/4 → 4).

Step 2: Agent Bot Assignment in Chatwoot

(This is the most common reason messages don't reach Botpress!)

    [ ] Attach the Agent Bot to your Inbox

        In Chatwoot, creating an Agent Bot is only step one. It must be assigned to handle incoming messages for that specific Inbox.

        Go to Settings → Inboxes → Select your Inbox → Settings tab.

        Under Bot Agent, select your Botpress bot from the dropdown list.

        Click Update.

Step 3: Webhook & Secret Configuration

    [ ] Botpress Webhook in Chatwoot

        Copy the exact Webhook URL generated inside your Botpress Integration Hub ([https://webhook.botpress.cloud/](https://webhook.botpress.cloud/)...).

        Paste it into the Chatwoot Agent Bot URL de Webhook field.

    [ ] Clear the Webhook Secret

        Ensure the Webhook Secret field in Chatwoot is completely empty (reset/delete any characters).

Step 4: Botpress Integration Hub Settings

    [ ] Fill in all required fields in Botpress Integration Hub:

        Bot Token: (The access token from your Chatwoot bot)

        Base Url: [https://lassociacio.siguemedia.com](https://lassociacio.siguemedia.com)

        Account Id: 1 (or your actual account ID)

        Inbox Id: (your inbox ID number)

    [ ] Save & Enable Integration

        Click Save in Botpress.

        Ensure the toggle/switch for the Chatwoot integration is set to Enabled.

        Re-publish your Botpress bot.

Step 5: Server & Network Checks (Self-Hosted Instance)

Because lassociacio.siguemedia.com is hosted on a custom domain/server:

    [ ] Check Firewall / Cloudflare Blocking

        Botpress sends outgoing requests from Cloudflare/AWS IP addresses. Ensure your server firewall (Plesk/UFW/Fail2ban) isn't blocking incoming POST requests from webhook.botpress.cloud.

    [ ] Verify Public Webhook Reachability

        Open a browser or terminal and check if your Chatwoot webhooks endpoint is accessible over standard HTTPS (SSL certificate must be valid).

Step 6: Testing & Log Verification

    [ ] Send a Test Message

        Open your Chatwoot live widget or inbox and send: "Hola".

    [ ] Check Botpress Logs

        Go to Botpress Studio → Open Logs (bottom bar).

        Look for an incoming event: Message of type text received from... on chatwoot.

    [ ] Check Chatwoot Production Logs

        If no event appears in Botpress, check your Chatwoot logs on your server:
        Bash

        docker logs -f chatwoot_web_1 # or tail -f log/production.log

        Look for Webhooks::OutgoingWorker errors or 401 Unauthorized / 404 Not Found responses.


DOCKER CONTENEDORES
No necesitas tener httpdocs-base-1 encendido continuamente.

Ese contenedor es lo que en Docker / Rails se denomina un contenedor de inicialización o utilitario (one-off container).
¿Para qué sirve realmente httpdocs-base-1?

Su única función es ejecutar tareas puntuales de configuración o mantenimiento cuando despliegas o actualizas la aplicación, como por ejemplo:

    Ejecutar las migraciones de la base de datos (rails db:chatwoot_prepare o rails db:migrate).

    Precompilar recursos estáticos (assets) si fuera necesario.

    Crear el usuario administrador inicial la primera vez que instalas Chatwoot.

Una vez que esas tareas se completan (o si la instalación ya está en producción y funcionando), el contenedor se detiene y es 100% normal que se quede en estado Exited (0).
La arquitectura mínima activa de Chatwoot

Para que tu entorno de Chatwoot esté 100% operativo en producción, solo necesitas tener activos en ejecución continua (Up) los siguientes contenedores:

    httpdocs-rails-1: Atiende la interfaz web, el panel de administración, la API y los sockets de tiempo real.

    httpdocs-sidekiq-1: Procesa la cola de tareas en segundo plano (incluyendo el envío de webhooks a Botpress y correos).

    httpdocs_redis: Almacena las colas de trabajo y datos en caché.

    PostgreSQL (ya sea un contenedor httpdocs_postgres o una base de datos externa): Guarda las conversaciones, usuarios y configuración.

Si rails y sidekiq están encendidos y conectados a Redis y Postgres, tu Chatwoot funcionará a la perfección sin consumir RAM ni CPU innecesaria intentando mantener httpdocs-base-1 encendido. 
