---
description: running a mainnet node for pio-mainnet-1
---

# Running a mainnet node

The steps for running mainnet are exactly the same as testnet except that the github repo is here [https://github.com/provenance-io/mainnet](https://github.com/provenance-io/mainnet) and the chain id is `pio-mainnet-1`

````markup
Step 1:download the latest quickysync via https://provenance.io/quicksync and latest provenanced version.
At the time of writing this document latest version on mainnet was v1.7.6.
Download release from https://github.com/provenance-io/provenance/releases/
For e.g for version v1.7.6 url is https://github.com/provenance-io/provenance/releases/tag/v1.7.6
Step 2:Untar data directory from the quicksync download and replacing the untarred data directory to $PIO_HOME/data
Step 3: Run the below commands
export PIO_HOME=~/.provenanced // or directory of your choosing.
provenanced init choose-a-moniker --chain-id pio-mainnet-1
curl https://raw.githubusercontent.com/provenance-io/mainnet/main/pio-mainnet-1/genesis.json> genesis.json
mv genesis.json $PIO_HOME/config
Step 4:Change config.toml to have the db-backend set to `cleveldb` 
```# Database backend: goleveldb | cleveldb | boltdb | rocksdb | badgerdb
# * goleveldb (github.com/syndtr/goleveldb - most popular implementation)
#   - pure go
#   - stable
# * cleveldb (uses levigo wrapper)
#   - fast
#   - requires gcc
#   - use cleveldb build tag (go build -tags cleveldb)
# * boltdb (uses etcd's fork of bolt - github.com/etcd-io/bbolt)
#   - EXPERIMENTAL
#   - may be faster is some use-cases (random reads - indexer)
#   - use boltdb build tag (go build -tags boltdb)
# * rocksdb (uses github.com/tecbot/gorocksdb)
#   - EXPERIMENTAL
#   - requires gcc
#   - use rocksdb build tag (go build -tags rocksdb)
# * badgerdb (uses github.com/dgraph-io/badger)
#   - EXPERIMENTAL
#   - use badgerdb build tag (go build -tags badgerdb)
db_backend = "cleveldb```
Step 5: provenanced start --p2p.seeds 4bd2fb0ae5a123f1db325960836004f980ee09b4@seed-0.provenance.io:26656, 048b991204d7aac7209229cbe457f622eed96e5d@seed-1.provenance.io:26656 --x-crisis-skip-assert-invariants
````
