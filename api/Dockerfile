FROM golang

WORKDIR /go/src/app
COPY . .

RUN go mod init
RUN go get -d -v -t ./...
RUN go install -v ./...

CMD ["app"]

EXPOSE 9090