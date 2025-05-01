# Build stage
FROM golang:1.24.2 AS builder

WORKDIR /app

RUN go install github.com/air-verse/air@latest

# Copy air config
COPY .air.toml ./

# Copy dependencies first for better caching
COPY go.mod go.sum ./
RUN go mod download

# Copy source code last to maximize caching
COPY . .

# Expose the app port
EXPOSE 8080

# Use Air to run in development
CMD ["air", "-c", ".air.toml"]

# Final stage
# FROM alpine:3.21

# WORKDIR /app

# # Add ca certificates and timezone data
# RUN apk --no-cache add ca-certificates tzdata

# # Copy the binary from builder
# COPY --from=builder /app/app .

# # Make sure the binary is executable
# RUN chmod +x /app/app

# # Expose the application port
# EXPOSE 8080

# # Run the application with explicit path
# CMD ["/app/app"]