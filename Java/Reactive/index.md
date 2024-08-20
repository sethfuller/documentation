# Reactive Programming

## Features
    - Reactive Programming is a new programming paradigm
    - Asynchronous and non blocking
    - Data flows as an **Event/Message** driven stream
    - Functional Style Code
    - BackPressure on Data Streams

## API
### BackPressure
The app controls the data flow

    - requestForData() - From app
    - request(2) - request 2 From app
    - onNext(1) - From data source
    - onNext(2) - From data source
    - cancel() - From app - done processing data

## Reactive Streams
    - Publisher - subscribe(Subscriber<T> subscriber)
    - Subscriber
    - Subscription
    - Processor
      - extends Subscriber and Publisher

## Tools
    - netty - non blocking Server uses Event Loop Model
    - Project Reactor - for writing non blocking code
    - Spring WebFlux - uses Netty and Project Reactor for building non blocking or reactive APIs

## Flux and Mono
    - Flux and Mono is a reactive type that implements the Reactive Streams Specification
    - Flux and Mono are part of the **reactor-core** module
    - Flux is a reactive type to represent 0 to N elements
    - Mono is a reactive type to represent 0 to 1 element

## Flux
### map()
    - One to One transformation
    - Does the simple transformation from **T** to **V**
    - Used for simple synchronous transformations
    - Does Not support transformations that return Publisher

### flatMap()
    - One to N transformations
    - Does more than just transformation. Subscribes to Flux or Mono
      that's part of the transformation and then flattens it and
      sends it downstream.
    - Used for asynchronous transformations
    - Use it with transformations that return Publisher

### concatMap()
    - Use if ordering matters (similar to flatMap)

