FROM golang:1.21.4-bullseye AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /main ./cmd

FROM golang:1.21.4-bullseye AS dev
WORKDIR /app
COPY --from=builder /app /app
RUN go install github.com/cosmtrek/air@latest
COPY .air.toml .
CMD ["air", "-c", ".air.toml"]

FROM gcr.io/distroless/base AS prod
WORKDIR /app
COPY --from=builder /app /app
CMD ["/app/main"]
