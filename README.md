# docker-fulcrum

A Docker container for running a [Fulcrum server](https://github.com/cculianu/Fulcrum) and interfacing to it using a standard REST API.

Fulcrum is an indexer. An indexer is like a search engine, which crawls the blockchain and *indexes* the raw data into a database. The Full Node only tracks transactions. It does not track important data like address balances, UTXOs, and transaction histories. The Fulcrum indexer exists to track those things. It implements the [Electrum protocol](https://electrumx.readthedocs.io/en/latest/protocol.html).

## Installation

These instructions assume you are using Docker installed on a Ubuntu Linux operating system.

- Clone the repository:
  - `git clone https://github.com/Permissionless-Software-Foundation/docker-fulcrum`
- Navigate to the directory for your architecture.
- Edit the mainnet.conf file to reflect your settings.
- Edit the docker-compose.yml file to point to where the database should live.
- Create a directory called `certs` in the same directory as the docker-compose.yml file.

### Create SSL certificate

The [electrum-cash](https://www.npmjs.com/package/electrum-cash) library for interfacing with the Electrumx protocol requires an SSL certificate in order to operate. You need to generate a self-signed
certificate for the REST API server to communicate with Fulcrum.

This does not effect the SSL connection to the REST API. It can still be secured using nginx or Apache and an SSL certificate from Let's Encrypt.

#### Generate a self-signed Certificate

Follow these instructions to generate your own self signed certificate. You'll
end up with two files: server.crt is the public key and certificate. server.key
is the private key.

- `cd certs` - Enter the newly created *certs* directory.
- `sudo apt update`
- `sudo apt install openssl`
- `openssl genrsa -des3 -out server.pass.key 2048`
- `openssl rsa -in server.pass.key -out server.key`
- `rm server.pass.key`
- `openssl req -new -key server.key -out server.csr`
- `openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt`
- `rm server.csr`

### Build the Docker Container

- Update the `docker-compose.yml` file with the path to where you want to store the blockchain data.
- `docker compose build`
- `docker compose up -d`

## License

[MIT](./LICENSE.md)
