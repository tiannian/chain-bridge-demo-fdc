# chain-bridge-demo-fdc

Reference project:

- [compound-finance/gateway](https://github.com/compound-finance/gateway)
- [frontier](https://github.com/paritytech/frontier)
- [substrate](https://substrate.dev/)
- [chainbridge](https://github.com/ChainSafe/ChainBridge)
- [Aurora](https://near.org/zh/blog/aurora-launches-near/)
- [SputnikVM](https://github.com/rust-blockchain/evm)

## Plan

Step 1 (Demo & MVP):

```
Ethereneum <-> Findora Defi Chain(Substrate based, ETH compact) <-> Findora
            ^                                                    ^
            |                                                    |
        ChainBridge                                         Bridge (need develop)
```

Step 2:

```
Ethereneum <-> Findora Defi Chain(Substrate based, ETH compact) <-> Findora
            ^                                                    ^
            |                                                    |
Crosschain(by substrate pallet and contracts)               Crosschain(by substrate pallet)
```

## Plan of Demo (WIP)

1. Findora Defi Chain (A ETH compact chain based on the substrate (frontier)).
2. Deploy ETH/ERC20/ERC721 (contracts) in ETH testnet.
3. Use `ChainBridge` to cross these assets.
4. Support vanilla ETH ? (It seems support. Map ETH on A chain to ERC20 on B Chain)
5. Map FRA to Findora Defi Chain in ERC20 by js quickly.

### Findora Defi Chain Demo

[frontier](https://github.com/paritytech/frontier)

Verify this chain can use all tools for ETH.
- [X] Basic Transfer.
- [X] Smart Contract.
- [X] ERC20/ERC721 Token.

## Plan of Step 2:

### Findora Defi Chain

This chain based on frontier or substrate.

1. Totally compact ETH.
2. Crosschain bridge in decentralized on Findora Defi Chain.
2. Write a pallet to cross ETH. Reference: [compound gateway](https://github.com/compound-finance/gateway)
3. Write a pallet to cross Findora.
    - [ ] Add basic Event for transaction on finadora.
    - [ ] Add lock account support on finadora.
    - [ ] Bridge.

## Step for Demo.

### ERC20 & ERC721

> Token on rinkeby.

```bash
https://rinkeby.etherscan.io/tx/0x60362a94097fe74b95eaf2af8effae8f059d635bfe33374114533eeec0aa17d4
```

### Start environment

#### ChainBridge

Use this command to deploy smart contract on ETH. We use Rinkeby testnet.

``` bash
./index.js --url https://rinkeby-light.eth.linkpool.io/ --privateKey $PRIVATE_KEY --gasPrice deploy --all --relayers 0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5 --relayerThreshold 1 --chainId 0
```

Output:

``` bash
Deploying contracts...
✓ Bridge contract deployed
✓ ERC20Handler contract deployed
✓ ERC721Handler contract deployed
✓ GenericHandler contract deployed
✓ ERC20 contract deployed
WARNING: Multiple definitions for safeTransferFrom
✓ ERC721 contract deployed
✓ CentrifugeAssetStore contract deployed

================================================================
Url:        https://rinkeby-light.eth.linkpool.io/
Deployer:   0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Gas Limit:   8000000
Gas Price:   10000000000
Deploy Cost: 0.15561065

Options
=======
Chain Id:    0
Threshold:   1
Relayers:    0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Bridge Fee:  0
Expiry:      100

Contract Addresses
================================================================
Bridge:             0xB7223605039dD9BFAE528B71eA7157725b9d8416
----------------------------------------------------------------
Erc20 Handler:      0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A
----------------------------------------------------------------
Erc721 Handler:     0xBc55db4d86b1CaBD46901A81D832fd0ce001d939
----------------------------------------------------------------
Generic Handler:    0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
----------------------------------------------------------------
Erc20:              0x14F92974D3980D74cf64BF83bc9f2c4b420e1A10
----------------------------------------------------------------
Erc721:             0xd73BE42678b94Cb75e8Bb840b2df1BEe1a26320A
----------------------------------------------------------------
Centrifuge Asset:   0xAed12A4f43ca3B97f234c61D7d637cC1534C396b
================================================================
```

Then configure contracts on ETH.

```
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 bridge register-resource \
        --bridge 0xB9cAFB95eb31Fc68807fbfA5b52bDB87eF6932fB \
        --handler 0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A \
        --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00 \
        --targetContract
        ```

