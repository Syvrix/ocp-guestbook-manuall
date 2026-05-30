FROM registry.access.redhat.com/ubi10/go-toolset:10.0 AS builder

WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN GOFLAGS="-mod=mod" GOOS=linux GOARCH=amd64 go build -o /guestbook-api .

FROM registry.access.redhat.com/ubi10-minimal:10.0
COPY --from=builder /guestbook-api /usr/local/bin/guestbook-api

EXPOSE 8080
USER 1001
ENTRYPOINT ["/usr/local/bin/guestbook-api"]
