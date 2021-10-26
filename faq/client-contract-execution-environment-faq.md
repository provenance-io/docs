# Client Contract Execution Environment \(P8e\) FAQ

## What is the Client Contract Execution Environment or CCEE or P8e? <a id="what-is-the-client-contract-execution-environment-or-ccee-or-p-8-e"></a>

The Client Contract Environment \(CCEE or P8e\) is a framework that is part of the Provenance Blockchain ecosystem. The CCEE includes components that facilitate the management of contracts and its associated documents both on-chain and off-chain. One component deals with the secure communication between business partners to exchange information required to fulfill the contract. This communication flows through an asynchronous mail-box system where all messages are authenticated and encrypted by the two parties’ keys.

In addition, the CCEE includes facilities to easily refer to documents by their hash-id, and to encrypt and store those docs in an Encrypted Object Store \(EOS\). Those encrypted documents are indexed by their hash-id and optionally by selected document attribute values. The latter allows one to aggregate documents based on common attributes without revealing their sensitive content.

## What does P8e stand for?

P8e is the nickname for Provenance Blockchain’s Client Contract Execution Environment. The abbreviation comes from the word "Provenance", which starts with a “P”, then skips 8 letters and ends with “e” - “P8e”.

## What is a hash-id <a id="what-does-p-8-e-stand-for"></a>

When external documents are referred to by the cryptographic hash of their contents in on-chain, immutable transactions, it extends the immutability to that off-chain information. The hash value of a document’s content is used as a globally unique identifier \(URI\) or name \(URN\) for that content and is typified by ‘hash-id’.

## What is the EOS or Encrypted Object Store?

For many financial applications, many of the relevant documents, data and contracts are confidential and private. To facilitate that requirement, the Provenance Blockchain ecosystem includes services that make it easy to refer to confidential documents by its hash-id, and encrypt and store those documents in a database where they are indexed by their hash-id. The encryption-key is specific for the owner or client of that document. When a contract requires the exchange of such a document, the service will re-encrypt the document with a key associated with the business partner receiving that document.

