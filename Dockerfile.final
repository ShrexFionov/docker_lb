# Stage 1: Build
FROM golang:1.21-alpine AS builder
WORKDIR /app
RUN apk add --no-cache git
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/server .

# Stage 2: Final
FROM alpine:3.18
RUN apk --no-cache add ca-certificates
RUN adduser -D appuser
WORKDIR /app
COPY --from=builder /app/server .
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
RUN chown -R appuser:appuser /app
USER appuser
EXPOSE 8080
CMD ["./server"]
