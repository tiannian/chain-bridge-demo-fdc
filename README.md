# chain-bridge-demo-fdc

Reference project:

- [compound-finance/gateway](https://github.com/compound-finance/gateway)
- [frontier](https://github.com/paritytech/frontier)
- [substrate](https://substrate.dev/)
- [chainbridge](https://github.com/ChainSafe/ChainBridge)
- [Aurora](https://near.org/zh/blog/aurora-launches-near/)
- [SputnikVM](https://github.com/rust-blockchain/evm)

## Plan

```
Ethereneum <-> Findora
            ^
            |
        ChainBridge
```

## Step for Demo.

### ERC20 & ERC721

> Token on rinkeby.

```bash
https://rinkeby.etherscan.io/token/0xaa4fb0541d18aa4b0b89eb706263ea9c56589698
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

``` bash
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey $PRIVATE_KEY --gasPrice 10000000000 bridge register-resource --bridge 0xB7223605039dD9BFAE528B71eA7157725b9d8416 --handler 0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00 --targetContract 0xaa4fb0541d18aa4b0b89eb706263ea9c56589698
```

Deploy chainbridge contract on FDC.

``` bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey $PRIVATE_KEY --gasPrice 10000000000 deploy --all --relayers 0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5 --relayerThreshold 1 --chainId 1
```
Output:

``` bash
✓ Bridge contract deployed
✓ ERC20Handler contract deployed
✓ ERC721Handler contract deployed
✓ GenericHandler contract deployed
✓ ERC20 contract deployed
WARNING: Multiple definitions for safeTransferFrom
✓ ERC721 contract deployed
✓ CentrifugeAssetStore contract deployed

================================================================
Url:        http://127.0.0.1:9933
Deployer:   0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Gas Limit:   8000000
Gas Price:   10000000000
Deploy Cost: 0.15526877

Options
=======
Chain Id:    1
Threshold:   1
Relayers:    0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Bridge Fee:  0
Expiry:      100

Contract Addresses
================================================================
Bridge:             0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A
----------------------------------------------------------------
Erc20 Handler:      0xB7223605039dD9BFAE528B71eA7157725b9d8416
----------------------------------------------------------------
Erc721 Handler:     0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A
----------------------------------------------------------------
Generic Handler:    0xBc55db4d86b1CaBD46901A81D832fd0ce001d939
----------------------------------------------------------------
Erc20:              0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
----------------------------------------------------------------
Erc721:             0x14F92974D3980D74cf64BF83bc9f2c4b420e1A10
----------------------------------------------------------------
Centrifuge Asset:   0xd73BE42678b94Cb75e8Bb840b2df1BEe1a26320A
================================================================
```

Configure bridge

```bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey $PRIVATE_KEY --gasPrice 10000000000 bridge register-resource \
    --bridge 0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A \
    --handler 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00 \
    --targetContract 0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
```

Set burn

``` bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey $PRIVATE_KEY --gasPrice 10000000000 bridge set-burn \
    --bridge 0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A \
    --handler 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --tokenContract 0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
```

Set mint

``` bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey $PRIVATE_KEY --gasPrice 10000000000 erc20 add-minter \
    --minter 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --erc20Address 0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
```

Configure and start chainbridge

Approve token:

``` bash
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey $PRIVATE_KEY --gasPrice 10000000000 erc20 approve \
    --amount 100 \
    --erc20Address 0xaa4fb0541d18aa4b0b89eb706263ea9c56589698 \
    --recipient 0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A
```

Deposit token

``` bash
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey $PRIVATE_KEY --gasPrice 10000000000 erc20 deposit \
    --amount 100 \
    --dest 1 \
    --bridge 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --recipient 0xA346505ABcD3670812dc8DDCC10535baeAe78541 \
    --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00
```
