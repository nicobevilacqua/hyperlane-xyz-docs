# Migration to Enforced Interchain Gas Payments

{% hint style="info" %}
Keep up to date with Discord announcements to stay in the loop as the migration plan progresses.
{% endhint %}

Previously, interchain gas payments were not enforced by the relayer that's operated by the core team. The migration process toward enforcing accurate interchain gas payments has begun, and it involves some changes that apps should be aware of.

**TLDR:**

* Interchain gas payments are starting to be enforced using on-chain fee quoting
* The old IGP contracts have been deprecated, and there are new IGP contracts that apps must move to
* See [Gas](broken-reference) for more details on how to correctly pay interchain gas

### Overview of Changes

Previously, a single `InterchainGasPaymaster` contract address was provided. This contract didn't do much-- it would accept any payment and emit an event indicating that a gas payment was made, but it didn't require specific payment amounts.

The best current source of information for paying for gas can be found in the [Gas](broken-reference) section.

#### On-chain fee quoting

The provided IGP contracts are moving to on-chain fee quoting (spec [here](https://github.com/hyperlane-xyz/hips/pull/3)). This involves `payForGas` calls specifying an amount of destination gas that will be used by their message, which is used to quote and enforce a required gas payment on the origin chain. If enough origin chain native tokens have not been paid to cover interchain gas costs, the `payForGas` call reverts. This provides apps and users confidence that their messages will be delivered as long as their transaction on the origin chain doesn't revert and sufficient payment as been made.

To begin with, the IGP contracts are only enforcing that _some_ payment has been made. In the future, the implementation of the IGP contracts will change such that token exchange rates and gas prices are considered to correctly quote interchain gas payments.

See [Paying the Correct Amount](broken-reference) to understand how this looks in practice.

#### IGP contract address changes

The previous IGP contract address has been deprecated. Instead, two different IGP contracts are taking its place: the `DefaultIsmInterchainGasPaymaster` ([mainnet addresses](addresses.md#defaultisminterchaingaspaymaster-read-here)), and the `InterchainGasPaymaster` ([mainnet addresses](addresses.md#interchaingaspaymaster-advanced-read-here)).

See [Which IGP To Use & Understanding Gas Amounts](broken-reference) to understand which IGP contract your app should be using. Apps should move over to the appropriate IGP contract as soon as possible.

### What's Next

The full migration toward enforced and accurate on-chain gas payments is happening in a few phases. The first phase just happened.

1. &#x20;**Phase 1: New IGP contracts that charge interchain gas payments on the origin chain** (Completed)
   1. At this point, new IGP contracts are provided for applications to start using. These addresses are not expected to change in the future. The IGP contracts quote a non-zero interchain gas payment that's enforced, but the quoted payments don't yet use destination token exchange rates or gas prices to quote an accurate payment. Gas amounts are also not strictly enforced by the relayer.
2. **Phase 2: Only messages that have been paid for via the new IGPs are relayed** (Up next)
   1. At this point, the relayer operated by the core team will only process messages that have made a corresponding `payForGas` call to one of the new IGP contracts. This requires applications to move over to the new IGP contracts.
3. **Phase 3: Gas amounts become enforced by the relayer.**
   1. At this point, the relayer operated by the core team will process messages that have specified the accurate amount of gas in the interchain gas payment.
4. **Phase 4: Quoting fully accurate interchain gas payments.**
   1. At this point, the IGP contracts will use on-chain token exchange rates and destination chain gas prices to provide accurate quotes for messages.

### FAQ

**Are there any changes to the \`IInterchainGasPaymaster\` interface?**

No. The interface is exactly the same, just the IGP contract addresses have changed.

#### How do I know how much gas to use as the \`\_gasAmount\` parameter when using the IGP?

See [Which IGP To Use & Understanding Gas Amounts](broken-reference) to understand how this may differ if you're using the default Interchain Security Module.

Generally, this would involve estimating an upper bound amount of gas that your messages use on the destination.

**When is Phase 2 going into effect?**

We'll start enforcing that the new IGPs are used for gas payments within about a week. Expect an announcement on Discord.

**Is this relevant for V1 or V2?**

This is only relevant for V2.

**What can I expect from future changes to IGPs?**

Future changes, like accurately quoted interchain gas payments using token exchange rates and destination chain gas prices, won't involve new IGP contract addresses to switch over to. The new provided IGP contracts are upgradeable, so as long as you migrate now to the new IGP contracts and start [paying for interchain gas](broken-reference) based off what the IGP contract quotes, no future action is required.

**How do I pay for gas with middlewares, like Interchain Accounts, Interchain Queries, and the Liquidity Layer?**

All of these "middlewares" return the message ID that's dispatched, which can&#x20;

**This may break some of my existing mainnet contracts that are live on v2.**

While this is mostly backward compatible due to the IGP interface being the exact same, we understand these changes may still pose issues. Please reach out to us either directly or on Discord!
