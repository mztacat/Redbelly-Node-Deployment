# Part 1 of this tutorial will be on Twitter --- This is the part 2 be sure to be directed from there

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
