FROM golang:1.19 as builder
WORKDIR /csi
ADD go.mod go.sum /csi/
RUN go mod download
ADD . /csi/
RUN ls -al
RUN GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o aio.bin github.com/hetznercloud/csi-driver/cmd/aio

FROM --platform=linux/amd64 alpine:3.15
RUN apk add --no-cache ca-certificates e2fsprogs xfsprogs blkid xfsprogs-extra e2fsprogs-extra btrfs-progs cryptsetup
ENV GOTRACEBACK=all
RUN mkdir -p /plugin
COPY --from=builder /csi/aio.bin /plugin

ENTRYPOINT [ "/plugin/aio.bin" ]
