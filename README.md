# Redbelly-Node-Deployment (updating . . . ) 
#### Remodification of [( @HerculesNode )](https://github.com/kemevo/RedBelly-Node) in conjuction with Official Docs

---

## Minimum Hardware Requirements

- **CPU:** 8-core CPU or equivalent
- **Architecture:** x86-64 (also known as x64, x86_64, AMD64, and Intel 64)
- **RAM:** 16 GB of RAM
- **Operating System:** Ubuntu 22.04.2 LTS or later (x86-64 compatible)
- **Storage:** 1 TB
- **Network Bandwidth:** 40 Mbps with a stable connection

---



# Updates and Installation 🫡
To set up your Redbelly Node, run the following commands in your terminal:

```bash
sudo apt update
sudo apt install screen snapd net-tools cron curl unzip
sudo DEBIAN_FRONTEND=noninteractive apt-get -y upgrade
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot

```


# Create Screen & continue

```bash
screen -S red
email=your_email@example.com
fqn=example.xyz
sudo certbot certonly --standalone -d $fqn. --non-interactive --agree-tos -m $email
sudo chown -R $USER:$USER /etc/letsencrypt/

```
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/cc923f00-dbc3-49b4-8e37-cc439896114e)


#### Set your email (replace 'your_email@example.com' with your actual email)
#### Set your Fully Qualified Domain Name (replace 'example.xyz' with your actual domain)
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/b1651913-0f79-454f-a1cf-def717eeca59)  


#### Save the PATH to SSL, you will need this later 
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/ff6c28da-eeb8-45d1-a4be-116a5e298931)


# Firewall Configuration
```bash
sudo ufw enable
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 8545
sudo ufw allow 1888
sudo ufw allow 1111
```

---

# Now go back to the mail you received for the node process 
#### Click the `Set up your account `

![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/b55576ea-9a34-48c5-bebe-b327f44abf25)
[Redbelly Node Service Desk](https://redbelly.atlassian.net/servicedesk/customer/portal/13/user/login?destination=portal%2F13%2Fgroup%2F17%2Fcreate%2F86)


## FIll the form with this input in appropriate section 
#### Create two metamask wallet ( we will be using the private key of the second one) 

![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/f96de0a3-c076-46a4-877a-3607bb8edd97)

Node Url : domain name from namecheap
Public address : first new metamask wallet
Signing address : second new metamask wallet (private key of this needed) 
The below ports are recommended, however, make sure you supply the same port values in the node registration form which you will find in Node Registration defined in the next section:
- 8545: HTTP-RPC port
- 8546: WS-RPC port
- 1111: Recovery port
- 1888: Consensus port


# WAIT TO RECEIVE ID AT THIS POINT ---- I AM ALSO WAITING FOR THAT ----
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/c434fc0d-5986-4a03-a053-dc0aeba983b2) 

---

# APPLICATION HAS BEEN APPROVED, LET'S PROCEED 🫡
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/5fcb47e2-d43d-44f8-9229-be488759714f)
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/f436358d-88ab-4af0-84f6-f0eb68c8b841)

#### Take note of your Node ID

---

# Configuring config.yaml
To configure the `config.yaml` file, open it using the `nano` text editor. Run the following command in your terminal:

```bash
nano config.yaml
```

### Paste this

``` ip: <ENTER FULLY QUALIFIED DOMAIN NAME (FQDN) HERE>
id: <ENTER THE ID PROVIDED HERE>
genesisContracts:
  bootstrapContractsRegistryAddress: 0xDAFEA492D9c6733ae3d56b7Ed1ADB60692c98Bc5
consensusPort: 1888
grpcPort: 1111
privateKeyHex: <Private key for signing address for activity monitor>
poolConfig:
  initCap: 5
  maxCap: 30
  idleTimeout: 180
clientKeepAliveConfig:
  keepAliveTime: 1
  keepAliveTimeOut: 20
serverKeepAliveConfig:
  serverKeepAliveTime: 70
  serverKeepAliveTimeOut: 10
  minTime: 60
rpcPoolConfig:
  maxOpenCount: 1
  maxIdleCount: 1
  maxIdleTime: 30
```


#### Update the following parameters in the config.yaml file:

- ip: Set it to your domain name from namecheap (example.xyz) depending on your configuration.
- id: You will receive this ID by email after filling out the form.
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/c66d26ce-b510-4a0d-a1d7-a6165f29e23e)
- privateKeyHex: Use the private key of the wallet provided as the signing address in the form.

