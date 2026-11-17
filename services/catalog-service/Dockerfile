# Stage 1: Build
FROM golang:1.24-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build \
    -ldflags="-s -w" \
    -o catalog-service .

# Stage 2: Runtime
FROM alpine:3.22

WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/catalog-service .

RUN chown appuser:appgroup catalog-service

USER appuser

EXPOSE 8082

ENTRYPOINT ["./catalog-service"]
