## Compiler

The `protoc` compiler is written in C++ and needs to be built from source:

> Note: [here](https://github.com/google/protobuf/tree/master/src) details shell dependencies required

- `git clone git@github.com:google/protobuf.git`
- `./autogen.sh`
- `./configure`
- `make # this can take quite a while ~10 mins for me`
- `make check # this can take quite a while ~10 mins for me`
- `sudo make install`

## Go Example

- `go get -a github.com/golang/protobuf/protoc-gen-go` (Go plugin for protoc compiler)
- `git clone https://github.com/grpc/grpc.git`
- `cd grpc/examples`
- `mkdir testing-helloworld`
- `protoc -I ./protos ./protos/helloworld.proto --go_out=plugins=grpc:testing-helloworld`

This generates:

```
testing-helloworld/
└── helloworld.pb.go

0 directories, 1 file
```

- `go get -u google.golang.org/grpc/examples/helloworld/greeter_client`
- `go get -u google.golang.org/grpc/examples/helloworld/greeter_server`
- `cd $GOPATH/src/google.golang.org/grpc/examples/helloworld`
- `cd greeter_server & go run main.go`
- `cd greeter_client & go run main.go`

> Note: the client/server you downloaded reference their own pb.go  
> /src/google.golang.org/grpc/examples/helloworld/helloworld/helloworld.pb.go  
> If you want, you can create your own client/server code to be sure...  

- `cd grpc/examples` (wherever you originally git cloned the grpc repo)
- `cp greeter_server.go ./testing-server.go`

Replace the line:

```go
pb "google.golang.org/grpc/examples/helloworld/helloworld"
```

With:

```go
pb "github.com/wherever/you/cloned/grpc/examples/testing-helloworld"
```

Now you should be able to run:

```go
go run testing-server.go
```

And verify using either the existing Go client you downloaded (see above) or an existing Ruby client (see below).

## Ruby Example

- `git clone https://github.com/grpc/grpc.git`
- `cd grpc/examples/ruby`
- `bundle install` (includes install of Ruby plugin for protoc)
- `bundle exec ./greeter_server.rb`
- `bundle exec ./greeter_client.rb`

## Mixing Server and Client

You can mix and match servers and clients.

e.g. have a Go server running with a Ruby client connecting, and vice versa

## Custom Services

You'll find that if you want to write your own proto files and compile them, having the `protoc` compiler itself is not enough. You also need to build grpc (https://github.com/grpc/grpc/blob/master/INSTALL.md) from source too :-/

- `git clone https://github.com/grpc/grpc.git`
- `cd grpc`
- `git submodule update --init`
- `make`
- `make install`

Ref : https://gist.github.com/Integralist/e26339e4d4469471a256317a9bacb8bb
