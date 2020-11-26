# Oasis-Network-Quest-Hackathon


An Oasis NFT Farm  by staking ERC20 Token

**Video Explanation:**   
[TestDemo video](https://drive.google.com/file/d/1h1Xmf1iqTTbmIcguAwhQE_K5qeF_Gds4/view?usp=sharing) <br>
[CoreFeature video](https://drive.google.com/file/d/1GUqVjkfHYmNEFYqkpNq23_k3Xj5eTGsq/view?usp=sharing)



OasisTestToken address: 0xc0beca469ba923b4dd31cf1eb89a737fedc78c33 <br>
OasisTestNFT address: 0xaee6120f7c721e926584dd52811611b151877d43 <br>
OasisNFTFarm address : 0xe8e610630a2465bb66af312bb2d0ec4b01c1cce0

## Overall Process Flow:
 * Deploy NFTFarm Contract with two constructor emissionRate which is  the rate of points generating per LP token per second staked, and lpToken contract address.
 * Administrator use addNFT function to transfer NFT from ERC1155 Contract.
 * User should approve for BFTFarm in ERC1155 Contract  before depositing lp tokens to this contract.
 * NFT's are deposited into this contract, having some Price as `points` associated to them.
 * In order to claim an NFT, the user must have sufficient `points` to reach Price threshold.
 * To increase `points` balance, user must deposit lp tokens to this contract.
 * `points` balance increases dynamically with each passing second allowing user to Farm NFTs!

## Features:
* Supports ERC-1155 NFT
* Stake LP tokens.
* Rate of NFT accumulation proportional to amount of LP tokens user is providing.
* Farm for all NFTs at once. Choose particular NFTs on claim.
* Resume farming from where left, if LP tokens withdrawn in between.
* Claim all eligible random NFTs with farmed points balance.
* Withdraw LP tokens, and claim NFTs in single transaction.

## Functions:
```constructor(uint256 _emissionRate, IERC20 _lpToken) public```

_emissionRate: points generated per LP token (wei) per second staked by user

_lpToken: token address to be staked

```
function addNFT(
        address contractAddress,    // token contract address. Only ERC-1155 NFT Supported!
        uint256 id,                 // token id
        uint256 total,              // amount of NFTs deposited to farm (need to approve before)
        uint256 price               // price in `points`
    ) external;
```
Can only be called by owner. To add ERC-1155 NFT to be farmed by others.
Owner must have approved contract to transfer NFT before calling this function.

```
function stake(uint256 _amount) external;
```
Called by user to stake _amount of LP tokens in the contract.
User must have approved contract to spend at least _amount of LP tokens.

```
function pointsBalance(address userAddress) public view returns (uint256) 
```
Dynamic function to get points balance accumulated till now.

```
function claim(uint256 _nftIndex, uint256 _quantity) public;
```
Allow user to claim `_quantity` of NFT at index `_nftIndex` in `nftInfo` array, if sufficient points accumulated (else transaction reverts).
NFTs are farmed and sent to user address.

```
function claimRandom() public;
```
Allow user to claim random NFTs from the NFT pool until they exhaust their point balance.

```
function withdraw(uint256 _amount) public;
```
Allow user to withdraw `_amount` of LP tokens. Reverts if `_amount` exceeds deposited LP tokens by user.

```
function exit() external;
```
Allow user to claim random NFTs and withdraw all LP tokens from contract.
