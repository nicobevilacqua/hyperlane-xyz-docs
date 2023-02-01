---
description: Pioneering permissionless and modular interoperability between blockchains
---

# Hyperlane Introduction

Hyperlane is the permissionless and modular interoperability layer. Hyperlane empowers developers to build Interchain Applications, apps that can connect between any blockchain environment and offer users a unified experience.&#x20;

### **Hyperlane APIs**

Hyperlane's [APIs](./#hyperlane-apis) offer developers secure and simple ways to communicate between blockchains, and the Hyperlane [SDKs](broken-reference) makes building interchain applications quick and easy.&#x20;

[messaging-api](apis/messaging-api/ "mention") [token-bridge-api.md](apis/token-bridge-api.md "mention") [send.md](apis/send.md "mention")[query.md](apis/query.md "mention")[warp-api.md](apis/warp-api.md "mention")



```mermaid
flowchart LR
    
    subgraph oc[Origin Chain]
        sc(Sender Contract) 
        mb[\Mailbox/]
    end
    
    subgraph rc[Destination Chain]
        rec(Recipient Contract)
        mb2[/Mailbox\]
        ism(Interchain Security Module)
    end

    val{{"Validator(s)"}}
    aws[(Off-Chain<br>Highly-Available<br>Storage)]
    rel[/Relayer/]

    
    sc--"mailbox.dispatch()"-->mb
    mb--"latestCheckpoint()"-->val
    val--signed checkpoint-->aws
    mb-->rel
    aws-->rel
    rel--"mailbox.process()"-->mb2
    mb2--"recipient.handle()"-->rec
    mb2<--"ism.accept()"-->ism
    
    style val fill:#fcf,stroke:#929,stroke-width:3px
    style aws fill:#fc9,stroke:#f90,stroke-width:3px
    style rel fill:#e4f3ff,stroke:#025aa1,stroke-width:3px 
    style oc fill:#c8e6fe,stroke:#ddd,stroke-dasharray: 5 5,stroke-width:3px
    style rc fill:#c8e6fe,stroke:#ddd,stroke-dasharray: 5 5,stroke-width:3px
    style sc stroke-width:3px
    style rec stroke-width:3px
    style mb fill:#e4f3ff,stroke:#025aa1,stroke-width:3px
    style mb2 fill:#e4f3ff,stroke:#025aa1,stroke-width:3px
```

### Integrate Hyperlane into your app

Check out the [getting started guide](introduction/getting-started.md) for everything you need to start building with Hyperlane. If you run into an issues or have any questions, [join our discord](https://discord.gg/hyperlane) to get support from the community of Hyperlane builders!

### Learn more about the Hyperlane Protocol

Take a look at the [protocol docs](protocol/overview.md) to understand the Hyperlane protocol architecture and [security model](protocol/sovereign-consensus.md).

### Learn more about Hyperlane use cases. Continue to the [next page](introduction/why-hyperlane/) to learn more&#x20;
