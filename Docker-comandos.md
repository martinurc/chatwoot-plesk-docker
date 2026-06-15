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
