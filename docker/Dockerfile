FROM golang:1.22-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download
COPY ./*.go ./
RUN CGO_ENABLED=0 GOOS=linux go build -o clamdproxy .

FROM alpine:latest

RUN apk --no-cache add ca-certificates
WORKDIR /app
COPY --from=builder /app/clamdproxy /app/

EXPOSE 3315 2112
ENTRYPOINT ["/app/clamdproxy"]

# clamav:3310 get from docker-compose url
CMD ["--listen", "0.0.0.0:3315", "--backend", "clamav:3310", "--log-level", "debug", "--metrics", "0.0.0.0:2112"]