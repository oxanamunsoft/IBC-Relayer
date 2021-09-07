# IBC-Relayer

This guide assumes that the Rizone server is already running on the server, the Kichin client is installed and there are wallets with a balance in both networks. The ep relay will be launched with the help of an official reseller from the Cosmos team.

###1.Download and install the relayer: <br>
>git clone https://github.com/cosmos/relayer.git<br>
>cd relayer<br>
>make install<br>
>cd<br>

2. Initialize the relayer:<br>
>rly config init<br>

3. Create and go to the folder in which the settings for our networks will be stored:<br>
>mkdir <name>_config<br>
>cd <name>_config<br>
  
4. Create a file for the Stargaze network and write the necessary settings there:
  
vi kichain-t-4.json
  
#Insert the following data inside the file
{
  "chain-id": "kichain-t-4",
  "rpc-addr": "https://rpc-challenge.blockchain.ki:443",
  "account-prefix": "tki",
  "gas-adjustment": 1.5,
  "gas-prices": "0.025utki",
  "trusting-period": "48h"
}

5. Do same for the Osmosis network:
  
vi groot-011.json
{
  "chain-id": "groot-011",
  "rpc-addr": "http://localhost:26657",
  "account-prefix": "rizon",
  "gas-adjustment": 1.5,
  "gas-prices": "0.025uatolo",
  "trusting-period": "48h"
}

6. Parse these settings into the relayer config:
  
rly chains add -f cygnusx-1_config.json
rly chains add -f kichain-t-4.json
cd

7. Create new wallets for interacting with the relayer (the team recommends not using validator keys, but creating new ones, since otherwise there may be errors up to the node crash). Don’t forget to save your seed phrases and wallet addresses:
  
rly keys add groot-011 name  wallet 
rly keys add kichain-t-4 name wallet

8. Add the newly created keys to the config of the relayer:
rly chains edit groot-011 key groot-RELAYER-KEY-NAME
rly chains edit kichain-t-4 key kichain-RELAYER-KEY-NAME
  9. Change the waiting timeout in the relayer settings (there were no recommendations from the team on this matter, but in the previous testnet, due to the low timeout, transactions did not have time to be processed by the relayer:
  
nano ~/.relayer/config/config.yaml
Looking for a line
timeout: 10s

Replace with
timeout: 30s
  
