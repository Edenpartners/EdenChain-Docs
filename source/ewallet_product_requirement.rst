EWallet Product Requirements
============================

Goals
To define user experience scenario of the E-Wallet app
Background and strategic fit
Eden Partners is preparing DApp ecosystem within EdenChain, called Garden of Eden.
The main goal of Garden of Eden is to promote circulation of EDN coins by expanding various use cases.
E-Wallet will be a key interface for EDN coin holders and DApp users to add/withdraw/manage EDN coins.
E-Wallet also needs to store value and keep track of transactions of EDN currency as well as to send value to others.
Target platform of E-Wallet is Android and iOS.
Terms and Assumptions
EDN wallet - an cryptographically generated address for Ethereum to store and transfer ERC-20 created EDN coin. Later when EdenChain main net is available, the EDN wallet should support EdenChain too. This wallet has a public address information and private key. The private key should never be disclosed to public.
DApp wallet - an cryptographically generated address for Eden Chain super-node (the application specific side-chain of Eden Chain).
EDN - symbol of EdenChain based cryptographic coin. EDN is used commonly to refer ERC-20 created coin and EdenChain main net generated coin together. And both have identical value,
TEDN - (tentative) symbol of Garden of Eden (DApp eco system) specific coins. TEDN can be consumed within Garden of Eden environment only and can only be exchanged from/to EDN. The value of each TEDN is identical and pegged to the value of 1 EDN. Each registered user of Eden Chain platform can have only one TEDN wallet.
TEDN wallet technically does not exist at E-Wallet. TEDN wallet for a user is created and maintained at Eden Platform backend servers and access by user's sign-in. E-Wallet shows balance and transaction history of TEDN by browsing information from Eden Platform backend servers.
Requirements
From users perspective E-Wallet provides functions in high level as followings:

Sign-up	User needs to sign up EdenChain platform service by creating new user account at his/her choice or authentication of selected 3rd party service (eg. google, facebook, etc). Sign-up needs to be completed before accessing any of E-Wallet service.	Must have	User account is unique identity to EdenChain platform and EDN and TEDN wallets. Access to EDN and TEDN wallets is provided by EdenChain platform user account.
Sign-in	User needs to sign in with valid user account into EdenChain platform service before accessing any of E-Wallet service.	Must have	User Authentification by IAM
Create EDN wallet(s)	
User can create one or more EDN wallet and maintain as a list

Must have


Deposit EDN	
User can exchange their ETH or ERC-20 based tokens into EDN and deposit into a specified EDN wallet

Must have	
Processing by 3rd party DEX API, eg. Kyber Network
ETH is a Must Have option/BTC is favorable
Withdraw from EDN to other coins is not considered
App should be able to handle all exchange process within it's UX
Deposit TEDN	User can transfer specified EDN amount from a specific EDN wallet to TEDN	Must have	User can deposit TEDN from E-Wallet's main UX or a popup window invoked by web pages.
Withdraw TEDN	User can transfer specified TEDN amount to a specific EDN wallet	Must have	User can deposit TEDN from E-Wallet's main UX or a popup window invoked by web pages.
Transfer between EDN wallets	User can transfer EDN value from a specific EDN wallet to another	Must have	
Send EDN	User can transfer EDN value from his(her) EDN wallet to other's EDN wallet address	Must have	
Receive EDN	Show wallet address and QR code conveniently whenever user needs to present EDN wallet to receive EDN	Middle	
Balance Check	User should be able to check up-to-dated balance of each EDN and TEDN	Must have	Balance of EDN should be retrieved by ETH network (for ERC-20 EDN) and balance of TEDN should be retrieved by Eden Chain backend servers.
Transaction History	User should be able to view history of every transaction of each EDN and TEDN	Must have	
Security Check	E-Wallet should not run when the OS is rooted	Must have	
Private Key Storage	
Private key of each EDN wallet should be stored in encrypted way and should not remain in memory or any kind of storage as a clear text whenever is not used

Must have	When supported by OS or device manufacturer, encryption/decryption can be operated by biometric information
Wallet backup and recovery	Each EDN/DApp wallet can be backup and recovered by human readable seed phrases	Must have	
Seed phrase for each wallet is generated when wallet address is first created. User is required to keep this seed phrase in safe since this information is only way to restore wallet.

PIN Create	User should create 6-digit universal PIN code	High	A universal PIN needs to be used to withdraw EDN wallets or TEDN wallet. For this purpose, when user first sign up, user should be prompted to create PIN number.
PIN Check	User should enter 6-digit valid PIN code	High	When user needs to withdraw TEDN or deposit TEDN(withdraw EDN), user must enter valid PIN code as authorization.
IAM Integration	Admin user can monitor and analyze user's behavior through IAM's service	High	
A/B Testing, Analytics, App Indexing, Authentication (except Phone Auth), Cloud Messaging (FCM), Crashlytics, Dynamic Links, Invites, Performance Monitoring, Predictions, and Remote Config.

The above should be possible

User interaction and design
Story board currently being edited at following location:

https://www.figma.com/file/oquI1z7YwNoZAKNnTHa5Nv/E-Wallet?node-id=0%3A1

In order to play "prototype", please click "play" button on right top corner
