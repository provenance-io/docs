# Configure a Sentry

## Sentry Nodes \(Public and Private\)

To best protect a validator node, the node should be on its own network front-ended by nodes called sentries. This is done to prevent network attacks\(DDoS, brute force, etc.\) from being launched directly against a validator network. A sentry node is simply a full node that connects to the Provenance Blockchain blockchain and relays the chain to the validator. This ensures that the validator network information remains hidden and is not accessible over the public internet.

For the Provenance Blockchain network, two sets of sentry nodes are recommended. Both are essentially simple full nodes with some additional configuration. It is recommended that at least two of each type of sentry node be created to ensure high availability. Creating these nodes in separate regions/data centers is also recommended.

### **Public Sentry Nodes**

Public sentry nodes are accessible over the public internet and allow others to leverage nodes to connect to the Provenance Blockchain blockchain. The access should be limited to the p2p port of 26656 but allows any IP to connect. These nodes are recommended to have additional endpoints to connect to and strengthen the Provenance Blockchain blockchain. The public sentry will act as a sort of protection for the private sentry nodes. It will require that each private sentry node id be added to the **private\_peer**_**\_**_**ids** in the config.toml file to avoid them being gossiped about and found on the network. Additional recommended configurations are listed below.

### **Private Sentry Nodes**

Private sentry nodes are the last line of network protection for the validator nodes. They will connect to the public sentries to continue to relay the Provenance Blockchain network, but additionally may be used to connect to specific partners. As an example, if you would like to partner with Company X, you could allow them to connect directly to this layer and whitelist their access via firewall rules. These nodes additionally would require you to add the validator node ids to the  **private\_peer**_**\_**_**ids** in the config.toml file. This will ensure they will not be gossiped about on the network. Additional recommended configurations are listed below.

### **Node Configuration**

The following configuration parameters are found in the config.toml file

<table>
  <thead>
    <tr>
      <th style="text-align:left">Configuration</th>
      <th style="text-align:left">Setting</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">pex</td>
      <td style="text-align:left">
        <p>true (public sentry), false (private sentry)</p>
        <ul>
          <li>This turns the peer exchange reactor on or off for a node.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">persistent-peers</td>
      <td style="text-align:left">
        <p>validator node ids, other sentry node ids</p>
        <ul>
          <li>a comma separated list of <code>nodeID@ip:port</code> values that define
            a list of peers that are expected to be online at all times</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">private-peer-ids</td>
      <td style="text-align:left">
        <p>private sentry node ids, validator node ids</p>
        <ul>
          <li>Comma separated list of peer IDs to keep private (will not be gossiped
            to other peers)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">unconditional-peer-ids</td>
      <td style="text-align:left">
        <p>sentry node ids</p>
        <ul>
          <li>List of node IDs, to which a connection will be (re)established ignoring
            any existing limits</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">addr-book-strict</td>
      <td style="text-align:left">
        <p>true (public sentry), false (private sentry)</p>
        <ul>
          <li>By default nodes with a routable address will be considered for connection.
            If this setting is turned off (false), non-routable IP addresses, like
            addresses in a private network can be added to the address book.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

### Recommended Hardware Configuration

{% hint style="info" %}
CPU/Memory/Storage is determined based on how you intend to use Provenance Blockchain and how the node is configured \(type\) and data retention periods. These are general use numbers and may be adjusted based on the desired performance. 
{% endhint %}

| Node Type | CPU | Memory | Storage |
| :--- | :--- | :--- | :--- |
| Public Sentry/Private Sentry | 4 vCPU | 8GB | 500GB |

## 

