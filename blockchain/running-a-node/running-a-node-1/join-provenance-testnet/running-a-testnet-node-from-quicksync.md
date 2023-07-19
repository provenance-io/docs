---
description: Running a testnet node for pio-testnet-1 from a quicksync file.
---

# Running a testnet node from quicksync



The steps for running testnet are exactly the same as mainnet except that the github repo is here https://github.com/provenance-io/testnet and the chain id is `pio-testnet-1`

````markup
Step 1:download the latest quickysync via https://test.provenance.io/quicksync.
Also Also note there is much smaller download available here `https://tools.highstakes.ch/provenance_snapshot` which is preferable to use to get started and also works the same as quicksync download.
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
Step 6: provenanced start --testnet --p2p.seeds c8818d411225372c66cccecc88dcc581ff7f2c03@104.198.181.197:26656,2105e36647e7007bfa7fdea8d45eb3b4d6a69f36@34.75.213.5:26656,08cf5b5fa2b80bf86a6370aec4c4473a0084195b@34.83.1.25:26656 --x-crisis-skip-assert-invariants
````
