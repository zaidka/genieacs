# GenieACS

GenieACS is a high performance Auto Configuration Server (ACS) for remote
management of TR-069 enabled devices. It utilizes a declarative and fault
tolerant configuration engine for automating complex provisioning scenarios at
scale. It's battle-tested to handle hundreds of thousands and potentially
millions of concurrent devices.

## Quick Start

*Important: This is a pre-release branch. Use
[v1.1](https://github.com/genieacs/genieacs/tree/v1.1) for production
deployments.*

Install [Node.js](http://nodejs.org/) and [MongoDB](http://www.mongodb.org/).
Refer to their corresponding documentation for installation instructions. The
supported versions are:

- Node.js: 10.x through 12.x
- MongoDB: 2.6 through 4.1

Clone this repo or download the source archive then *cd* into the source
directory and install the required dependencies:

    npm install

Now build:

    npm run build

Finally, run the following services from the build that's generated under
'dist' directory:

### genieacs-cwmp

This is the service that the CPEs will communicate with. It listens on port
7547 by default. Configure the ACS URL in your devices accordingly.

    ./dist/bin/genieacs-cwmp

You may optionally use [genieacs-sim](https://github.com/genieacs/genieacs-sim)
as a dummy TR-069 simulator if you don't have a CPE at hand.

### genieacs-nbi

This is the northbound interface module. It exposes a REST API on port 7557 by
default. This one is only required if you have an external system integrating
with GenieACS using this API.

    ./dist/bin/genieacs-nbi

### genieacs-fs

This is the file server from which the CPEs will download firmware images and
such.

    ./dist/bin/genieacs-fs

### genieacs-ui

This serves the web based user interface. It listens on port 3000 by default.

    ./dist/bin/genieacs-ui --ui-jwt-secret secret

The argument *--ui-jwt-secret* supplies the key used for signing browser
cookies.

The UI has plenty of configuration options. To populate the an initial sample
configuration, run:

    ./dist/tools/configure-ui

Use admin/admin credentials to log in.

### Docker deployment

Maybe you want to deploy in your machine without installing all dependencies.  
In this case, you can simply run:

```shell
# You volume must have this file
cp config/config-sample.json config/config.json
docker-compose up -d
```

This will turn on:

- MongoDB
- Redis
- CWMP
- FS
- NBI

Then you can test your devices directly in your machine.  
If you change some code, you can just restart the service you changed by docker-compose.  
The source is already mounted to make development easier.

Useful commands when deployed by docker-compose:

```shell
# See logs like tailf
docker-compose logs -f [service]

# Restart some service
docker-compose restart [service]

# Build again (if you add some new in package.json)
docker-compose build [service]

# Access mongodb database
docker-compose exec mongo mongo genieacs
```

## Support

The [forum](https://forum.genieacs.com) is a good place to get guidance and
help from the community. Head on over and join the conversation! In addition,
the [wiki](https://github.com/genieacs/genieacs/wiki) provides useful
documentation and tips from GenieACS users.

For commercial support options and professional services, please visit
[genieacs.com](https://genieacs.com/support/).

## License

Copyright 2013-2019 GenieACS Inc. GenieACS is released under the [AGPLv3
license
terms](https://raw.githubusercontent.com/genieacs/genieacs/master/LICENSE).
