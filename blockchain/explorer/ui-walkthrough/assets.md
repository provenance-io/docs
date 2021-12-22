# Assets

Assets are typically known as [markers](../../../modules/marker-module.md) or tokens. They are the representation of coins on the blockchain. An asset can contain just itself, or other assets and [NFTs](forthcoming/nfts.md). In more abstract terms, if an asset is a fund, that fund can contain shares of other funds and distinct non-fungible assets.&#x20;

## Listview

![A listview of assets on the blockchain](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 8.23.52 PM.png>)

The listview shows a paginated list of assets on the blockchain, sorted by **Last Tx** descending. This means that the most commonly used assets will show at the top of the list.

Information to note:

* **Name**: the common name for the asset. This is determined by the asset's metadata, ie `nhash` is more commonly known as `hash`
* **Total Supply**: the total circulating supply sitting in regular accounts
* **Holding Account**: the escrow account for the asset
* **Marker Type**:&#x20;
  * Coin -> a marker that represents a standard fungible coin (default)
  * Restricted -> a marker that represents a denom with `send_enabled = false`
* **Mintable**: whether the asset can be minted through a regular transaction&#x20;
* **Last Tx**: the latest transaction timestamp that applied to the asset&#x20;
* **Age**: how old the latest transaction is in real time

## Detail

![Header asset information](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.19.00 PM.png>)

Information to note:

* **Asset Name**: the common name for the asset. This is determined by the asset's metadata, ie `nhash` is more commonly known as `hash`
* **Min Unit** / **Decimal**: the minimum unit is the smallest unit of the asset. When you have a fraction of the asset, it is calculated in terms of the minimum unit. Decimal is how many decimal places from the minimum unit to the asset unit, ie from `nhash` to `hash`, its 9 decimal places, where `1000000000 nhash = 1 hash`
* **Holders**: how many addresses hold the asset
* **Fungible Tokens**: how many distinct fungible tokens the asset holds
* **Non-Fungible Tokens**: how many non-fungible tokens the asset holds

### Managing Accounts

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.19.09 PM.png>)

An asset can be managed by governance alone, or additional addresses having specific permissions on the asset.&#x20;

### Asset Holders

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.19.22 PM.png>)

_Work in progress_

The asset holder list, although no names are attached to the addresses, shows which accounts hold a larger portion of the asset itself.

### Transactions

![A list of transactions that perform administrative actions on the asset](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.19.35 PM.png>)

![A list of transactions that perform transfer operations associated with the asset](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.20.07 PM.png>)

Two sets of transaction lists show administrative actions performed on the asset, and transfer operations that pertain to the movement of the asset. These can be useful to show what types of operations are commonly performed around the asset.



