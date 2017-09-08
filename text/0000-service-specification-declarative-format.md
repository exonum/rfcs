- Feature Name: service-specification-declarative-format
- Start Date: 2017-08-16
- RFC PR: (leave this empty)
- Exonum Issue: (leave this empty)

# Summary
[summary]: #summary

Propose a declarative format based on [YAML](yaml). This format allows to reduce
the effort required to create custom services.

# Motivation
[motivation]: #motivation

A significant amount of template code, which can be generated automatically, is
needed to create the service (see [Cryptocurrency Tutorial: How to Create Services](create-service)).
This RFC propose a human-readable format for specifying the parameters for
generating such code.

# Detailed design
[design]: #detailed-design

## General description

In order to create a service one should describe following parameters:

- [Configuration](configuration)
- [Transactions](transactions)
- [Storage](storage)
- [API](api)

### configuration

Configuration parameters used by the service.

#### configuration.public

Configuration parameters to be stored in the blockchain.

#### configuration.private

Configuration parameters to be stored locally.

### types

Defines transaction messages and structures with their fields to be stored  with
their fields.

### transactions

Transaction-related code (including corresponding transaction structures and
templates of `verify` and `execute` methods) must be generated according to
parameters described in this section.

#### Transaction Structures

Transaction messages with their fields (including type) must be
defined in the `types` section.

#### Verification and Execution

Templates for `verify` and `execute` methods for each transaction type must be
generated. User should fill in bodies of that methods to implement transaction
logic.

### storage

DB-related code (including corresponding structures, tables, and method to obtain
a separate table element) must be generated according to parameters described in
this section.

Information about database structure must be available to light clients via
blockchain.

Methods for modifying the stored data should be implemented by user.

#### Structures

Structures to be stored with their fields (including type) must be defined in
the `types` section.

#### Tables

Defines tables (with table types and types of table contents) combining the
aforementioned structures.

### api

API-related code (including corresponding data types and `wire` method) must be
generated according to parameters described in this section.

User may manually implement any other types of endpoints and add their handling
into `wire` method.

#### Storage-related

For getting each item type described in the [Tables](tables) section a separate
endpoint must be created.

#### Health-check

According to [design by contract](wiki:dbc) approach user may define endpoints
for invariants checking (e.g. amount of money on all accounts in the system).

## ID Autogeneration

Service IDs, table IDs, and transaction IDs must be generated automatically.

## Documentation

Documentation comments must be generated along with the corresponding code.

## Example

This example is based on **Cryptocurrency Tutorial: How to Create Services**.

```YAML
configuration: {}

types:
  TxCreateWalletRequest:
    type: struct
    doc: |
       `TxCreateWalletRequest` is used in `TxCreateWallet` transactions
    fields:
    - name: pub_key
      type: PublicKey
      doc: Owner's key
    - name: name
      type: string
      doc: Owner's name
  TxTransferRequest:
    type: struct
    doc: |
       `TxTranferRequest` is used in `TxTransfer` transactions
    fields:
    - name: from
      type: PublicKey
      doc: Originator of the transaction
    - name: to
      type: PublicKey
      doc: Recipient of funds
    - name: amount
      type: u64
      doc: The amount of funds transferred in the transaction
    - name: seed
      type: u64
      doc: >
        Additional transaction ID to discriminate simultaneous transactions
        with equal amounts
  Wallet:
    type: struct
    doc: |
       `Wallet` is used to save data in the storage
    fields:
      - name: pub_key
        type: PublicKey
        doc: Owner's key
      - name: name
        type: string
        doc: Owner's name
      - name: balance
        type: u64
        doc: Wallet balance

transactions:
  TxCreateWallet:
    messageId: 0
    transport:
    - type: http+json
      method: post
      endpoint: "{{basePath}}/v1/create"
    requestType: TxCreateWalletRequest
    doc: Creates new account
  TxTransfer:
    messageId: 1
    transport:
    - type: http+json
      method: post
      endpoint: "{{basePath}}/v1/transfer"
    requestType: TxTransferRequest
    doc: Transfers coins from one account to another

storage:
  Wallets:
    type: MapIndex
    key_type: PublicKey
    value_type: Wallet

api: {}
```

# How We Teach This
[how-we-teach-this]: #how-we-teach-this

**Cryptocurrency Tutorial: How to Create Services** should be corrected taking
into account this RFC.

# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

# Alternatives
[alternatives]: #alternatives

JSON/YAML/TOML use the same underlying data model and are interchangeable as
serialization format.

Alternative IDLs:

- **OpenAPI**  
  Doesn't support generics. This makes describing table structures problematic.
- **WSDL**  
  An XML-based IDL so it is too verbose and not human readable.

# Unresolved questions
[unresolved]: #unresolved-questions

Functionality corresponding to verifying cryptographic proofs and performing
transactions is not covered by this RFC and must be implemented by the service
creator.

[create-service]: https://github.com/exonum/exonum-doc/blob/master/src/get-started/create-service.md
[yaml]: http://yaml.org/
[wiki:dbc]: https://en.wikipedia.org/wiki/Design_by_contract
