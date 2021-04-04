# Reapers

P8e API leverages many background tasks needed to assist in the execution of a contract and handle feedbacks provided from the blockchain after memorialization.



#### MissingAckReaper

For any connected affiliates, this reaper checks for events with a **`ENVELOPE_RESPONSE`** status with a index time recorded for the envelope record but has not been set to a **`COMPLETED`** state. The reaper will then recreate an event for that envelope record so that it reaches the **`COMPLETED`** state.

#### MailboxReaper

The **MailboxReaper** is designed for multiparty contract execution and is implemented using a polling mechanism to continuously poll the mailbox for any inbound envelopes to be processed.

#### ErrorReaper

Polls from an event stream, similar to the **ScopeFetchReaper,** to retrieve error events back from the blockchain, for this particular reaper it is specifically looking for a **`TxError`** that will get recorded.

#### ScopeFetchReaper

Polls from an event stream, similar to the **ErrorReaper,** to retrieve scope events back from the blockchain, to be recorded into the database and indexed data store. 

#### HeartbeatReaper

Check and maintains the connection health status of the affiliate connected to the service. 

