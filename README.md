[![CircleCI](https://circleci.com/gh/hiddentao/ethgoal/tree/master.svg?style=svg)](https://circleci.com/gh/hiddentao/ethgoal/tree/master) [![Coverage Status](https://coveralls.io/repos/github/hiddentao/ethgoal/badge.svg?branch=master)](https://coveralls.io/github/hiddentao/ethgoal?branch=master)

# Ethgoal

Achieve goals with your friends' help, powered by the Ethereum blockchain.

**NOTE: THESE CONTRACTS HAVE NOT YET BEEN AUDITED, USE AT YOUR OWN RISK!**

## How it works

1. Submit a "pledge" to the blockchain along with digital signatures of between 1 and 3 friends who agree to judge whether you've
achieved your pledge by a given end date. You submit a deposit along with your pledge. This is denominated in DAI
and can be any amount of your choosing.

2. After the the end date has passed your friends can vote to say you've passed or failed. They will by default 1 week to
cast their votes - this is known as the _"judgement period"_.

3. If the majority vote to say that you failed to meet your pledge then your deposit gets split between them and you get nothing back.
If they vote to say that you've passed or they fail to vote then it is assumed that you passed and you thus get your deposit back.

4. You can then withdraw the DAI due to you at any time after the pledge judgement period has passed.

You can have any number of pledges open at a time.

_Note: on non-mainnet networks we deploy `MintableToken`, a simple ERC-20 token which allows for unlimited minting._

## Chai integration

On mainnet, when you submit your deposit it gets deposited in a `Bank` contract which will actually send it to
[Chai](https://chai.money) to earn interest - this way we don't need to charge the users any fees upfront!

_Note: on non-mainnet networks we deploy `DevChai`, a mock Chai implementation that simply mints more `MintableToken`s._

## Architecture

**Key contracts**

* `Controller.sol` _(non-upgradeable)_ - the main contract users interact with.
* `Bank.sol` _(upgradeable)_ - handles user token deposits and withdrawals and speaks to Chai.
* `Settings.sol` _(upgradeable)_ - all other contracts use this to discover each other.

**Upgradeability**

The upgradeability architecture is implemented in `Proxy.sol` and is based on
the [OpenZepellin eternal storage pattern](https://blog.openzeppelin.com/smart-contract-upgradeability-using-eternal-storage/).

However, an ability to prevent future upgrades has been added (see the `freezeImplementation()` method). This means that the
admins will be able to make the code truly immutable at any point in time once they are confident there are no more upgrades
needed.


## For devs

Run a devnet:

```shell
yarn devnet
```

Deploy contracts locally:

```shell
yarn deploy:local
```

Run tests:

```shell
yarn test
```

Run tests with coverage:

```shell
yarn coverage
```

## LICENSE

AGPLv3
