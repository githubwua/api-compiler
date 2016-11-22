[![Build Status](https://travis-ci.org/googleapis/api-compiler.svg?branch=master)](https://travis-ci.org/googleapis/api-compiler)

# Google API Compiler

## Overview

Google API Compiler (Api Compiler) is an open source tool for processing API
specifications. It currently supports OpenAPI specification, Protocol Buffers
(proto), and GRPC API Configuration, and can be extended to support other
formats.

Google API Compiler parses the input files into object models, processes and
validates the models, and generate various outputs, such as:

- Validation warnings and errors.
- Normalized/validated Google API Service Configuration.
- API discovery document.
- API reference documentation.
- API client libraries.

## Google API Service Configuration

`Google API Service Configuration` is generated by `Google API Compiler` and is
an intermediate format, not meant to be hand written. It defines the surface and
behavior of an API service, including interface, types, methods, authentication,
discovery, documentation, logging, monitoring and more. It is formally defined
by the proto message
[`google.api.Service`](https://github.com/googleapis/googleapis/tree/master/google/api/service.proto)
and works with both REST and RPC APIs. Developers typically create proto files,
defining the surface of the API service and create the `GRPC API configuration`
using YAML files. They then use the Google API Compiler to generate the Google
API Service Configuration as
[`google.api.Service`](https://github.com/googleapis/googleapis/tree/master/google/api/service.proto)
proto message.

NOTE: Google API Service Configuration is a rich and mature specification used
for Google production services, such as Cloud Logging, Cloud Vision,
Cloud Bigtable, IAM, and more.

## Used by other tools.
Google API compiler is used by other tools like [googleapis/toolkit](https://github.com/googleapis/toolkit)
to read the users API definition and autogenerate client libraries.

## Cloning Google API Compiler

Clone the _Google API Compiler_ repo
```
$ git clone https://github.com/googleapis/api-compiler
```
Update submodules
```
$ git submodule update --recursive --init
```

## Creating Google API Service Configuration from proto files and GRPC API Configurations

### Creating a proto descriptor file

Google API Compiler does not consume the proto files directly. Developers need
to use `protoc` to generate the proto descriptor, then feed it to the Google
API Compiler.

```
# Creates a proto descriptor from proto files using protoc.
$ protoc <file1.proto> <file2.proto> --include_source_info --include_imports --descriptor_set_out=out.descriptors
```

### Creating GRPC API Configurations

```
# -------------File: myapi.yaml-----------------

# The schema of this file.
type: google.api.Service

# The version of the GRPC API Configurations.
config_version: 3

# The service name. It should be the primary DNS name for the service.
name: library-example.googleapis.com

# The official title of this service.
title: Google Example Library API

# The list of API interfaces exposed by the service.
apis:
- name: google.example.library.v1.LibraryService

# Other aspects of the service, such as authentication.
# ...
```

### Executing the Google API Compiler

```
DESCRIPTOR_FILE=<PATH TO out.descriptor>
CONFIG_FILE=<path to yaml file>
JSON_FILE_NAME=<json output file name>
BINARY_FILE_NAME=<binary output file name>

./run.sh \
--configs $CONFIG_FILE \
--descriptor $DESCRIPTOR_FILE \
--json_out $JSON_FILE_NAME \
--bin_out $BINARY_FILE_NAME
```

This command will output the Google API Service Configuration in
different formats:
- Binary file: $BINARY_FILE_NAME
- Json file: $JSON_FILE_NAME

Either format can be used to configure a Google Cloud Endpoints API.

## Creating Google API Service Configuration from an OpenAPI Specification

Validate an OpenAPI Specification and create the corresponding service
configuration.

```
OPENAPI_FILE=<OpenAPI Spec filename>
JSON_FILE_NAME=<json output file name>
BINARY_FILE_NAME=<binary output file name>

./run.sh \
--openapi $OPENAPI_FILE \
--json_out $JSON_FILE_NAME \
--bin_out $BINARY_FILE_NAME
```

This will create the service configuration in different formats:
- Binary file: $BINARY_FILE_NAME
- JSON file: $JSON_FILE_NAME

Either format can be used to configure a Google Cloud Endpoints API.


## Compile Google API Compiler

Build source code
```
$ ./gradlew buildApplication
```
For running tests, you need to have `protoc` in your path. If you don't
already have protoc version 3, you can download
it from https://github.com/google/protobuf/releases and set a symbolic link to
the protoc.
```
# Example
$ sudo ln -s  <Path to the downloaded protoc> /usr/local/bin/protoc
```


