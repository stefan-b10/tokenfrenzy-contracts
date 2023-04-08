# Token Frenzy

Token Frenzy is a  ERC20 tokens only lottery game on blockchain. Players can enter the lottery with any ERC20 accepted by the lottery manager when starting the lottery. Players odds of winning are calculated based on price vs. WETH(or other token chosen by the manager) (current version using only Uniswap V2 pools) when entering the lottery (tokens are not swapped to WETH, just price fetching). This incentivise players to 'hunt' best prices to increase their changes of winning the lottery.
## DISCLAIMER!!!
This contract is for learning/testing purposes only. Not to be used in production!!! Becase of the way the prices of tokens are calculated a player can easily manipulate the price when entering the lottery by using flashloans.

## Installation
Clone repository:
```
git clone https://github.com/stefan-b10/tokenfrenzy-contracts.git
```

Install dependencies:
```
npm install
```

## Usage

### Deploying contract
Deploying the contract needs 4 arguments:
 - LINK address (Lottery manager is responsible to fund the contract with LINK tokens needed for getting a random number from Chainlink VFR2)
 - VRFWrapper address 
 - WETH address (or other token chosen by manager/deployer)
 - Network to deploy to (goerli or mumbai)
```
npm run deploy "linkAddress" "vrfWrapper" "wethAddress" "network"
```
### Start lottery
Starting lottery needs 7 arguments:
 - Contract address (address of the deployed TokenFrenzy contract)
 - A array of accepted tokens 
 - A array of pools from wich to calculate the value for each accepted tokens
 - Minimum accepted bet (in WETH)
 - Owners fee in proccent (10000 = 100%)
 - Lottery closing time in seconds since unix epoch
 - Network
```
npm run startLottery "contractAddress" "address1,address2...addressN" "pool1,pool2...poolN" "nimBet" "ownersFee" "closingTime" "network"
```

### Enter lottery
Enter lottery need 4 arguments:
 - Contract address (address of the deployed TokenFrenzy contract with lottery started)
 - Token address
 - Amount of token
 - Network
```
npm run enterLottery "contractAddress" "tokenAddress" "amountToken" "network"
```
### Close lottery
Close lottery can be called by anyone if closing time < time of calling. When ``closeLottery`` function is called, the contract sends a request for a random number to Chainlink VRF2, wich after a minimmum of 3 block will callback the contract with the random number generated.
It needs 2 arguments:
 - Contract address (address of the deployed TokenFrenzy contract)
 - Network
```
npm run closeLottery "contractAddress" "network"
```
### Find winner
Function ``findWinner`` can be called by anyone, it checks every player if he is the winner in 100 batches => it migth need multiple calls to find a winner. Random generated number will be in range [0,9999]. Each player is assigned a range of numbers, whom ever has the range wich contains the random number wins. It needs 2 arguments:
 - Contract address (address of the deployed TokenFrenzy contract with lottery started)
 - Network
```
npm run findWinner "contractAddress" "network"
```
### Withdraw tokens
This function can be called by the lottery winner or the manager, it sends all the assigned tokens to the caller (TotalTokens - fee for the winner, fee for the manager). It need 3 arguments:
 - Contract address (address of the deployed TokenFrenzy contract with lottery started) 
 - Index of the lottery
 - Network
```
npm run withdrawTokens "contractAddress" "index" "network"
```

A basic UI for interacting with the contract can be found [TokenFrenzy app](https://github.com/stefan-b10/tokenfrenzy-app.git)
