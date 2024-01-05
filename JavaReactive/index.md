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

## Tools
    - netty - non blocking Server uses Event Loop Model
    - Project Reactor - for writing non blocking code
    - Spring WebFlux - uses Netty and Project Reactor for building non blocking or reactive APIs
