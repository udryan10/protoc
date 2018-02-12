# compile image
FROM alpine:latest as protoc-compile

ENV PROTOBUF_VERSION=3.5.0 \
    GRPC_VERSION=1.9.1

RUN apk add --no-cache build-base curl automake autoconf libtool git zlib-dev

RUN mkdir /tmp/protobuf /tmp/grpc

RUN curl -L https://github.com/google/protobuf/archive/v${PROTOBUF_VERSION}.tar.gz | tar xvz --strip-components=1 -C /tmp/protobuf

RUN git clone --depth 1 --recursive -b v${GRPC_VERSION} https://github.com/grpc/grpc.git /tmp/grpc && \
    rm -rf /tmp/grpc/third_party/protobuf && \
    ln -s /tmp/protobuf /tmp/grpc/third_party/protobuf

WORKDIR /tmp/grpc

# protoc will be compiled through the grpc code as a third_party
RUN make install prefix=/build/usr


# final image
FROM golang:1.9.2-alpine3.6 as protoc

COPY --from=protoc-compile /build/ /
COPY --from=protoc-compile /tmp/protobuf/src/protoc /usr/local/bin
RUN mkdir -p /usr/local/include/google/protobuf/
COPY --from=protoc-compile /tmp/protobuf/src/google/protobuf/*.proto /usr/local/include/google/protobuf/
RUN apk add --no-cache build-base curl automake autoconf libtool git zlib-dev

RUN go get -u -v github.com/golang/dep/cmd/dep \
     github.com/golang/protobuf/protoc-gen-go \
     github.com/gogo/protobuf/protoc-gen-gofast \
     github.com/gogo/protobuf/protoc-gen-gogo \
     github.com/gogo/protobuf/protoc-gen-gogofast \
     github.com/gogo/protobuf/protoc-gen-gogofaster \
     github.com/gogo/protobuf/protoc-gen-gogoslick \
     github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger \
     github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway \
     github.com/johanbrandhorst/protobuf/protoc-gen-gopherjs \
     github.com/ckaznocha/protoc-gen-lint
 
ENTRYPOINT [ "protoc", "-I." ]
