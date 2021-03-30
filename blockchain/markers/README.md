# Markers

## Creating a New Coin

{% hint style="info" %}
`<coin>` is in the format of `<supply/denom>` or `1000hash`
{% endhint %}

```text
provenanced -t --chain-id pio-testnet-1 tx marker new 100000lrc --from stakeholder1 --gas auto --gas-adjustment 1.3 --fees 3000nhash
```

