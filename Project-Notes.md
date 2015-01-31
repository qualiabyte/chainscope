
# Chainscope

## Overview

+ Project to load Bitcoin blockchain into Elasticsearch
+ An experiment in building Bitcoin infrastructure
+ Provides API for queries of blocks and transactions
+ May be more flexible in some ways than available APIs
+ Current use case: internal infrastructure

## About Elasticsearch

+ Search server built on Lucene indexing library
+ Designed for scalable, distributed, near real-time search
+ Written in Java by Shay Bannon, first release in 2010
+ Serves JSON over HTTP

## What's Lucene?

+ Document indexing and retrieval library
+ Written in Java by Doug Cutting, first release in 1999
+ Models data as documents with fields of text
+ Similar feeling to NoSQL document stores like MongoDB
+ Inverted indexes map contents to resources

## Why use Elasticsearch?

+ Good reputation for full-text search
+ Strong experience by one of our team members
+ More than just full-text search!
+ Easy to use, good performance

## A taste of Elasticsearch

### Comparison to Relational Databases

Relational DB  ⇒ Databases ⇒ Tables ⇒ Rows      ⇒ Columns
Elasticsearch  ⇒ Indices   ⇒ Types  ⇒ Documents ⇒ Fields

+ Schemaless documents
+ Data types are auto-detected!
+ Document fields are auto-indexed!

### Data Mapping

Maps JSON values to Java data types

+ String: string
+ Whole number: byte, short, integer, long
+ Floating-point: float, double
+ Boolean: boolean
+ Date: date

## What's elasticsearch-blockchain?

+ Node.js module written by Or Weinberger this year (Jan 2015)
+ Exports data from Bitcoind JSON-RPC
+ Imports data into Elasticsearch
+ Starts at block 1, pushes blocks sequentially
+ Resumes session from heighest block

## How elastic-blockchain works

+ Uses the `bitcoin` module on NPM
+ Reads configuration for bitcoind and ES
+ Checks last height in ES
+ Retrieves block from Bitcoind using `getBlockHash( height )`
+ Runs `getBlock( hash )` to get block as JSON
+ Runs `getRawTransaction( txID )` for every transaction in block
+ Runs `decodeRawTransaction( txData )` to get transaction as JSON

## Marvel Sense plugin

+ Great for prototyping queries
+ Auto-completion for ES syntax and live data
+ Free for development
+ Requires license in production

## Example Queries!

+ See the [Chainscope Demo](Chainscope-Demo.txt)

## Results and Statistics

1. Local Virtual Machine
  + Blocks 1 - 120,000
  + 1.8 GB in /var/lib/elasticsearch/
2. Home Node
  + Blocks 300,000 - 323,000
  + 42 GB in /data/es/nodes/indices/blocks
3. Elasticsearch-blockchain
  + According to es-blockchain docs
  + 220 GB for full blockchain

## Future Development and Directions

+ Build more interesting queries
+ Discover valuable use cases
+ Tune Bitcoind, Elasticsearch, and ES-blockchain
  for extracting the data completely and reliably
+ Safe, read-only API to Elasticsearch for public use
+ Nice front-end for simple searches and exploration
