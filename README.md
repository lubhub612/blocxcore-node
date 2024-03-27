Blocxcore Node
============

A Blocx full node for building applications and services with Node.js. A node is extensible and can be configured to run additional services. At the minimum a node has an interface to [Blocx Core (blocxd) v0.13.0](https://github.com/dashpay/dash/tree/v0.13.0.x) for more advanced address queries. Additional services can be enabled to make a node more useful such as exposing new APIs, running a block explorer and wallet service.

## Usages

### As a standalone server

```bash
git clone https://github.com/lubhub612/blocxcore-node
cd blocxcore-node
npm install
./bin/blocxcore-node start
```

When running the start command, it will seek for `.blocxcore/blocxcore-node.json` conf file in the working directory (see [/docs/services/blocxd.md](/docs/services/blocxhd.md) for an example).
If it doesn't exist, it will create it, with basic task to connect to blocxd.

Some plugins are available :

- Insight-API : `./bin/blocxcore-node addservice @dibya6/insight-api`
- Insight-UI : `./bin/blocxcore-node addservice @dibya6/insight-ui`

You also might want to add these index to your blocx.conf file :
```
-addressindex
-timestampindex
-spentindex
```

### As a library

```bash
npm install @dibya6/blocxcore-node
```

```javascript
const blocxcore = require('@dibya6/blocxcore-node');
const config = require('./blocxcore-node.json');

let node = blocxcore.scaffold.start({ path: "", config: config });
node.on('ready', function () {
    console.log("Blocx core started");
    
    node.services.blocxd.on('tx', function(txData) {
        let tx = new blocxcore.lib.Transaction(txData);
        console.log(tx);
    });
});
```

## Prerequisites

- Blocx Core (blocxd) (v0.13.0) with support for additional indexing *(see above)*
- Node.js v8+
- ZeroMQ *(libzmq3-dev for Ubuntu/Debian or zeromq on OSX)*
- ~50GB of disk storage
- ~1GB of RAM

## Configuration

Blocxcore includes a Command Line Interface (CLI) for managing, configuring and interfacing with your Blocxcore Node.

```bash
blocxcore-node create -d <blocx-data-dir> mynode
cd mynode
blocxcore-node install <service>
blocxcore-node install https://github.com/yourname/helloworld
blocxcore-node start
```

This will create a directory with configuration files for your node and install the necessary dependencies.

Please note that [Blocx Core](https://github.com/dashpay/dash/tree/master) needs to be installed first.

For more information about (and developing) services, please see the [Service Documentation](docs/services.md).

## Add-on Services

There are several add-on services available to extend the functionality of Bitcore:

- [Insight API](https://github.com/lubhub612/insight-api/tree/master)
- [Insight UI](https://github.com/lubhub612/insight-ui/tree/master)
- [Bitcore Wallet Service](https://github.com/lubhub612/blocxcore-wallet-service/tree/master)

## Documentation

- [Upgrade Notes](docs/upgrade.md)
- [Services](docs/services.md)
  - [Blocxd](docs/services/blocxd.md) - Interface to Blocx Core
  - [Web](docs/services/web.md) - Creates an express application over which services can expose their web/API content
- [Development Environment](docs/development.md) - Guide for setting up a development environment
- [Node](docs/node.md) - Details on the node constructor
- [Bus](docs/bus.md) - Overview of the event bus constructor
- [Release Process](docs/release.md) - Information about verifying a release and the release process.


## Setting up dev environment (with Insight)

Prerequisite : Having a blocxd node already runing `blocxd --daemon`.

Blocxcore-node : `git clone https://github.com/lubhub612/blocxcore-node -b develop`
Insight-api (optional) : `git clone https://github.com/lubhub612/insight-api -b develop`
Insight-UI (optional) : `git clone https://github.com/lubhub612/insight-ui -b develop`

Install them :
```
cd blocxcore-node && npm install \
 && cd ../insight-ui && npm install \
 && cd ../insight-api && npm install && cd ..
```

Symbolic linking in parent folder :
```
npm link ../insight-api
npm link ../insight-ui
```

Start with `./bin/blocxcore-node start` to first generate a ~/.blocxcore/blocxcore-node.json file.
Append this file with `"@dibya6/insight-ui"` and `"@dibya6/insight-api"` in the services array.

## Contributing

Please send pull requests for bug fixes, code optimization, and ideas for improvement. For more information on how to contribute, please refer to our [CONTRIBUTING](https://github.com/dashevo/dashcore/blob/master/CONTRIBUTING.md) file.

## License

Code released under [the MIT license](https://github.com/lubhub612/blocxcore-node/blob/master/LICENSE).

Copyright 2016-2018 Blocx Core Group, Inc.

- bitcoin: Copyright (c) 2009-2015 Bitcoin Core Developers (MIT License)
