# docker-fulcrum

A Docker container for running a [Fulrum server](https://github.com/cculianu/Fulcrum) and interfacing to it using a standard REST API.

## Install

These instructions assume you are using Docker installed on Ubuntu 18.04.

- Clone the repo: `git clone https://github.com/Permissionless-Software-Foundation/docker-fulcrum && cd docker-fulcrum`
- Edit the mainnet.conf or testnet.conf file to reflect your settings.
- Download a pre-synced database from the [CashStrap page](https://fullstack.cash/cashstrap), or you can sync from genesis.
- Edit the docker-compose.yml file to point to where the database should live.

### Create SSL certificate

The [electrum-cash](https://www.npmjs.com/package/electrum-cash) library for interfacing with the Electrumx protocol requires an SSL certificate in order to operate. You need to generate a self-signed
certificate for the REST API server to communicate with Fulcrum.

This does not effect the SSL connection to the REST API. It can still be secured using nginx or Apache and an SSL certificate from Let's Encrypt.

#### Generate a self-signed Certificate

Follow these instructions to generate your own self signed certificate. You'll
end up with two files: server.crt is the public key and certificate. server.key
is the private key.

- `sudo apt update`
- `sudo apt install openssl`
- `openssl genrsa -des3 -out server.pass.key 2048`
- `openssl rsa -in server.pass.key -out server.key`
- `rm server.pass.key`
- `openssl req -new -key server.key -out server.csr`
- `openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt`
- `rm server.csr`

In the same directory as the `docker-compose.yml` file, create a directory called `certs`. Move the `server.*` files into the `certs` directory.

### Build the Docker Container

- Update the `docker-compose.yml` file with the path to where you want to store the blockchain data.
- `docker-compose build`
- `docker-compose up -d`

## License

[MIT](./LICENSE.md)
