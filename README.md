# IBC-Relayer

This guide assumes that the Rizone server is already running on the server, the Kichin client is installed and there are wallets with a balance in both networks. The ep relay will be launched with the help of an official reseller from the Cosmos team.

### 1.Download and install the relayer: <br>
>git clone https://github.com/cosmos/relayer.git<br>
>cd relayer<br>
>make install<br>
>cd<br>

### 2. Initialize the relayer:<br>
>rly config init<br>

### 3. Create and go to the folder in which the settings for our networks will be stored:<br>
>mkdir <name>_config<br>
>cd <name>_config<br>
  
### 4. Create a file for the Stargaze network and write the necessary settings there:<br>
  
>vi kichain-t-4.json<br>
  
##### Insert the following data inside the file<br>
>{<br>
>  "chain-id": "kichain-t-4",<br>
>  "rpc-addr": "https://rpc-challenge.blockchain.ki:443",<br>
>  "gas-adjustment": 1.5,<br>
>  "gas-prices": "0.025utki",<br>
>  "trusting-period": "48h"<br>
>}<br>

### 5. Do same for the Osmosis network:<br>
  
>vi groot-011.json<br>
  
>{<br>
>  "chain-id": "groot-011",<br>
>  "rpc-addr": "http://localhost:26657",<br>
>  "account-prefix": "rizon",<br>
>  "gas-adjustment": 1.5,<br>
>  "gas-prices": "0.025uatolo",<br>
>  "trusting-period": "48h"<br>
>}<br>

### 6. Parse these settings into the relayer config:<br>
  
>rly chains add -f cygnusx-1_config.json<br>
>rly chains add -f kichain-t-4.json<br>
>cd<br>

### 7. Create new wallets for interacting with the relayer (the team recommends not using validator keys, but creating new ones, since otherwise there may be errors up to the node crash). Don’t forget to save your seed phrases and wallet addresses:<br>
  
>rly keys add groot-011 name  wallet <br>
>rly keys add kichain-t-4 name wallet<br>

### 8. Add the newly created keys to the config of the relayer:<br>
>rly chains edit groot-011 key rizon-RELAYER-KEY-NAME<br>
>rly chains edit kichain-t-4 key kichain-RELAYER-KEY-NAME<br>
  ### 9. Change the waiting timeout in the relayer settings (there were no recommendations from the team on this matter, but in the previous testnet, due to the low timeout, transactions did not have time to be processed by the relayer:<br>
  
>nano ~/.relayer/config/config.yaml<br>
>Looking for a line<br>
>timeout: 10s<br>

>Replace with<br>
>timeout: 30s<br>
  
 ### 10. Transfer to the relayer wallets 10 RIZON and 10 KICHAIN <br>
  >rizon-KEY-NAME — this is the name of the key that was used to run the validator<br>
  >kichain-KEY-NAME — the corresponding key on the Osmosis<br>
  
>rizon tx bank send rizon-KEY-NAME rizon-RELAYER-ADDRESS 10000000uatolo<br>
>kid tx bank send kichain-KEY-NAME kichain-RELAYER-ADDRESS 10000000utki<br>
  
 ### 11.  Wallets must be funded on both networks. check the availability of coins with the command:<br>
>rly q balance groot-011<br>
>rly q balance kichain-t-4<br>

  ##### If there are coins on the balance and the relay shows them, then we continue, We initialize the light client in both networks with the command:<br>

>rly light init groot-011 -f<br>
>rly light init kichain-t-4 -f<br>
We try to generate a channel between the networks with the command:<br>

>rly paths generate groot-011 kichain-t-4 transfer  -- port=transfer<br>
##### If the command does not work, try several times or add the - debug parameter to see the step-by-step actions of the system the output should be as follows:<br>

>Generated path(transfer), run 'rly paths show transfer  -- yaml' to see details<br>
