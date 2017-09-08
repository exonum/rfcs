- Feature Name: service-specification-declarative-format
- Start Date: 2017-08-16
- RFC PR: (leave this empty)
- Exonum Issue: (leave this empty)

# Summary
[summary]: #summary

Propose a declarative format based on [TOML](toml). This format allows to reduce
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

### [configuration]

Configuration parameters used by the service.

#### [configuration.public]

Configuration parameters to be stored in the blockchain.

#### [configuration.private]

Configuration parameters to be stored locally.

### [transactions]

Transaction-related code (including corresponding transaction structures and
templates of `verify` and `execute` methods) must be generated according to
parameters described in this section.

#### Transaction Structures

Defines transaction messages with their fields (including type and size).

#### Verification and Execution

Templates for `verify` and `execute` methods for each transaction type must be
generated. User should fill in bodies of that methods to implement transaction
logic.

### [storage]

DB-related code (including corresponding structures, tables, and method to obtain
a separate table element) must be generated according to parameters described in
this section.

Information about database structure must be available to light clients via
blockchain.

Methods for modifying the stored data should be implemented by user.

#### [storage.structures]

Define structures with their fields (including type and size) to be stored.

#### [storage.tables]

Defines tables (with table types and types of table contents) combining the
aforementioned structures.

### [api]

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

```toml
[configuration]

[transactions]

[transactions.TxCreateWallet.pub_key]
field_type = "&PublicKey"
field_size = 32
[transactions.TxCreateWallet.name]
field_type = "&str"
field_size = 8

[transactions.TxTransfer.from]
field_type = "&PublicKey"
field_size = 32
[transactions.TxTransfer.to]
field_type = "&PublicKey"
field_size = 32
[transactions.TxTransfer.amount]
field_type = "u64"
field_size = 8
[transactions.TxTransfer.seed]
field_type = "u64"
field_size = 8

[storage]

[storage.structures.Wallet.pub_key]
field_type = "&PublicKey"
field_size = 32
[storage.structures.Wallet.name]
field_type = "&str"
field_size = 8
[storage.structures.Wallet.balance]
field_type = "u64"
field_size = 8

[[storage.tables]]
table_type = "MapIndex"
key_type = "PublicKey"
value_type = "Wallet"

[api]

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

JSON or JSON can be considered as an alternative to TOML (or can be supported in
parallel with TOML).

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
[toml]: https://github.com/toml-lang/toml
[wiki:dbc]: https://en.wikipedia.org/wiki/Design_by_contract
