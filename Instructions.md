1. Create container files
2. Build podman images
 - postgres
    build
        podman build -t postgres-guestbook .\postgresql\
    run
        podman run -d --name postgres -p 5432:5432 postgres-guestbook
 - redis
    build
        podman build -t redis-guestbook .\redis\
    run
        podman run -d --name redis -p 6379:6379 redis-guestbook
 - backend
    build
        podman build -t backend-guestbook .\backend\
    run
        podman run -d --name backend-guestbook -p 8080:8080 backend-guestbook
- frontend
    build
        podman build -t frontend-guestbook .\frontend\
    run
        podman run -d --name frontend-guestbook -p 127.0.0.1:8081:8080 frontend-guestbook

OBS! has to hardcode the url to 127.0.0.1 or the connection wont resolve for some reason.
3. Surf to 127.0.0.1:8081 and check it works.
4. im using ghcr.io for the workflow because its OCI compliant and  skips creating secrets and uses the github-token that is already in store by github. It also lets us store docker images alongside the github repos as a alternative to docker hub.

## Fast commands
check listening ports
```bash  
    podman exec frontend-guestbook ss -tlnp 
```
rebuild frontend
```bash  
podman stop frontend-guestbook
podman rm frontend-guestbook
podman build -t frontend-guestbook .\frontend\
podman run -d --name frontend-guestbook -p 127.0.0.1:8081:8080 frontend-guestbook
```
Build whole project
 ```bash   
podman build -t postgres-guestbook .\postgresql\
podman build -t redis-guestbook .\redis\
podman build -t backend-guestbook .\backend\
podman build -t frontend-guestbook .\frontend\
```
Run all pods
 ```bash  
podman run -d --name postgres -p 5432:5432 postgres-guestbook
podman run -d --name redis -p 6379:6379 redis-guestbook
podman run -d --name backend-guestbook -p 8080:8080 `
  -e DB_HOST=host.containers.internal `
  -e DB_PORT=5432 `
  -e DB_USER=guestbook `
  -e DB_PASSWORD=password `
  -e DB_NAME=guestbook `
  -e REDIS_HOST=host.containers.internal `
  -e REDIS_PORT=6379 `
  -e REDIS_PASSWORD=redis `
  backend-guestbook
 podman run -d --name frontend-guestbook -p 127.0.0.1:8081:8080 frontend-guestbook
 ```
