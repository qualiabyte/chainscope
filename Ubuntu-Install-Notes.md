
# Ubuntu 14.04 Install Notes

These notes guide you through setting up Bitcoin, Elasticsearch, and the
elasticsearch-blockchain module we use to transfer blocks between the two.
We've used package repositories where available to allow
for easy updates.

## Oracle JDK and Elasticsearch

Elasticsearch requires Java, and Oracle's official JDK is the recommended version. Here we install the latest version of each.

### Install Oracle JDK 8

    # Oracle JDK 8
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java8-installer

### Install Elasticsearch

    # Elastic Search
    wget -qO - https://packages.elasticsearch.org/GPG-KEY-elasticsearch | sudo apt-key add -
    sudo add-apt-repository "deb http://packages.elasticsearch.org/elasticsearch/1.4/debian stable main"
    sudo apt-get update && sudo apt-get install elasticsearch
    sudo update-rc.d elasticsearch defaults 95 10

### Secure Elasticsearch

To secure elasticsearch, edit the YAML configuration file.

    # Edit YAML file
    sudo vim /etc/elasticsearch/elasticsearch.yml

Here we configure elasticsearch to listen only to local clients.

    # Bind to local host
    network.bind_host: localhost

For more security, we could disable dynamic scripting. Since it's used by plugins like Kibana and Marvel Sense, we leave it enabled here -- but this means we should *only allow access to trusted clients*.

    # Don’t disable dynamic scripts
    # Note: Originally disabled this for security,
    # but it turns out Kibana needs this enabled
    # script.disable_dynamic: true

Now we can exit and restart elasticsearch.

    # Save, exit, and restart to enable changes
    sudo service elasticsearch restart

### UFW (Uncomplicated Firewall)

As an additional precaution, we can enable the UFW firewall for more control over client connections from remote IPs to various ports. Enabling the firewall starts it immediately and on future startup.

    sudo ufw enable

We can now allow all access from various IPs or subnets:

    sudo ufw allow from <trusted-ip>
    sudo ufw allow from <trusted-ip>/24

Or on various ports:

    # Open a local port to a trusted machine
    sudo ufw allow from <trusted-ip> to any port <number>

    # Open a local port to any machine
    sudo ufw allow <port>

### Marvel Sense

Now we can install the Marvel Sense plugin for Elasticsearch:

    cd /usr/share/elasticsearch/
    sudo ./bin/plugin -i elasticsearch/marvel/latest

## Clojure

Installing Clojure is purely optional! We're considering using it as an interface to Elasticsearch. Other methods are fine as well -- including the Elasticsearch libraries available in other languages, or using the REST API directly.

    # To install Clojure using Leiningen,
    # Get the lein command
    mkdir -p ~/bin
    wget -O ~/bin/lein https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein
    chmod a+x ~/bin/lein

## Bitcoin

### Installing Bitcoin

Here we install bitcoind and bitcoin-qt via the offical ppa.

    # Install bitcoin via ppa
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update && sudo apt-get install bitcoind bitcoin-qt

    # Configure bitcoind in ~/.bitcoin/bitcoin.conf
    $ vim ~/.bitcoin/bitcoin.conf
    $ chmod go-rwx ~/.bitcoin/bitcoin.conf

### Configuring Bitcoin

To transfer blocks, it's essential to enable bitcoind's JSON-RPC server.  
Be sure to set the rpc user and password.

    # ~/.bitcoin/bitcoin.conf

    # RPC Server
    server=1
    rpcuser=bitcoinrpc
    # rpcpassword=YOUR PASSWORD HERE!
    rpcport=8332
    rpcallowip=127.0.0.1
    rpcssl=0

    # RPC Client
    rpcconnect=127.0.0.1

    # Build Transaction Index
    txindex=1

    # Limit Networking
    # Note: Enable this only if you’d prefer
    # not to download additional blocks
    # maxconnections=0
    # listen=0

    # Enable Wallet
    disablewallet=0

### Installing Block Data

This optional step is useful for getting started with
the first few block data files, for example if you've downloaded
them via torrent rather than synchronizing the full chain with bitcoind.

    # Move blk000**.dat to ~/.bitcoin/blocks
    mv blk000**.dat ~/.bitcoin/blocks

### Building the Transaction Index

    # Read blocks and build transaction index
    bitcoind -reindex

## Node.js and NPM

    # Node.js
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs
    sudo npm install npm -g

## Elasticsearch-Blockchain

    # Clone the repo
    git clone https://github.com/orweinberger/elasticsearch-blockchain.git
    cd elasticsearch-blockchain

    # Install dependencies
    npm install .

    # Start bitcoind and elasticsearch
    bitcoind -daemon
    sudo service elasticsearch start

    # Edit the configuration
    cp config-example.yaml config.yaml
    vim config.yaml

    # Start transfer from bitcoind to elasticsearch
    node ./index.js
