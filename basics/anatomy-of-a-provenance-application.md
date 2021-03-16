---
description: >-
  Provenance is an ecosystem for application-specific financial services
  blockchain implementations.
---

# Anatomy of a Provenance Application

### Section Setup

Follow along with this section by cloning and running a Provenance node using Docker:

```text
$ git clone https://github.com/provenance-io/provenance.git
$ cd provenance
$ git checkout tags/v0.2.1 -b v0.2.1
$ make clean && make install
$ 
```

We get into more detail than introductory and discuss transaction flow in for our 3 major use cases \(we don't have to use Figure apps, just here for easy understanding/clarification\): 

* Figure Pay \(b2c transactions, all settlement, no Assets or value exchange\)
* Figure Lending \(Asset genesis, pooling, ownership exchange\)
* Figure Marketplace \(exchange, bilateral exchange\)

Thus, this section describes the anatomy of each of these types of apps.

 **A little deeper than introductory** 

