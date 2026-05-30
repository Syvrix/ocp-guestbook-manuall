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

## Fast commands
check listening ports
    podman exec frontend-guestbook ss -tlnp
rebuild frontend
    podman stop frontend-guestbook
    podman rm frontend-guestbook
    podman build -t frontend-guestbook .\frontend\
    podman run -d --name frontend-guestbook -p 127.0.0.1:8081:8080 frontend-guestbook


