# Solidity Multicall Contract

Contract to aggregate multiple contract web3-rpc requests into single request chunks. Used inside https://github.com/farm-army/farm-army-backend and based on "eth-multicall"

- NPM Package: https://www.npmjs.com/package/eth-multicall

## Deployed Contracts

As the official package is not responsible to support custom chain, feel free to reuse the already deployed contracts: 

| Chain                         | Contract                                   |
| ----------------------------- | ------------------------------------------ |
| Binance Smart Chain (BSC)     | 0xB94858b0bB5437498F5453A16039337e5Fdc269C |
| Polygon (Matic)               | 0x13E5407E38860A743E025A8834934BEA0264A8c1 |
| Fantom                        | 0xe6cd57c490cdc698aa6df974b207c4c044818d5a |
| KuCoin Community Chain        | 0xae49c2836d060bD7F4ed6566626b52cF4991B172 |
| Harmony                       | 0xd1AE3C177E13ac82E667eeEdE2609C98c69FF684 |
| Celo                          | 0xfEEaa3989087F2c9eB4e920D57Df0A3F83486414 |
| Moonriver                     | 0x814C1C56815D52b58d7254424c15307e7363E016 |

## Usage 

### eth-multicall (web3)

For more information see: https://www.npmjs.com/package/eth-multicall

```javascript
const { MultiCall } = require("eth-multicall");
const Web3 = require("web3");
const Web3EthContract = require("web3-eth-contract");

const ERC20ABI = [];

const web3 = new Web3(new Web3.providers.HttpProvider('https://bsc-dataseed.binance.org/', {
    keepAlive: true,
    timeout: 10000,
}));

const addresses = [
    "0x55d398326f99059ff775485246999027b3197955", // USDT
    "0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56", // BUSD
];

const tokens = addresses.map(address => {
    const token = new Web3EthContract(ERC20ABI, address);
    return {
        symbol: token.methods.symbol(),
        decimals: token.methods.decimals(),
    };
});

// BSC Contract
const [result] = await new MultiCall(web3, '0xB94858b0bB5437498F5453A16039337e5Fdc269C').all([tokens]);

console.log(result);
```
