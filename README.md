# protoc

A repo to manage docker images for the protobuf compiler (protoc), in order to standardize on protoc and grpc versions across developer machines.

Initially this was built with the focus on compiling for grpc and protbufs in golang, but can be easily extended to add support for other languages as needed.


Usage:
```
docker run --rm -v $(pwd):$(pwd) -w $(pwd) udryan10/protoc --go_out=plugins=grpc:. *.proto
``` 

A few other popular go protobuf plugins compiled into the image to provide some variety:
```
github.com/golang/protobuf/protoc-gen-go 
github.com/gogo/protobuf/protoc-gen-gofast 
github.com/gogo/protobuf/protoc-gen-gogo 
github.com/gogo/protobuf/protoc-gen-gogofast 
github.com/gogo/protobuf/protoc-gen-gogofaster 
github.com/gogo/protobuf/protoc-gen-gogoslick 
github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger 
github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway 
github.com/johanbrandhorst/protobuf/protoc-gen-gopherjs 
github.com/ckaznocha/protoc-gen-lint
```