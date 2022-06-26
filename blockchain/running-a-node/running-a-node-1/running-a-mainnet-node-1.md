---
description: Running a testnet node for pio-testnet-1 from a quicksync file.
---

# Running a testnet node from quicksync

The steps for running testnet are exactly the same as mainnet except that the github repo is here https://github.com/provenance-io/testnet and the chain id is `pio-testnet-1`

````markup
Step 1:download the latest quickysync via https://test.provenance.io/quicksync.
Step 2:Note down the provenance version required for the quicksync file downloaded in step 1.
Step 3:Download appropriate release from https://github.com/provenance-io/provenance/releases/ noted from step 2.
Step 4:Untar data directory from the quicksync download and replacing the untarred data directory to $PIO_HOME/data
Step 5: Run the below commands
export PIO_HOME=~/.provenanced // or directory of your choosing.
provenanced init choose-a-moniker --chain-id pio-testnet-1
curl https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/genesis.json> genesis.json
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
Step 6: provenanced start --testnet --p2p.seeds 2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51:26656 --x-crisis-skip-assert-invariants
````
