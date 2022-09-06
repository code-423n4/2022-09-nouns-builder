# Nouns Builder Contest Details

- $85,500 USDC main award pot
- $4,500 USDC gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-09-nouns-builder-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts September 06, 2022 20:00 UTC
- Ends September 15, 2022 20:00 UTC

# Overview

Nouns DAO is a significant form factor innovation for NFTs and DAOs. Instead of giving all power and capital solely to a project’s founders, the Nouns model enables communities to emerge progressively and be active participants in how their brand and community evolves.

Nouns Builder is a tool for deploying forks of Nouns DAO with custom NFT, auction, governance, and founder(s) settings.

See the [documentation](https://github.com/code-423n4/2022-09-nouns-builder/blob/main/docs/protocol-docs.md) for more information on the protocol.

# Scope

This contest is open for 9 days to give wardens time to understand the protocol properly. Representatives from ZORA will be available in the Code Arena Discord to answer any questions during the contest period.

The focus of the contest is to try and find any logic errors or ways to drain funds from a DAO in a way that is advantageous for an attacker at the expense of users with funds invested in the protocol. Wardens should assume that governance variables are set sensibly (unless they can find a way to change the value of a governance variable, and not counting social engineering approaches for this).

All contracts under `src` are to be reviewed. Any other contracts are to be ignored for this contest.

```ml
manager
├─ Manager — "DAO deployer and upgrade registry"
├─ IManager — "Interface for Manager contract"
├─ storage
│  ├─ ManagerStorageV1 — "Storage for Manager contract"
token
├─ Token — "ERC-721 Governance Token implementation"
├─ IToken — "Interface for Token contract"
├─ storage
│  ├─ TokenStorageV1 — "Storage for Token contract"
├─ types
│  ├─ TokenTypesV1 — "Custom data types for Token contract"
├─ metadata
│  ├─ MetadataRenderer — "ERC-721 metadata renderer implementation"
│  ├─ storage
│  │    ├─ MetadataRendererStorageV1 - "Storage for Metadata Renderer contract"
│  ├─ types
│  │    ├─ MetadataRendererTypesV1 - "Custom data types for Metadata Renderer contract"
│  ├─ interfaces
│  │    ├─ IBaseMetadata - "Shared interface for metadata renderers"
│  │    ├─ IPropertyIPFSMetadataRenderer - "Interface for rendering artwork stored on IPFS"
auction
├─ Auction — "ERC-721 Auction implementation"
├─ IAuction — "Interface for Auction contract"
├─ storage
│  ├─ AuctionStorageV1 — "Storage for Auction contract"
├─ types
│  ├─ AuctionTypesV1 — "Custom data types for Auction contract"
governance
├─ governor
│  ├─ Governor — "DAO proposal manager and transaction scheduler"
│  ├─ IGovernor — "Interface for Governor contract"
│  ├─ storage
│  │  ├─ GovernorStorageV1 — "Storage for Governor contract"
│  ├─ types
│  │  ├─ GovernorTypesV1 — "Custom data types for Governor contract"
├─ treasury
│  ├─ Treasury — "DAO treasury and transaction executor"
│  ├─ ITreasury — "Interface for Treasury contract"
│  ├─ storage
│  │  ├─ TreasuryStorageV1 — "Storage for Treasury contract"
│  ├─ types
│  │  ├─ TreasuryTypesV1 — "Custom data types for Treasury contract"
lib
├─ proxy
│  ├─ ERC1967Upgrade - "Modern, minimal proxy upgrade"
│  ├─ ERC1967Proxy - "Minimal ERC1967 proxy"
│  ├─ UUPS - "Minimal UUPS support"
├─ token
│  ├─ ERC721 - "Modern, minimal ERC721 implementation"
│  ├─ ERC721Votes - "Modern support for NFT voting and delegation"
```

# Setup

Install Dependencies:

```bash
yarn
```

```bash
forge install
```

Run Tests:

```bash
forge test
```

Run Tests with Modifiers:

- (-vv): Logs emitted during tests are also displayed.
- (-vvv): Stack traces for failing tests are also displayed.
- (-vvvv): Stack traces for all tests are displayed, and setup traces for failing tests are displayed.
- (-vvvvv): Stack traces and setup traces are always displayed.

```bash
forge test  -vvvv
```