#### Ctrl + X and Y then [ENTER] to exit editor 


# Node Initialization

To initialize your Redbelly Node and create an `observe.sh` script, follow these steps:

1. Create the `observe.sh` script using the `touch` command:
    ```bash
    touch observe.sh
    ```
    
2. Open the `observe.sh` script in the `vim` text editor:
    ```bash
    vim observe.sh
    ```

3. Within the `vim` editor, you can add your Node initialization script. Press `i` to enter insert mode, paste below script, and press `Esc` to exit insert mode.
4. Save and exit the `vim` editor by typing `:wq` and pressing `Enter`.
5. These steps will create an empty `observe.sh` script and open it in the `vim` editor for you to add your Node initialization script.


```
#!/bin/sh
# filename: observe.sh
if [ ! -d rbn ]; then
  echo "rbn doesnt exist. Initialising redbelly"
  mkdir -p rbn
  mkdir -p consensus
  cp config.yaml ./consensus

  ./binaries/rbbc init --datadir=rbn --standalone
  rm -rf ./rbn/database/chaindata
  rm -rf ./rbn/database/nodes
  cp genesis.json ./rbn/genesis
else
  echo "rbn already exists. continuing with existing setup"
  cp config.yaml ./consensus
fi

# Run EVM
rm -f log
./binaries/rbbc run --datadir=rbn --consensus.dir=consensus --tls --consensus.tls --tls.cert=<PATH TO SSL CERTIFICATE> --tls.key=<PATH TO SSL CERTIFICATE KEY> --http --http.addr=0.0.0.0 --http.corsdomain=* --http.vhosts=* --http.port=8545 --http.api eth,net,web3,rbn --ws --ws.addr=0.0.0.0 --ws.port=8546 --ws.origins="*" --ws.api eth,net,web3,rbn --threshold=200 --timeout=500 --logging.level info --mode production --consensus.type dbft --config.file config.yaml --bootstrap.tries=10 --bootstrap.wait=10 --recovery.tries=10 --recovery.wait=10
```
 - Replace `PATH TO SSL CERTIFICATE`
 - Replace `PATH TO SSL CERTIFICATE KEY`

#### Remember from this section (saved ) 
![image](https://github.com/mztacat/Redbelly-Node-Deployment/assets/31314340/563d68bb-bdd7-4083-839d-1ce4b22cc274)

#### Replace with your SSL path ( see my sample replacement)
``` # Run EVM
rm -f log
./binaries/rbbc run --datadir=rbn --consensus.dir=consensus --tls --consensus.tls --tls.cert=/etc/letsencrypt/live/red.mztacat.xyz/fullchain.pem --tls.key=/etc/letsencrypt/live/red.mztacat.xyz/privkey.pem --http --http.addr=0.0.0.0 --http.corsdomain=* --http.vhosts=* --http.port=8545 --http.api eth,net,web3,rbn --ws --ws.addr=0.0.0.0 --ws.port=8546 --ws.origins="*" --ws.api eth,net,web3,rbn --threshold=200 --timeout=500 --logging.level info --mode production --consensus.type dbft --config.file config.yaml --bootstrap.tries=10 --bootstrap.wait=10 --recovery.tries=10 --recovery.wait=10
```

#### Save and exit the `vim` editor by typing `:wq` and pressing `Enter`.


# Creating and Editing start-rbn.sh

To create and edit the `start-rbn.sh` script, follow these steps:

1. Create the `start-rbn.sh` script using the `touch` command:
    ```bash
    touch start-rbn.sh
    ```
2. Open the `start-rbn.sh` script in the `vim` text editor:
    ```bash
    vim start-rbn.sh
    ```

```
#!/bin/sh
# filename: start-rbn.sh
mkdir -p binaries
mkdir -p consensus
chmod +x rbbc
cp rbbc binaries/rbbc
mkdir -p logs
nohup ./observe.sh > ./logs/rbbcLogs 2>&1 &
```


4. Save and exit the `vim` editor by typing `:wq` and pressing `Enter`.

These steps will create an empty `start-rbn.sh` script and open it in the `vim` editor for you to add your Redbelly Node start script.


# Make executable and `RUN` 

```
chmod +x observe.sh
chmod +x start-rbn.sh
./start-rbn.sh
```
