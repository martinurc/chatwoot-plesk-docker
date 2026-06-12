¡Qué bueno que event.conversationId haya sido la llave! Botpress a veces tiene sus manías con el tipado de los objetos según cómo esté compilada la integración, pero ya lo tienes asegurado.

Respecto a tu pregunta sobre las Campaigns (Campañas) en Chatwoot Self-Hosted para iniciar conversaciones de forma proactiva: ¡Sí, se puede hacer perfectamente y es una de las mejores funciones que tiene!

Las campañas en Chatwoot están diseñadas justo para eso: en lugar de esperar a que el usuario diga "Hola", el widget se abre solo y le lanza un mensaje automático en base a ciertas reglas (ejemplo: si lleva 5 segundos en la web, si está en una URL concreta, etc.).
🚀 Cómo configurar una Campaña Proactiva en Chatwoot

Para crearla, sigue estos pasos desde tu panel de administración:

  En el menú lateral izquierdo de Chatwoot, ve a Campaigns (Campañas).

  Haz clic en Create Campaign (Crear campaña).
    Configura los siguientes campos clave:
        Title: Un nombre interno (ej: "Bienvenida automática - Web Econtrol").
        Inbox: Selecciona la bandeja de entrada del chat que está conectada a Botpress (Inbox 1).
        Sender: Elige qué agente "firmará" el mensaje saliente (puedes elegir a tu propio usuario o un perfil genérico).
        Message: El texto con el que el chat se abrirá solo (ej: "¡Hola! Veo que estás revisando nuestra web. ¿Te puedo ayudar en algo?").
        Campaign Type: Elige Proactive (Proactiva).
        Time delta: El tiempo en segundos que el sistema esperará desde que el usuario entra a la web antes de disparar el mensaje (ej: 5 o 10 segundos).
    Habilita el check de Enabled y dale a Save.
