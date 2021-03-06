# Exchanger #

Erlang financial exchange front-end.

- [Exchanger](#exchanger)
    - [Purpose](#purpose)
    - [Overview](#overview)
        - [Order processing](#order-processing)
        - [Authentication](#authentication)
    - [Communication](#communication)
        - [GPB specifics](#gpb-specifics)
        - [Front-end <-> Directory](#front-end---directory)
        - [Front-end <-> Client](#front-end---client)
        - [Front-end <-> Exchange](#front-end---exchange)
    - [Instalation](#instalation)
    - [Dependencies](#dependencies)

## Purpose ##

Is in charge of client authentication and redirecting buy and sell orders to the approriate exchange.

-----------------------

## Overview ##

### Order processing ###

[Described here](https://github.com/Seriyin/Exchanger-Server#order-processing)

Based on this specification front-end will:

- Forward trade orders to the relevant exchange.
- Forward ~~subscription requests for up to 10 companies per user to the relevant exchanges~~ relevant exchange address back to client **\***.

<sub> [*]  _For better performance exchanges publish directly to client unfortunatly making it impossible to globally restrict by company, only by exchange._ <sub>

-----------------------

### Authentication ###

[Described here](https://github.com/Seriyin/Exchanger-Client#authentication)

-----------------------

## Communication ##

_Front-end server will work with serialized pertinent information (client authentication requests, trade and consultation information) using [Protocol Buffers](https://github.com/google/protobuf)._

### GPB specifics ###

Protocol buffer messages will be grouped into wrapper messages to decode from multiple options. In Erlang, hasField() methods do not make sense, so a boolean value must be explicitly set for each possible message needing decoding on *Exchanger*.

### Front-end <-> Directory ###

Front-end server will be the client gateway to the [REST directory service](https://github.com/Seriyin/Exchanger-Directory).

Front-end must:

- Forward the directory service address directly to the client.
- Alert client in the event directory is unavailable.

-----------------------

### Front-end <-> Client ###

Front-end server will deal with requests of authentication and trade ordering.

Front-end must:

- Authenticate clients.
- Forward trade orders.
- Forward trade completions from exchanges back to client.

-----------------------

### Front-end <-> Exchange ###

Front-end server will forward trade orders to exchanges and trade completions back to client.

-----------------------

## Instalation ##

Run 

>*$>* rebar3 release -n prod

for production version with self-contained erts or

>*$>* rebar3 release -n dev

for dev version in main rebar.config directory.

-----------------------

## Dependencies ##

- Depends on [Chumak native erlang implementation of ZeroMQ](https://github.com/zeromq/chumak).
- ~~Depends on [jsone](https://github.com/sile/jsone) for handling JSON serialization natively on Erlang.~~ **\***
- Depends on [gpb](https://github.com/tomas-abrahamsson/gpb) for handling protocol buffers natively in Erlang.

Dependencies are resolved through use of [rebar3](https://github.com/erlang/rebar3).

[*] Shifted JSON dependency to client.
