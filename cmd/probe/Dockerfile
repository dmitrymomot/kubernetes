# Image with necessary dependencies
FROM golang:alpine AS container
RUN apk update && apk upgrade && \
	apk add --no-cache bash git openssh curl ca-certificates
COPY ./main.go /
RUN go get -d -v ./...
    
# Go application builder
FROM container AS builder
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix nocgo -o /probe ./main.go

# Application
FROM alpine:latest
RUN apk add --update ca-certificates && \
    rm -rf /var/cache/apk/* /tmp/*
WORKDIR /
COPY --from=builder /probe /
ENTRYPOINT [ "/probe" ]
