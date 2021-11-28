# Microservices

As mentioned before, *Jump App* is a multi microservices application based on different programing languages. The general goal is to allow users to configure a set of jumps between *Jump App* components and generate a continuous traffic flow defining the number of retries and their span of time.

As many of applications, *Jump App* is base on a frontend microservice and a set of backend microservices in order to implement the functionality in a modular architecture. The following sections describe this modular architecture from an overall perspective.

## Fronted

The frontend is an application based on React which implements an interface to generate flow between the backend microservices.

This frontend includes a configuration section which allows users to configure a set of microservices jumps, their sequence, the number of these jumps retries and the span of time between them.

It is also possible to enable gRPC traffic checking the respective checkbox. Using this option, the HTTP traffic is replaced by gRPC request to the backend.

NOTE: It is required to deploy *Jump App* gRPC microservices in order to be able to handle this kind of traffic.

## Backends

The main objective of the backend services is to receive a set of request with a *Jump* object which controls the sequence of jumps between the backend microservices.

With HTTP traffic, the backend components implement an API path */jump* which receives a request and operate as follows:

- POST Request - Receive a JSON Object, called *Jump*, and send a request to the next *jump* based on the information included in jumps field in the object. When *jumps* field contains only one jump, a GET request is perform, if not a POST request is send deleting the respective jump (n-1):

```$bash

$ curl -XPOST -H "Content-type: application/json" -v -d '{
    "message": "Hello",
    "last_path": "/jump",
    "jump_path": "/jump",
    "jumps": [
        "http://back-golang:8442",
        "http://back-python:8444" 
    ]
}' 'back-springboot:8443/jump'  # Send a POST to SPRINGBOOT

## First Jump 
CLIENT -> SPRINGBOOT

'{
    "message": "Hello",
    "last_path": "/jump",
    "jump_path": "/jump",
    "jumps": [
        "http://back-golang:8442", # Send a POST to GOLANG
        "http://back-python:8444" 
    ]
}'

## Second Jump
SPRINGBOOT -> GOLANG

'{
    "message": "Hello",
    "last_path": "/jump",
    "jump_path": "/jump",
    "jumps": [
        "http://back-python:8444" # Send a GET to PYTHON
    ]
}'

## Third Jump (RESPONSE)
GOLANG -> PYTHON

{
    "code": 200,
    "message": "/jump - Greetings from Python!"
}
```

Regarding gRPC traffic, the backend components implement a gRPC service and operate as follows:

- Jump Request - Receive a JSON Object, called *Jump*, and send a request to the next *jump* based on the information included in jumps field in the object. When *jumps* field contains only one jump, the last backend return the final response:

```$bash

$  grpcurl -plaintext -d '{"count": 0, "message": "hola", "jumps": ["back-golang:50051","back-python:50053"]}' back-golang:50052 jump.JumpService/Jump

## First Jump 
CLIENT -> SPRINGBOOT

'{
    "message": "hola",
    "count": "1",
    "jumps": [
        "back-golang:50051",
        "back-python:50053" 
    ]
}'

## Second Jump
SPRINGBOOT -> GOLANG

'{
    "message": "hola",
    "count": "2",
    "jumps": [
        "back-python:50053" 
    ]
}'

## Third Jump (RESPONSE)
GOLANG -> PYTHON

{
    "code": 200,
    "message": "/jump - Greetings from gRPC Python!"
}
```

## Author Information

Asier Cidon @Red Hat

asier.cidon@gmail.com
