# P8e UI

P8e includes a web UI for managing contract execution keys and monitoring/inspecting your contracts within the system.

## Deployment

This UI may be either run locally via docker, or hosted alongside your p8e environment.

### Running Locally

With [https://www.docker.com/products/docker-desktop\[docker](https://www.docker.com/products/docker-desktop[docker) installed\], you can run the following command to pull down and run the latest ui image \(replace the P8E\_URL value with the appropriate p8e webservice url for the instance you are using\). Additionally, you can specify the port for the ui to run on locally by changing the `3000` in the following command to your desired port.

```bash
docker run -p 3000:80 --env P8E_URL='http://my-p8e-webservice-url' us.gcr.io/figure-production/p8e-ui:latest
```

Then you can access the UI at [http://localhost:3000](http://localhost:3000) \(where 3000 is your designated port\).

### Running alongside hosted p8e

The same container specified above to run locally can be deployed and traffic routed to port 80. If no P8E\_URL environment variable is specified to the container, then it will be assumed that the p8e webservice can be found at `<base_url>/api/p8e`, where `<base_url>` is the base url of the hosted ui.

## Features

### Login

Logging into the UI is accomplished via your Provenance Blockchain account. Upon your first login to the p8e ui, you will be prompted to allow sharing data with p8e.

### Key Management

#### Contract Keys

You can add one or more contract keys for use in executing contracts. Each key can be given a human-readable alias for convenience and clarity. Additionally, when adding a key you will have to add an index name, which is the name of the index to associated this data within ElasticSearch. You may add a key already in use within p8e by pasting in the private key, which will then simply link the key with your identity \(without altering the existing alias and index name\).

Keys that are registered in the system can be set up to share data with one or more public keys. To access this functionality, simply click on the 'Manage' button under a key on the Key Management page and then click 'Add New Share' and enter the desired public key to share data with.

Until you add at least one contract key, no data will be visible in the ui. Data displayed within the ui is limited to what is accessible to your various keys.

### Dashboard

There is a dashboard view to give an overview of the state of the most recent 100 contracts in the system associated with one of your Contract Keys. The charts on the dashboard illustrate how long contracts are taking to complete, the current distribution of the state of these contracts and the timing breakdown of how long contracts are spending in each state. The 'Complete Contract Execution Time' and 'Contract Time Breakdown' graphs also support zooming in by selecting a subset of the data, which can be useful if there are outliers in the data compressing the scale.

### Contract Details

The 'Contracts' section of the ui displays more details on the latest contracts in the system associated with one of your Contract Keys. Here you can see the type of contract, timing details, status, any errors, and facts/prerequisites \(conditions\)/considerations \(functions\). This section can be useful for determining why a contract may have errored, as well as for viewing the pii data stored securely within the p8e object store. In order to view data, simply click on the line with the name and hash of the data you wish to view and a modal will display the desired data.

### Scope Details

The 'Scope' section of the ui displays the current and historical state of scopes. You can either search for a scope by uuid, or click on the scope uuid link from a contract that acted upon a scope. When viewing a scope, you can just click on any fact/prerequisite \(condition\)/consideration \(function\) input or output and a modal will display the desired data. At the bottom of the scope details page is a listing of the historical state of that scope, which you can click on and view in another page.

