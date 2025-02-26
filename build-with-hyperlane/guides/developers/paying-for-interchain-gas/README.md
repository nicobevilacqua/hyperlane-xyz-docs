# Paying For Interchain Gas

{% hint style="info" %}
Changes have been made to how gas payments work and a new set of IGP contract addresses have been released as of January 25, 2023. The latest information is reflected here. See [Migration to Enforced Interchain Gas Payments](../../paying-for-interchain-gas/migrating-to-enforced-interchain-gas-payments.md) for migration details and future plans.
{% endhint %}

The lifecycle of a Hyperlane message involves two transactions: one on the origin chain to send the message, and one on the destination chain to deliver the message.

For convenience, Hyperlane provides an on-chain API on the origin chain that can be used to pay [relayers](../../../../protocol/agents/relayer.md) in the origin chain's native token to deliver messages on the destination chain. This payment is called an Interchain Gas Payment (IGP).

<!-- INCLUDE diagrams/interchain-gas.md -->
<!-- WARNING: copied from the included file path. Do not edit directly. -->
```mermaid
%%{ init: {
  "theme": "neutral",
  "themeVariables": {
    "mainBkg": "#025AA1",
    "textColor": "white",
    "clusterBkg": "white"
  },
  "themeCSS": ".edgeLabel { color: black }"
}}%%

flowchart TB
    subgraph Origin
      Sender
      M_O[(Mailbox)]
      IGP[InterchainGasPaymaster]
      Sender -- "id = dispatch()" --> M_O
      Sender -- "payForGas(id)\n{value: payment}" --> IGP
    end

    Relayer((Relayer))

    M_O -. "indexing" .-> Relayer
    IGP -. "paymaster" .- Relayer

    subgraph Destination
      M_D[(Mailbox)]
    end

    Relayer -- "process()" --> M_D

    style Sender fill:#efab17

    click IGP https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/solidity/contracts/igps/InterchainGasPaymaster.sol
```

<!-- WARNING: copied from the included file path. Do not edit directly. -->
<!-- END -->