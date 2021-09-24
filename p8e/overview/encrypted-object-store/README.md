# Encrypted Object Store

P8e SDK encrypts all data sent to Encrypted Object Store \(EOS\) using ECIES and the DIME format. If all audience members on that request reside on the owner's EOS, the data is stored and is never replicated to any other affiliate's EOS. However, when there's audience members that are owned by a remote EOS, the data is replicated to those other affiliate environments. The DIME standard defines an encryption scheme so that if the data fell into the wrong hands, it could not be decrypted back into the plaintext version. In practice, even though this is safe to replicate over HTTP, mTLS is highly preferred. EOS itself is just a storage and replication mechanism for this data and it does not have the necessary keys in order to decrypt any of it itself.

