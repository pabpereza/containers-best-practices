FROM golang:1.23 AS builder

# Set the current working directory inside the container
WORKDIR /app

# Copy go mod and sum files
COPY go.mod go.sum ./

# Download all dependencies
RUN go mod download

# Copy the source code into the container
COPY *.go .

# Compile the binary
RUN go build -o /app_bin

# Create a non-root user on a temporary file
RUN echo "appuser:x:65534:65534:appuser:/:" > /etc_passwd

FROM scratch

# Copy the passwd file to create a non-root user
COPY --from=builder /etc_passwd /etc/passwd

# Copy compiled binary from builder
COPY --chown=appuser:appuser --from=builder /app_bin /app_bin

# Set the user to use when running the image
USER appuser

# Set the entrypoint to the binary
CMD ["/app_bin"]
