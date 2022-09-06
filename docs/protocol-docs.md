# Nouns Builder Documentation

Documentation for interacting with the Nouns Builder Protocol.

## Table of Contents

- [Architecture](#architecture)
- [Custom Settings](#custom-settings)
- [Deploy](#deploy)

## Architecture

![protocol](../assets/protocol.png)

## Custom Settings

DAOs are customized with the following settings declared in [IManager](https://github.com/code-423n4/2022-09-nouns-builder/blob/0915d87f82c85039bfd8de815b1ef9ccc49b9bda/src/manager/IManager.sol).

#### Founders

```solidity
/// @notice The founder parameters
/// @param wallet The wallet address
/// @param ownershipPct The percent ownership of the token supply
/// @param vestExpiry The timestamp that vesting expires
struct FounderParams {
  address wallet;
  uint256 ownershipPct;
  uint256 vestExpiry;
}

```

Similar to the [Nounder's reward](https://nouns.wtf/nounders), each founder can allocate themselves a percentage of the token supply up to a future timestamp. While compensation isn't required, at least one founder address must be provided upon deploy. This founder will be responsible for adding token artwork and launching the first auction. Additionally the founder will be granted the right to veto any malicious proposals threatening a 51% attack on the DAO while token supply is low. They can revoke this right at any time.

Note: if a DAO has multiple founders, the responsibilities will be given to the first one listed.

#### Token

```solidity
/// @notice The ERC-721 token parameters
/// @param initStrings The encoded token name, symbol, collection description, collection image uri, renderer base uri
struct TokenParams {
  bytes initStrings;
}

```

- The collection image uri (eg. [IPFS](https://bafybeicyerb22m6c57kcbibm56dgj6mi7axk3jlibfyxebdyvrofeu5tci.ipfs.dweb.link/0xbcdc9da9c893d3d2adc2daa09984af9195ef9f3e0ab338aff57a80c9e17fa8fd.webp)) gets encoded in the `contractURI` stored on the DAO's [Metadata Renderer]() contract
- The encoded `contractURI` and `tokenURI`s (generated upon mint) are decoded by the renderer base uri (eg. `api.zora.co/renderer/stack-images`)

#### Auction

```solidity
/// @notice The auction parameters
/// @param reservePrice The reserve price of each auction
/// @param duration The duration of each auction
struct AuctionParams {
  uint256 reservePrice;
  uint256 duration;
}

```

Deploy with [GDAs](https://www.paradigm.xyz/2022/04/gda) and [VRGDAs](https://www.paradigm.xyz/2022/08/vrgda) soon :eyes:

#### Governance

```solidity
/// @notice The governance parameters
/// @param timelockDelay The time delay to execute a queued transaction
/// @param votingDelay The time delay to vote on a created proposal
/// @param votingPeriod The time period to vote on a proposal
/// @param proposalThresholdBps The basis points of the token supply required to create a proposal
/// @param quorumThresholdBps The basis points of the token supply required to reach quorum
struct GovParams {
  uint256 timelockDelay;
  uint256 votingDelay;
  uint256 votingPeriod;
  uint256 proposalThresholdBps;
  uint256 quorumThresholdBps;
}

```

Proposal and quorum thresholds are specified as basis points due to the dynamic token supply.

## Deploy

```solidity
/// @notice Deploys a DAO with custom token, auction, and governance settings
/// @param founderParams The DAO founder(s)
/// @param tokenParams The ERC-721 token settings
/// @param auctionParams The auction settings
/// @param govParams The governance settings
function deploy(
  FounderParams[] calldata founderParams,
  TokenParams calldata tokenParams,
  AuctionParams calldata auctionParams,
  GovParams calldata govParams
)
  external
  returns (
    address token,
    address metadataRenderer,
    address auction,
    address treasury,
    address governor
  );

```

The `Manager` contract creates a DAO with provided settings.

Once deployed, the first (or only) founder is granted ownership of the DAO's `Token`, `Metadata Renderer`, and `Auction` contracts to add artwork, adjust any metadata or auction settings prior to launch, and eventually start the first auction.

As the owner, the founder can transfer this responsibility if they'd like.

Artwork is added via [`addProperties()`](https://github.com/code-423n4/2022-09-nouns-builder/blob/0915d87f82c85039bfd8de815b1ef9ccc49b9bda/src/token/metadata/MetadataRenderer.sol#L91-L95) on the `Metadata Renderer`. Ownership of the contract is automatically transferred to the `Treasury` upon execution.

Note: the protocol does not enforce artwork to be added, but any tokens minted will not have generated attributes.

The first auction is created via [`unpause()`](https://github.com/code-423n4/2022-09-nouns-builder/blob/0915d87f82c85039bfd8de815b1ef9ccc49b9bda/src/auction/Auction.sol#L241) on the `Auction` contract. Ownership will also automatically transfer to the `treasury`.

At this point, the DAO is launched and any further changes must be executed via governance. Happy bidding!
