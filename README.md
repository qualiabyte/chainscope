
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

For extracting blocks and transactions, we currently use [blockchain-elasticsearch](https://github.com/orweinberger/elasticsearch-blockchain),
 a Node.js module available on NPM.

## Development

We can still use help with:

+ Building interesting example queries
+ Discovering valuable use cases
+ Tuning bitcoind and Elasticsearch to index
   blocks and transactions completely and reliably
+ Providing a safe, read-only interface to ES for public use
+ Developing a nice front-end for simple queries or exploration

## Examples


For example Elasticsearch queries, see our [Chainscope Demo](Chainscope-Demo.txt).
