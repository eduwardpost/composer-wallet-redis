# composer-wallet-redis

This is Hyperledger Composer Wallet implementation using [Redis](https://redis.io/) as a store.

_ *NOTE* that this is based on the preview version of the 'Cloud Wallet' feature. The API that needs to be implemented should be expected to change_

## Usage

The steps below assume that you have an application or playground, or rest server for Hyperledger Composer that wish to use.
Also it assumes you are familar with NPM, and the card concept in the Composer


### *Step 1*

Run the `startRedis.sh` script to get the docker image for redis started and running.
The CLI for redis can be started with

```
docker run -it --link composer-wallet-redis:redis --rm redis redis-cli -h redis -p 6379
```

Will refer you to web to find out more about running redis and querying it

### *Step 2*

Firstly, this module that provides the support to connect from Composer to the Object Storage needs to be installed.
This is loaded using a node.js require statment, and the current preview will look for this in the global modules. 

```
npm install -g @ampretia/composer-waller-redis
```

### *Step 3*

Configuration needs to be passed to the client appliation using composer to use this new wallet.

There are two main ways this can be achieved. Via configuration file, or via environment variables. 

*File*
Assuming that you do not have the config directory already - this is using the standard node npm `config` module


- Create a directory `config` in the current working directory of the application
- Create a file `default.json` in this `config` directory
- Ensure that the contents of the file are
```
{
  "composer": {
    "cardstore": {
      "type": "@ampretia/composer-wallet-redis",
      "desc": "Uses a local redis instance,
      "options": {

      }
    }
  }
}
```

- `type` is the name of this module
- `desc` is some text for the humans

If you run the local docker image for redis, there is no specific configuration that is required. The redis client used will be default connect to the port that is run. More information on the client is at https://www.npmjs.com/package/redis
Any options specified in options, will be passed directly into the `redis.createClient()` function


*Environment Variable*

As this is using the *config* module specifing the details on the command line via environment variables can be achieved by

```
export NODE_CONFIG={"composer":{"cardstore":{"type":"@ampretia/composer-wallet-redis","desc":"Uses  a local redis instance,"options":{}}}}
```

The any application (or command line, eg `composer card list`) that is in this shell will use the cloud wallets. 