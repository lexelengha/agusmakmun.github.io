---
layout: post
title:  "REST fundamentals"
date:   2021-07-02 18:00:00 +0200
categories: [rest]
---

* `REST` - or `Representational State Transfer`, is an architectural style for enforcing standards between computer systems on a network, making it easier for systems to exchange data with each other. RESTful systems, often referred to as RESTful, are stateless and separate the interests of the client and the server.

# Separating client and server

In the `REST` architectural style, the client implementation and the server implementation can be performed independently of each other. This means that the client-side code can be changed at any time without affecting the server's operation, and the server-side code can be changed without affecting the client's operation.

As long as each side knows what format the messages should be sent to the other side, they can be stored modularly and separately. By separating user interface tasks from storage tasks, we increase the flexibility of the interface between platforms and improve extensibility by simplifying server components. In addition, the separation allows each component to develop independently.

Using the `REST` interface, different clients go to the same `REST` endpoints, perform the same actions, and receive the same responses.

# Lack of persistence

Systems that follow the `REST` paradigm are stateless, which means the server doesn't need to know about the client's state and vice versa. Thus, both the server and the client can understand any message received without even seeing the previous messages. This lack of persistence is achieved through the use of resources, not commands. They describe any objects, documents, or things that might be required to be stored or sent to other services.

These constraints help `RESTful` applications achieve reliability, fast performance, and extensibility as components that can be managed, updated, and reused without affecting the entire system even while it is running.

In `REST` architecture, clients send requests to find or modify resources, and servers send responses to those requests. 

# Sending requests

`REST` requires the client to make a request to the server to get or change data on the server. The request usually consists of:

* `HTTP method`, which determines the type of operation;
* a `header` that allows the client to convey information about the request;
* `resource paths`;
* `an optional message body` containing data.

There are `4 main HTTP` methods that we use in requests to interact with resources in the `REST` system:

* `GET` - getting a specific resource (by id) or a collection of resources;
* `POST` - create a new resource;
* `PUT` - updating a specific resource (by id);
* `DELETE` - deleting a specific resource by id;

In the request header, the client sends a content type that it can receive from the server. This field is called `Accept`. It ensures that the server does not send data that cannot be understood or processed by the client. Content type parameters are `MIME` types (or `Multipurpose Internet Mail Extensions`).

For example, a text file containing `HTML` would be specified as `text/html`. If this text file contains `CSS`, then it will be listed as `text/css`. The general text file will be referred to as `text/plain`. However, this default, `text/plain`, is not exhaustive. If the client expects `text/css` but receives `text/plain`, it will not be able to recognize the content.

Other types and commonly used subtypes:

* image — `image/png`, `image/jpeg`, `image/gif`;
* audio — `audio/wav`, `audio/mpeg`;
* video — `video/mp4`, `video/ogg`;
* application — `application/json`, `application/pdf`, `application/xml`, `application/octet-stream`.

For example, a client with access to resource `ID 123` in the article resource on the server can send a `GET` request as follows:

```
GET/news/123
Accept: text/html, application/xhtml
```

Requests must contain the path to the resource on which the operation is to be performed. In a `RESTful API`, paths should be designed to help the client understand what's going on. Usually, the first part of the path should be the plural form of the resource. This makes nested paths easy to read and understand.

Paths should contain the information necessary to locate the resource with the necessary degree of specificity. When referencing a list or collection of resources, it is not always necessary to add an identifier. For example, a `POST` request to the path `somesite.com/persons` will not need an additional ID, since the server generates an ID for the new object.

In cases where the server sends a payload to the client, it must include the content type in the response header. This content header field warns the client about the type of data it is sending in the response body. These content types are `MIME` types, just as they are in the `Accept` field of the request header. The content type that the server returns back in the response must be one of the parameters specified by the client in the accept request field.

For example, a client accesses resource `ID 123` in the articles section with this `GET` request:

```
GET /news/123 HTTP/1.1
Accept: text/html, application/xhtml
```

The server should send back content with a response header:

```
HTTP/1.1 200 (OK)
Content-Type: text/html
```

This means that the requested content is returned in the response body with `text/html`, the type of content the client will be able to accept.

# Answer codes

The server replies contain status codes to notify the client of the success of the operation. As a developer, you don't need to know every status code (there are many), but you should know the most common ones and how they are used.

For each `HTTP` method, status codes are expected that the server should return if successful:

* GET — return `200 (OK)`
* POST — return `201 (CREATED)`
* PUT — return `200 (OK)`
* DELETE — return `204 (NO CONTENT)`

If the operation does not work, the most specific status code corresponding to the problem encountered will be returned.