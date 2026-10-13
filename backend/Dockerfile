# Этап 1: Сборка приложения
FROM golang:1.22-alpine AS builder
WORKDIR /build

# Сначала копируем только файлы зависимостей для эффективного кэширования слоев
COPY go.mod ./
RUN go mod download

# Копируем остальной исходный код и собираем статический бинарник
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /out/app/main

# Этап 2: Исполнение в минималистичном окружении
FROM gcr.io/distroless/static-debian12:nonroot
WORKDIR /app

# Копируем скомпилированный бинарник из первого этапа
COPY --from=builder /out/app/main /app/app

EXPOSE 8080
ENTRYPOINT ["/app/app"]