# Data Sharing with Portfolio Manager

Figure manages and trades its loans on the [Figure Portfolio Manager](../portfolio-manager-market-place.md) and Marketplace. To allow these applications to read the loan data in the LOS P8e object store, the LOS has to “share” the data by permissioning the public keys for the other applications. When P8e scopes are permissioned to another public key, the P8e environment takes care of replicating the data to the encrypted object store of the other application. Future mutations of the scope through contract execution will also be replicated to keys that are permissioned to read the scope.

