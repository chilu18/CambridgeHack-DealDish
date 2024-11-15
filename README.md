# DishDeal
DishDeal is a reduce-food-waste to earn iOS app that uses augmented reality and machine learning to tokenise food into 3D NFTs, connecting consumers with restaurants that have excess food. Upon purchase and feedback, users are rewarded with B3TR tokens for each transaction, redeemable for future discounts or events at the participating restaurants.



- [Video](https://youtu.be/XVJjdIk-8Fc)
- [Deck](https://www.canva.com/design/DAF_vmwDygs/Rcg9VjB7_lQ0k18DvzawSw/edit?utm_content=DAF_vmwDygs&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)
- [One-Pager](https://docs.google.com/presentation/d/1873mPEsKndjUbPkEcYQeLXyGMbBpuLim/edit?usp=sharing&ouid=102918366363179824848&rtpof=true&sd=true)

## Team

- Artemiy Malyshau: MSc Applied CS and Engineering, Imperial College London [Connect](https://www.linkedin.com/in/artemiy-malyshau/)
- Jeevan Jutla: Binance Security Engineer [Connect](https://www.linkedin.com/in/jeevan-jutla/)

## Images
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/ea833294-4e58-40fd-aba0-e2c4ded65f88 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/1ea06490-61bc-4542-95e8-72b9ef0de5c4 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/fad67247-ceb2-4e21-9b64-168a05ffb101 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/552eea04-2f86-44fd-bfb5-a46a4198ae86 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/857c4da6-ffb4-45d9-b691-730ad2e07c12 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/c034c109-e7e6-41d5-a7e3-157b6ef3db3d width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/61276444-bc3c-4473-991b-59bee33eca01 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/d59551a6-399f-490e-9407-e949f7923709 width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/8f657e23-9bb8-45f7-9cb1-048060e12f0d width=19%> 
<img src=https://github.com/nkoorty/CambridgeHack/assets/80065244/42368563-c48a-41e4-8857-6deab9887daf width=19%> 


## Tech Stack
All code is commented 🚀

- `@walletconnect/web3modal-swift` to connect to existing veworld wallet.
- `@vechain/connex` to build the raw data for contract interaction and allow a logical fork at the latest step between Web3Auth and connex-signing-service.
-  `thor-devkit` to handle transaction signing.
- `@vechain/ethers` for custom wallet management with a private key.
- `@walletconnect/web3modal-swift` and `@web3auth/modal` for the Web3Auth implementation.
- `@google/GoogleSignIn-iOS`: authenticating google credentials.
- **Role-Based Access Control Contract**
- **Flask Server:**  to parse the image and 3D model between the phone to the server and update status of the AWS NeRF model.
- **Nvidia NeRF Instant NGP:** To generate a 3D model from an image in ~45 seconds [Paper](https://docs.nerf.studio/nerfology/methods/instant_ngp.html) 
- **AWS EC2**: distribute NeRF model across GPUs.
- **Web3Storage API**: for IPFS storage of the 3D model.
- **TS backend**:conecting payments and issues commands to create or delete NFTs.
- `@vechain/vechain-sdk-core` to sign transactions and deploy the contract.
- **NFT digital twin contract**: containing metadata and hashes of the model.
- **Marketplace Contract**: to manage transactions of tokens between users.
- **Apple Pay**: for fiat payment payment provider.
- **SwiftUI**: iOS Frontend & AR Kit.
- **ERC-20**: is used to reward the user upon transaction and feedback.

## Workflow
![ArchDiagram](https://github.com/nkoorty/CambridgeHack/assets/22000925/0bf9040b-c054-470a-81ed-63673b8579e7)

#### Sign-Up & Login
-  Once the user opens the app they have two options:
    1. Login: Connect VeWorld using WalletConnect
    2. Sign Up: Walletless onboarding using Google OAuth and Web3Auth as a non-custodial way of storing private keys:
 - A Role-Based Access Control contract is used to manage if a user is a consumer or a restaurant and their access within the app.

#### Login as a Restaurant
- The restaurant captures an image of the real-world food item they want to tokenise.
- A photo is sent from the app to our Python server (you can try it out) (http://176.58.109.155/) and uploads it to an AWS EC2 instance.
- The AWS EC2 instance is running NVIDIA's NeRF Instant NGP that is used to generate a 3D model (`.usdz`) from an image.
- The 3D model is hashed and stored on IPFS using Web3Storage.
- The restaurant inputs details about the item (Name, Price, Use-by-Date).
- Face ID is used to generate a signer key and `@vechain/vechain-sdk-core` is used to sign the transaction and deploy an NFT digital twin contract that contains the hash of the model along with the metadata.
- The tokenised food item is then displayed in the marketplace.

#### Login as a Consumer
- The user can view the marketplace of the restaurants and food items with their digital twin NFT's.
- The user can view the food items in AR on their phone.
- The user can decide to buy an item and the NFT marketplace contract manages tranasactions of tokens between users.
- To make for a seamless experience for new users, fee delegation is used to facilitate the sale using Apple Pay.
- Once the users make a purchase they are rewarded with a percentage of tokens using a custom ERC-20 token (B3TR token once available) that can be used for discounts on future purchases or events at the restaurant.
- Users can leave reviews and feedback and are rewarded a small amount of ERC-20/B3TR tokens for community feedback and encouragement to act sustainably.


## Tokenomics
- Users earn tokens for each purchase made from the app, with additional rewards for leaving reviews and feedback.
-  Tokens can be redeemed for discounts, special offers, or exclusive events at participating restaurants, driving repeat usage and engagement.
-  The in-app ERC-20 token will act as a 1:1 will the B3TR token.
