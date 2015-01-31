
# Chainscope

Chainscope is a project which loads the Bitcoin blockchain
into Elasticsearch, so that the content of every field is
automatically indexed and searchable.
It's an experiment that shows one way to build your own
Bitcoin infrastructure, that may be more flexible in some ways
than available APIs.

## Tools

We're using the following tools:

+ Ubuntu 14.04
+ Bitcoind
+ Elasticsearch 1.4
+ Marvel Sense plugin

For extracting blocks and transactions, we currently use [elasticsearch-blockchain](https://github.com/orweinberger/elasticsearch-blockchain),
 a Node.js module available on NPM.

## Development

Future directions for development include:

+ Building interesting example queries
+ Discovering valuable use cases
+ Tuning bitcoind and Elasticsearch to index
   blocks and transactions completely and reliably
+ Providing a safe, read-only interface to ES for public use
+ Developing a nice front-end for simple queries or exploration

## Learn More

+ For details about the project, see [Project Notes](Project-Notes.md).
+ For example Elasticsearch queries, see the [Chainscope Demo](Chainscope-Demo.txt).
+ For experimenting with your own node, see our [Ubuntu Install Notes](Ubuntu-Install-Notes.md).

## About

A project for Blockchain University 2015 by

+ Ben Bamberger
+ Eliot Weber
+ Tyler Florez
+ Viktor Korbukh
