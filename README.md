# chain-bridge-demo-fdc

Use this command to deploy smart contract on ETH. We use Rinkeby testnet.

```
./index.js --url https://rinkeby-light.eth.linkpool.io/ --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice deploy --bridge --erc20Handler --relayers 0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5 --relayerThreshold 1 --chainId 1
```

Output:

```
Deploying contracts...
✓ Bridge contract deployed
✓ ERC20Handler contract deployed

================================================================
Url:        https://rinkeby-light.eth.linkpool.io/
Deployer:   0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Gas Limit:   8000000
Gas Price:   10000000000
Deploy Cost: 0.05865926

Options
=======
Chain Id:    1
Threshold:   1
Relayers:    0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Bridge Fee:  0
Expiry:      100

Contract Addresses
================================================================
Bridge:             0xB9cAFB95eb31Fc68807fbfA5b52bDB87eF6932fB
----------------------------------------------------------------
Erc20 Handler:      0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A
----------------------------------------------------------------
Erc721 Handler:     Not Deployed
----------------------------------------------------------------
Generic Handler:    Not Deployed
----------------------------------------------------------------
Erc20:              Not Deployed
----------------------------------------------------------------
Erc721:             Not Deployed
----------------------------------------------------------------
Centrifuge Asset:   Not Deployed
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

