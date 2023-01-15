# Orbital Witness DevOps Interview Test

## Solution

I have set up a git hub actions CI pipeline that will build 3 images on commit to main to this repo and push them to docker hub. End users can stand up a local environment by using the docker compose file by running 'docker compose up' 

The application can be hit by running:

  http --form post http://localhost/token     username=alice password=password

  http --form post http://localhost/orders     title_number=OW12345678     document_type=lease    token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImFsaWNlIiwiY2FuX3RyYW5zYWN0Ijp0cnVlLCJleHAiOjE2NzMyODYxNTh9.gOaA5omC38CtHJmNArJBv-3Ej2J3hInS_3d6LUluxKk

## Background

Orbital Witness is a building a document ordering and processing system using software-oriented architecture principles.

As a DevOps engineer, you are tasked with improving the engineering efficiency by producing automation tools
that provision services in an efficient, predictable and reproducible way.

We use [HTTPie](https://github.com/httpie/httpie) in our examples for clarity.

## Architecture Overview

```
                      +-----------------------------------+
                      |                                   |
                      |         Redis Message Queue       |
                      |                                   |
                      +--------^------------------+-------+
                               |                  |
                              (4)                (6)
                               |                  |
+------------+       +---------+---+      +-------v------+
|            |       |             |      |              |
|  Auth API  |       |  Order API  |      |  OCR Worker  | (*n)
|            |       |             |      |              |
+--^------+--+       +--^-------+--+      +--------------+
   |      |             |       |
  (1)    (2)           (3)     (5)
   |      |             |       |
+--+------v-------------+-------v---+
|                                   |
|              Customer             |
|                                   |
+-----------------------------------+
```

There are 3 distinct services:

- [Auth API] - Creates authentication tokens to valid users.
- [Order API] - Processes document orders.
- [OCR Worker] - Performs Optical Character Recognition (OCR) against documents.

A document order flow is as follows:

1. A customer authenticates with the [Auth API] by providing a valid username and password.
2. An authentication token that is valid for 30 seconds is returned.
3. Use this authentication token to make a document order request to the [Order API].
4. The [Order API] submits the order to a message queue for further background processing.
5. A successful response is returned.
6. Meanwhile, a [OCR Worker] processes the order from the message queue by performing OCR against the document.

We use environment variables to configure a service.

## Your task

- Read the documentation for each service. Run and test them to make sure they work as expected.
- Once you're familiar with the services, use your preferred tools to automate provisioning of a **local development** environment.
- Be sure to include external dependencies like Redis and configuration management.
- In order for a microservices-based architecture to work best, implement a HTTP router in front of the services that routes
  requests to the correct service. A popular solution is path-based routing:
  - http://platform/authentication/_ → http://authentication-api/_
  - http://platform/order/_ → http://order-api/_
- Make your automation available on a public GitHub repository with a `README` on how to get things started.

Don't worry too much about making this production ready. We may discuss production considerations with you later on.

## Bonus (Optional)

- Containerise services.
- Apply the [Twelve Factor App](https://12factor.net/) methodology.

[auth api]: auth-api
[order api]: order-api
[ocr worker]: ocr-worker
