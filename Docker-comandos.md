## Docker

Luego de cambiar cualquier configuración en las **.env** variables ejecutar el siguiente comando por SSH para levantar de nuevo containers:

    docker compose -f httpdocs/docker-compose.yaml -p httpdocs up -d

OUTPUT:

```
ll be ignored, please remove it to avoid potential confusion 
[+] up 5/5
 ✔ Container httpdocs-postgres-1 Running                                                                                  0.0s
 ✔ Container httpdocs-base-1     Started                                                                                  2.0s
 ✔ Container httpdocs-redis-1    Started                                                                                  2.1s
 ✔ Container httpdocs-sidekiq-1  Started                                                                                  2.0s
 ✔ Container httpdocs-rails-1    Started
```

Reiniciar todo desde cero (Limpieza profunda y violenta)

    docker compose -f httpdocs/docker-compose.yaml -p httpdocs down

    docker compose -f httpdocs/docker-compose.yaml -p httpdocs up -d --force-recreate


### El tigre Rack::Attack 

Salta cuando hay muchas peticiones de una misma IP. Agregar en el .env

    RACK_ATTACK_WHITELISTED_IPS=IP1,IP2, etc

[SMTP-logs-advice](SMTP-logs-advice.md)
