FROM registry.access.redhat.com/ubi10/nginx-126:10.0

USER 0
RUN mkdir -p /opt/app-root/src
COPY nginx.conf /etc/nginx/nginx.conf
COPY index.html /opt/app-root/src/

EXPOSE 8080
USER 101
CMD ["nginx", "-g", "daemon off;"]
