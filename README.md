# React Native gRPC

## Install protoc

* protoc : [link](https://github.com/protocolbuffers/protobuf/releases)

### OSX

```sh
brew install protobuf
```

```sh
PROTOC_ZIP=protoc-3.7.1-osx-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v3.10.0/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```

### Linux

```sh
PROTOC_ZIP=protoc-3.7.1-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v3.10.0/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```

## Install grpc-web

```sh
GROC_WEB_PLUGIN=protoc-gen-grpc-web-1.0.6-darwin-x86_64
curl -OL https://github.com/grpc/grpc-web/releases/download/1.0.6/$GROC_WEB_PLUGIN
sudo mv $GROC_WEB_PLUGIN /usr/local/bin/protoc-gen-grpc-web
chmod +x /usr/local/bin/protoc-gen-grpc-web
```

```sh
npm install ts-protoc-gen
```

## Code Generate

```sh
protoc \
  --proto_path=proto \
  --plugin=protoc-gen-ts=./node_modules/.bin/protoc-gen-ts \
  --js_out=import_style=commonjs:client/helloworld \
  --ts_out=service=grpc-web:client/helloworld \
  helloworld.proto
protoc \
  --proto_path=proto \
  --js_out=import_style=commonjs:web/src/helloworld \
  --grpc-web_out=import_style=commonjs,mode=grpcwebtext:web/src/helloworld \
  helloworld.proto
protoc \
  --proto_path=proto \
  --go_out=plugins=grpc:server/helloworld \
  helloworld.proto
```

## Run Application

### Client

```sh
rm -rf ios/build
react-native run-ios --simulator="iPhone 8"
```

### Server

```sh
GRPC_GO_LOG_VERBOSITY_LEVEL=99 GRPC_GO_LOG_SEVERITY_LEVEL=info go run ./server/main.go
```