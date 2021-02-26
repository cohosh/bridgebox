# bridgebox

A local test environment for testing BridgeDB and rdsys. This is **not** meant for production deployments of BridgeDB.

### Building the Docker containers

First, set the `$BRIDGEDB_REPO` environment variable to the absolute path of the cloned BridgeDB repository you're working with:
```
export BRIDGEDB_REPO=/path/to/bridgedb/repo
```

Pass in build arguments to Docker compose so that there aren't permission issues with the mounted BridgeDB code:
```
docker-compose build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g)
```
### Building and running BridgeDB

```
docker-compose up -d
```

To attach:
```
docker exec -it bridgedb /bin/bash
```

To build and install BridgeDB, run
```
cd bridgedb && sudo make install && cd ..
```
this will need to be re-run every time you make changes to the BridgeDB source code.

In order to fully test the changes, we need to make some mock bridges. This command will create a random assortment of both probe-resistant and regular bridges:
```
cd from-authority && ../bridgedb/scripts/create_descriptors --num-resistant-descs 100 200 && cd ..
```

And then finally, to run BridgeDB with the provided configuration file, 
```
bridgedb -c bridgedb.conf &
```

If you wish to make changes to the provided configuration file, not that you'll have to do so inside the container or rebuild the Docker container since it is copied into the image as part of the build process.

When BridgeDB is running, logs can be found in `/home/bridgedb/`.

You should now be able to navigate to `https://localhost` in your browser and interact with the BridgeDB instance.

### Running tests

You can run tests by executing `make coverage` or `make coverage-test` from within the bridgedb directory.
