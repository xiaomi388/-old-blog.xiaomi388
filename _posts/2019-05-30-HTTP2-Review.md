---
layout: post
title: HTTP/2 Review
tags: [Network]
---

A note of *Introduction to HTTP/2*.

# Introduction

Primary goals for HTTP/2:

- **Reduce latency** by enabling full request and response multiplexing
- **Minimize protocol overhead** via efficient compression of HTTP header fields
- Add support for **request prioritization** and **server push**

What HTTP/2 does:

- Modifies how the data is formatted (framed) and transported between the client and server, both of which manage the entire process
- Hides all the complexity from our applications within the new framing layer.

Not compatible with previous HTTP/1.x:

- HTTP/2 introduces a new binary framing layer that is not backward compatible with previous HTTP/1.x servers and clients

Disadvantages of HTTP/1.x:

- HTTP/1.x clients need to use **multiple connections** to achieve concurrency and reduce latency;
- HTTP/1.x does not **compress request and response headers**, causing unnecessary network traffic; 
- HTTP/1.x does not allow effective **resource prioritization**, resulting in poor use of the underlying TCP connection; 

# Binary framing layer

![binary framing layer](/assets/img/HTTP2-Review/binary_framing_layer01.svg)

The frame layer introduces a new optimized encoding mechanism between the socket interface and the higher HTTP API exposed to our applications.

Unlike the newline delimited plaintext HTTP/1.x protocol, all HTTP/2 communication is split into smaller messages and frames, each of which is encoded in binary format.

# Streams, messages, and frames

- *Stream*: A bidirectional flow of bytes within an established connection, which may carry one or more messages. Stream can be thought of as a logical single request/response communication.
- *Message*: A complete sequence of frames that map to a logical request or response message.
- *Frame*: The smallest unit of communication in HTTP/2, each containing a frame header, which at a minimum identifies the stream to which the frame belongs.

![streams, messages, and frames](/assets/img/HTTP2-Review/streams_messages_frames01.svg)

## Request and response multiplexing

HTTP/1.x connection: 

- only one response can be delivered at a time (response queuing) per connection. 
- This results in head-of-line blocking and inefficient use of the underlying TCP connection.

HTTP/2 connection:

- The new binary framing layer in HTTP/2 removes these limitations.

![multiplexing](/assets/img/HTTP2-Review/multiplexing01.svg)

The new binary framing layer in HTTP/2 resolves the head-of-line blocking problem found in HTTP/1.x and eliminates the need for multiple connections to enable parallel processing and delivery of requests and responses. 

### HTTP Head of line blocking

Head of Line blocking (in HTTP/1.1 terms) is often referring to the fact that each client has a limited number of TCP connections to a server (usually 6 connections per hostname) and doing a new request over one of those connections has to wait for the previous request on the same connection to complete before the client can make a new request.

HTTP/1.1 introduced a feature called "[Pipelining](https://en.wikipedia.org/wiki/HTTP_pipelining)" which allowed a client sending several HTTP requests over the same TCP connection. However HTTP/1.1 still required the responses to arrive in order so it didn't really solved the HOL issue and as of today it is not widely adopted.

HTTP/2 (h2) solves the HOL issue by means of multiplexing requests over the same TCP connection, so a client can make multiple requests to a server without having to wait for the previous ones to complete as the responses can arrive in any order.

## Stream prioritization

The HTTP/2 standard allows each stream to have an associated weight and dependency:

- Each stream may be assigned an integer weight between 1 and 256.
- Each stream may be given an explicit dependency on another stream.

Here is an example to clarify how prioritization works:

![stream_prioritization01](/assets/img/HTTP2-Review/stream_prioritization01.svg)

1. Neither stream A nor B specifies a parent dependency and are said to be dependent on the implicit "root stream"; A has a weight of 12, and B has a weight of 4. Thus, based on proportional weights: stream B should receive one-third of the resources allocated to stream A.
2. Stream D is dependent on the root stream; C is dependent on D. Thus, D should receive full allocation of resources ahead of C. The weights are inconsequential because C’s dependency communicates a stronger preference.
3. Stream D should receive full allocation of resources ahead of C; C should receive full allocation of resources ahead of A and B; stream B should receive one-third of the resources allocated to stream A.
4. Stream D should receive full allocation of resources ahead of E and C; E and C should receive equal allocation ahead of A and B; A and B should receive proportional allocation based on their weights.

**Note:  Stream dependencies and weights express a transport preference, not a requirement.**

## One connection per origin

All HTTP/2 connections are persistent, and only one connection per origin is required, which offers numerous performance benefits.

## Flow control

HTTP/2 provides a set of simple building blocks that allow the client and server to implement their own stream- and connection-level flow control:

- Flow control is directional. Each receiver may choose to set any window size that it desires for each stream and the entire connection.
- Flow control is credit-based. Each receiver advertises its initial connection and stream flow control window (in bytes), which is reduced whenever the sender emits a DATA frame and incremented via a WINDOW\_UPDATE frame sent by the receiver.
- Flow control cannot be disabled. When the HTTP/2 connection is established the client and server exchange SETTINGS frames, which set the flow control window sizes in both directions. The default value of the flow control window is set to 65,535 bytes, but the receiver can set a large maximum window size (2^31-1 bytes) and maintain it by sending a WINDOW\_UPDATE frame whenever any data is received.
- Flow control is hop-by-hop, not end-to-end. That is, an intermediary can use it to control resource use and implement resource allocation mechanisms based on own criteria and heuristics.

## Server push

Why would we need such a mechanism in a browser? A typical web application consists of dozens of resources, all of which are discovered by the client by examining the document provided by the server. As a result, why not eliminate the extra latency and let the server push the associated resources ahead of time? The server already knows which resources the client will require; that’s server push.

### PUSH\_PROMISE 101

All server push streams are initiated via PUSH_PROMISE frames, which signal the server’s intent to push the described resources to the client and need to be delivered ahead of the response data that requests the pushed resources. 

Once the client receives a PUSH_PROMISE frame it has the option to decline the stream (via a RST_STREAM frame) if it wants to. (This might occur for example because the resource is already in cache.) 

Preferences are communicated via the SETTINGS frames at the beginning of the HTTP/2 connection and may be updated at any time.

## Header compression

HTTP/2 compresses request and response header metadata using the HPACK compression format that uses two simple but powerful techniques.

1. It allows the transmitted header fields to be encoded via a static Huffman code, which reduces their individual transfer size.
2. It requires that both the client and server maintain and update an indexed list of previously seen header fields (in other words, it establishes a shared compression context), which is then used as a reference to efficiently encode previously transmitted values.



