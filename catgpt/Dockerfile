FROM golang:1.21 as build

WORKDIR /usr/src/app

# pre-copy/cache go.mod for pre-downloading dependencies and only redownloading them in subsequent builds if they change
COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN CGO_ENABLED=0 go build -o /usr/local/bin/app

# Now copy it into our base image.
FROM gcr.io/distroless/static-debian12:latest-amd64
COPY --from=build /usr/local/bin/app /
EXPOSE 8080 9090
CMD ["/app"]