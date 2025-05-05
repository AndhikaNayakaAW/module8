# Reflection
**Name:** Andhika Nayaka Arya Wibowo  
**NPM:** 2306174135

## 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC methods, and in what scenarios would each be most suitable?
- **Unary RPC**: client sends one request and gets one response.
    - *Use case*: simple operations like fetching a user profile or submitting a single payment.
- **Server-streaming RPC**: client sends one request and gets a stream of responses.
    - *Use case*: downloading a list of transactions or a log file in chunks.
- **Bi-directional streaming RPC**: both client and server send streams of messages independently.
    - *Use case*: live chat, real-time gaming updates, or continuous sensor data exchange.

## 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
- **Authentication**: verify client identity (e.g., TLS client certificates or JWT tokens).
- **Authorization**: check user permissions before executing each RPC method.
- **Encryption**: use TLS (HTTP/2) to encrypt data in transit and prevent eavesdropping.

## 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
- **Concurrency**: managing multiple streams safely without data races.
- **Back-pressure**: preventing fast producers from overwhelming slow consumers.
- **Error handling**: dealing with mid-stream failures and cleanly closing both sides of the stream.

## 4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?
- **Advantages**:
    - Easy conversion from a Tokio channel to a gRPC stream.
    - Integrates well with async tasks and Tokio runtime.
- **Disadvantages**:
    - Limited buffer size must be tuned to avoid blocking or dropped messages.
    - Adds an extra layer of abstraction that can obscure error sources.

## 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
- **Separate modules**: place each service implementation in its own Rust module/sub-crate.
- **Traits and generics**: define common traits for shared logic (e.g., logging, error handling).
- **Helper utilities**: extract repeated setup (channel, server builder) into reusable functions.

## 6. In the `MyPaymentService` implementation, what additional steps might be necessary to handle more complex payment processing logic?
- **Validation**: check request parameters (amount > 0, valid user ID).
- **Business rules**: apply fraud checks, balance updates, external gateway integration.
- **Error reporting**: return detailed status codes and error messages for failures.

## 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
- **Strong typing**: schema-first approach via Protobuf enforces consistent contracts.
- **Cross-language support**: auto-generated stubs for many languages improve interoperability.
- **Tighter coupling**: more rigid contracts vs. flexible JSON/HTTP REST, potentially harder to evolve.

## 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
- **Advantages**:
    - Multiplexing: multiple streams over one TCP connection reduces latency.
    - Header compression: HPack cuts down on redundant header data.
    - Server push: preemptively send data before the client asks.
- **Disadvantages**:
    - Complexity: harder to debug than simple HTTP/1.1 calls.
    - Browser support: limited—gRPC-Web or proxies often required.

## 9. How does the request-response model of REST APIs contrast with the bi-directional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
- **REST (request-response)**: each interaction is a separate HTTP call; good for simple CRUD but adds overhead per call.
- **gRPC streaming**: long-lived connections allow real-time, low-latency data flows; ideal for continuous updates.

## 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
- **Protocol Buffers**:
    - Compact binary format → smaller messages and faster parsing.
    - Strict schemas → early error detection but less flexible for ad-hoc fields.
- **JSON**:
    - Human-readable and flexible → easier to extend without breaking existing clients.
    - Larger payloads and slower parsing compared to Protobuf.