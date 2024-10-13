# Pragma Oracle

Pragma is the decentralized, transparent and composable oracle network,
leveraging state-of-the-art zero-knowledge cryptography. We partner with the
biggest market makers and the most liquid exchanges who sign and timestamp their
own high quality, robust data and send it directly on-chain.

Our data feeds are live on 🥕 Kakarot, where they leverage the power of the
ZK-EVM. Our code has been audited by leading security researchers at Nethermind
and we have more audits coming up soon.

We are currently deployed on the following addresses:

| Network                  | PragmaCaller Solidity Contract                                                                                                   | Native Starknet Address (ignore if you're an EVM developer)       |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Kakarot Starknet Sepolia | [0x5a3d161e5c63511F97F51fbF366B8238Cd0bBeAc](https://sepolia.kakarotscan.org/address/0x5a3d161e5c63511F97F51fbF366B8238Cd0bBeAc) | 0x36031daa264c24520b11d93af622c848b2499b66b41d611bac95e13cfca131a |
| Kakarot Mainnet          | **⏳ Soon**                                                                                                                      | **⏳ Soon**                                                       |

## Consuming our Data Feeds

You can get started with Pragma in just a few minutes. This guide will walk you
through the process of consuming data from Pragma's oracle network.

To use our Price feeds, you have two choices:

### Use our port of the `ChainlinkAggregatorV3` interface

If you already have an existing process working with Chainlink data feeds, you
can use our `PragmaAggregatorV3` interfaces (under the hood, it calls the
`PragmaCaller`).

The list of supported assets is:

| Ticker     | Pair ID                  | Decimals | Kakarot Starknet Sepolia                                                                                                         | Kakarot Mainnet |
| ---------- | ------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| BTC/USD    | 18669995996566340        | 8        | [0x330ec0B08B74a4F34Fd76B0917A55169885624Be](https://sepolia.kakarotscan.org/address/0x330ec0B08B74a4F34Fd76B0917A55169885624Be) | **⏳ Soon**     |
| ETH/USD    | 19514442401534788        | 8        | [0xcD025F607AdB9542B77C69A29B7b9Aa32Bf06811](https://sepolia.kakarotscan.org/address/0xcD025F607AdB9542B77C69A29B7b9Aa32Bf06811) | **⏳ Soon**     |
| WBTC/USD   | 6287680677296296772      | 8        | [0x4604A5b10818638F751829A580362eD5a42b9E5E](https://sepolia.kakarotscan.org/address/0x4604A5b10818638F751829A580362eD5a42b9E5E) | **⏳ Soon**     |
| USDC/USD   | 6148332971638477636      | 6        | [0xa3C78F0fd24523d1D5A70e47086343A445976911](https://sepolia.kakarotscan.org/address/0xa3C78F0fd24523d1D5A70e47086343A445976911) | **⏳ Soon**     |
| USDT/USD   | 6148333044652921668      | 6        | [0x52880cAe955C88546134e7394B4305c2fA79faB8](https://sepolia.kakarotscan.org/address/0x52880cAe955C88546134e7394B4305c2fA79faB8) | **⏳ Soon**     |
| WSTETH/USD | 412383036120118613857092 | 8        | [0x5Ad588FDF6a8851c4098fC3aec535f0A66316C0F](https://sepolia.kakarotscan.org/address/0x5Ad588FDF6a8851c4098fC3aec535f0A66316C0F) | **⏳ Soon**     |
| STRK/USD   | 6004514686061859652      | 8        | [0x7225f3743668B2751075cd53D25B449ca9967718](https://sepolia.kakarotscan.org/address/0x7225f3743668B2751075cd53D25B449ca9967718) | **⏳ Soon**     |
| UNI/USD    | 24011449254105924        | 8        | [0x858FA2FacF63A3e529cAb4F5a02ceaFb590db2c1](https://sepolia.kakarotscan.org/address/0x858FA2FacF63A3e529cAb4F5a02ceaFb590db2c1) | **⏳ Soon**     |
| EKUBO/USD  | 1278253658919688033092   | 8        | [0xa07CC3ff0557fD0F01FFb004469103353D6126F4](https://sepolia.kakarotscan.org/address/0xa07CC3ff0557fD0F01FFb004469103353D6126F4) | **⏳ Soon**     |
| LORDS/USD  | 1407668255603079598916   | 8        | [0xac0e304dA34912C9eD1d29a212feC74B36Eea4d6](https://sepolia.kakarotscan.org/address/0xac0e304dA34912C9eD1d29a212feC74B36Eea4d6) | **⏳ Soon**     |

#### 1. Define the PragmaAggregator interface

```typescript
interface IPragmaAggregatorV3 {
    function latestRoundData() external view returns (
        uint80 roundId,
        int256 answer,
        uint256 startedAt,
        uint256 updatedAt,
        uint80 answeredInRound
    );
}
```

#### 2. Retrieve the BTC/USD Spot Median Price

```typescript
contract PragmaDataConsumer {
    IPragmaAggregatorV3 internal dataFeed;

    constructor() {
        dataFeed = IPragmaAggregatorV3(
            0x330ec0B08B74a4F34Fd76B0917A55169885624Be // BTC/USD Pragma Interface
        );
    }

    function getPragmaDataFeedLatestPrice() public view returns (int256) {
        (
            /* uint80 roundID */,
            int256 price,
            /*uint256 startedAt*/,
            /*uint256 timeStamp*/,
            /*uint80 answeredInRound*/
        ) = dataFeed.latestRoundData();
        return price;
    }
}
```

### Directly call the `PragmaCaller`

The Pragma Oracle cairo contract has been integrated by Kakarot - thus it is
possible to call it directly in your Solidity contracts using the
[PragmaCaller](https://github.com/kkrt-labs/kakarot/blob/main/solidity_contracts%2Fsrc%2FCairoPrecompiles%2FPragmaCaller.sol)
interface!

The complete list of
[supported assets](https://docs.pragma.build/Resources/Starknet/data-feeds/supported-assets)
can be found in our documentation: just grab the pair id and you're good to go!

#### 1. Define the PragmaCaller interface

```typescript
interface IPragmaCaller {
    enum DataType {
        SpotEntry,
        FuturesEntry,
        GenericEntry
    }

    struct DataRequest {
        DataType dataType;
        uint256 pairId;
        uint256 expirationTimestamp;
    }

    struct PragmaPricesResponse {
        uint256 price;
        uint256 decimals;
        uint256 last_updated_timestamp;
        uint256 num_sources_aggregated;
        uint256 maybe_expiration_timestamp;
    }

    function getDataMedianSpot(DataRequest memory request) external view returns (PragmaPricesResponse memory);
}
```

#### 2. Retrieve the BTC/USD Spot Median Price

```typescript
interface IPragmaCaller {
    // ... (include the interface definition from step 1)
}

contract CallerExample {
    IPragmaCaller private pragmaCaller;
    // ascii conversion of "BTC/USD";
    uint256 constant BTC_USD_FEED = 18669995996566340;

    constructor(address pragmaCallerAddress) {
        pragmaCaller = IPragmaCaller(pragmaCallerAddress);
    }

    function getDataMedianSpot(uint256 pairId) public view returns (IPragmaCaller.PragmaPricesResponse memory) {
        IPragmaCaller.DataRequest memory request = IPragmaCaller.DataRequest(
            IPragmaCaller.DataType.SpotEntry,
            pairId,
            0
        );
        return pragmaCaller.getDataMedianSpot(request);
    }

    function getBtcMedianPrice() public view returns (uint256) {
        IPragmaCaller.PragmaPricesResponse memory response = getDataMedianSpot(BTC_USD_FEED);
        return response.price;
    }
}
```

An example has been deployed
[here](https://sepolia.kakarotscan.org/address/0x3899D87a02eFaB864C9306DCd2EDe06B90f28B14).

### Learn more

If you'd like to learn more about Pragma or use more advanced oracle functions
please check our [documentation](https://docs.pragma.build/)!
