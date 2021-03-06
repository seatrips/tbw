# Python True Block Weight

## Installation

```sh
install and sync relay server
git clone https://github.com/galperins4/tbw
cd ~/tbw
pip3 install setuptools
pip3 install -r requirements.txt
nano package.json (see configuration below)
npm install
sudo npm install pm2@latest -g (if using pm2)
```

## Configuration & Usage

Before runnning npm install update package.json by removing the line for the unneeded depency. Keep ark for ark/kapu support and lwf for lwf support

After the repository has been cloned you need to open the `config.json` and change it to your liking. Once this has been done execute `python3 tbw.py` to start true block weight script.

Alternatively I have also included an apps.json file if you want to run tbw via PM2 (need to install PM2 first and then pm2 start apps.json)

Important! - pay_addresses and keep keys should match in config.json. DO NOT delete the reserve key as it is required. All other's can be deleted or more added. In addition, payment is triggered to start based on when total blocks forged / interval is an integer (with no remainder). 

As the script leverages @FaustBrians ARK python client as well as database retreival and storage classes, python 3.6+ is required. In addition it is  now required to run this alongside an ark/kapu relay node given the DB interaction and little reliance on the API.

## Available Configuration Options
- netork: which network(options are ark, dark, kapu, lwf, lwf-t, oxy, oxy-t, onz, onz-t)
- start_block: script will start calculations only for blocks after specified start block
- delegate IP: this serves as a back-up IP for the API to call to in case the localhost does not respond
- dbusername: this is the postgresql database username nodeDB (usually your os username)
- publicKey: delegate public key
- interval:  the interval you want to pay voters in blocks. A setting of 211 would pay ever 211 blocks (or 422 ark)
- voter_share: percentage to share with voters (0.xx format)
- passphrase: delegate passphrase
- secondphrase: delegate second passphrase
- voter_msg: ARK and ARKfork coins only - message you want in vendor field for share payments
- block_check: How often you want the script to check for new blocks in seconds. Recommend low value (e.g., 30 seconds for ARK coins, high value for LISK coins)
- cover_tx_fees: Use this to indicate if you want to cover transaction fees (Y) or not (N)
- vote_cap: Use this if you cap voters for how much they can earn with votes. For example 10000 will mean any wallet over 10K will only be paid based on 10K weight
- vote_min: Use this if you have a minumum wallet balance to be eligible for payments
- blacklist: Options are block or assign. Block zero's out blocked accounts which then distributes their earnings to voters. Assign does the same but assigns weight to a designated account. 
- blacklist_addr: comma seperated list of addresses to block from voter payments
- blacklist_assign: if assign option is picked, this is the address those blacklisted shares go to. DO NOT SET to an account voting for said delegate. It is HIGHLY recommended this is set to the reserve address!
- fixed_deal: use this if you have a fixed deal with a voter (e.g., 45 ark per day).
- fixed_deal_amt: format is address:amount. The amount to pay should correspond to interval. 
- min_payment: Minimum threshold for payment. If set to 1, any payout less than 1 ARK will be held until the next pay run and accumulate
- reach: how many peers to broadcast payments to (Recommended - 20)
- keep: there are the percentages for delegates to keep and distrubute among x accounts (Note: reserve is required! all others are optional)
- pay_addresses: these are the addresses to go with the keep percentages (Note: reserve is required! all others are optional)

## To Do

- Add more features as necessary
- Additional exception handling

## Changelog

### 1.0
- NOTE: V1.0 made changes to database structure. As such, upgrades from 0.9 and below need to pay out old version (can force it with manual.py or wait until next payrun to switch over) balances and reinitilaize ark.db with v1.0
- added start_block config
- seperated dependency of pay.py on tbw.py. tbw and pay both run via pm2. Pay looks for pay files every 5 minutes and executes
- streamlined payment tx creation. Script now batches 40 tx per run and then pauses 5 minutes

### 0.9
- added support for lwf testnet and mainnet
- added support for oxy testnet and mainnet
- added suppport for onz testnet and mainnet
- added configurable block check option
- added configurable voter share message (ARK coins only)

### 0.8
- added vote-min option to allow for minimum wallet balances eligible for payouts

### 0.7
- small fix for 0 balances in delegate rewards due to changed/unused addresses to prevent broadcasting issues

### 0.6
- Added fixed deal options
- Added functionality for paying (or not paying) transaction fees on share payments
- Added reserve balance check - will not payout if your reserve account <=0 on payrun
- Added manual.py - This will let you pay manually based on values in ark.db (will also update db)

### 0.5
- Completely rewritten to pull data directly from node database for TBD
- Added blacklist functionality

### .05
- Added functionality to cap voters for distributions

### .04
- Squashed import on payment interval bug
- Added file to allow tbw to run via pm2 

### .03
- Modified config file to add minimum payment threshold functionality

### .02
- Modified config file and added multi-address capability for delegate share addresses

### .01
- Initial release

## Support

If you like this project and it helps you in your every day work I would greatly appreciate it if you would consider to show some support by donating to one of the below mentioned addresses.

- BTC - 38jPmBCdu9C5SBPbeb4BTBQG2SAbGvbfKf
- ETH - 0x9c3BB145C6bCde9BC502B90B8C32C0aa26714394
- ARK - AMhTN98yvWP8SJNyxmgEfg9ufuxHyapW73

## Security

If you discover a security vulnerability within this package, please open an issue. All security vulnerabilities will be promptly addressed.

## Credits

- [galperins4](https://github.com/galperins4)
- [All Contributors](../../contributors)

## License

[MIT](LICENSE) © [galperins4](https://github.com/galperins4)





