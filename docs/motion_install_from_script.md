Motion MasterNode Install from script (Recommended)
-------

Preparation

Get a VPS from a provider like DigitalOcean, Vultr, Linode, Scaleway, Time4vps etc. 
Recommended VPS size: 2GB RAM (if less its ok, we can make swap).
It must be Ubuntu 16.04 (Xenial).
Exactly 1000 MTN on your mn address of desktop cold wallet. 
NOTES:
- motion.conf file on LOCAL wallet MUST BE EMPTY! 
- masternode.conf file on VPS wallet MUST BE EMPTY! 
- PRE_ENABLED status is NOT an issue, just restart local wallet and wait a few minutes. 
- You need a different IP for each masternode you plan to host

Local Wallet Setup

Open your wallet on your desktop, click Receive, put your Label such as “MN1” then click Request and Copy the Address and Send EXACTLY 1000 MTN to this Address.

Wait for at least 15 confirmations then go to the tab at the bottom that says "Tools" and click at the top that says "Console" then run following command: 

``masternode outputs``

You should see one line corresponding to the transaction id (tx_id) of your 1000 coins with a digit identifier (digit). Save these two strings in a text file.
Example: 
``{ "6a66ad6011ee363c2d97da0b55b73584fef376dc0ef43137b478aa73b4b906b0": "0" }``
Note that if you get more than 1 line, it’s because you made multiple 1000 coins transactions, with the tx_id and digit associated.
Run the following command:

``masternode genkey``

You should see a long key: (masternodeprivkey) EXAMPLE: 7xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

This is your masternode private key, record it to text file, keep it safe, do not share with anyone. This will be called “masternodeprivkey”. 
Next, you have to go to the data directory of your wallet Go to wallet settings=> and click “Open masternode configuration file” You should see 2 lines both with a # to comment them out.
Please make a new line and add:

``MN1 (YOUR VPS IP):7979 masternodeprivkey tx_id digit``

Put your data correctly, save it and close. 
Go to Motion Wallet, Click Settings, Check “Show Masternodes Tab” Save and Restart your wallet.
Note that each line of the masternode.conf file corresponds to one masternode, if you want to run more than one node from the same wallet, just make a new line with new alias like MN2-MN3… and repeat steps.

VPS Setup

Preparation: Windows users will need a program called putty to connect to the VPS For a guide of how to use putty to connect to a vps please use: 
HOW TO USE PUTTY.
We need to install some dependencies. Please copy, paste this one-line command and hit enter:
apt-get update;apt-get upgrade; apt-get install nano software-properties-common git wget -y;
Now Copy this one-line command into the VPS hit enter:
wget https://raw.githubusercontent.com/juniormasters/motion-docs/master/scripts/masternode.sh && chmod +x masternode.sh && ./masternode.sh
When prompted, enter your “masternodeprivkey” from before and hit enter.
You will be asked for your VPS IP:7979 and a few other questions. 
The installation include the latest version of Motion Wallet on VPS.
The installation should finish successfully in a few minutes. Ask for help in discord if it doesn't.
Please note, the script will move motiond and motion-cli binaries to /usr/bin folder, so you don't need to navigate to motion/src folder anymore, you can run the commands without "./" on any place now.
After the script finishes, you will want to check that it is running properly. 
Please type in:
``motion-cli getinfo``
If you get an error about permissions, you just need to kill the process and restart with:
``killall motiond``
and restart with:
``motiond -daemon``
now test with:
``motion-cli getinfo``
or
``motion-cli getblockcount``
If you get an error that file does not exist, it may be that the script failed to build and we need to trace back the problem. Contact devs in discord.

Starting Your Masternode

Go back to your desktop wallet, to the Masternode tab. You need to wait for VPS to be full synced with network, you can check on your VPS by:
``motion-cli getblockcount``
(needs to the same block number as explorer to be in sync - check at: explorer.motionproject.org )
NOTE: If the Masternode tab isn’t showing, you need to click settings, check “Show Masternodes Tab” save, and restart the wallet If your Masternode does not show, restart the wallet
Now select the Masternode ALIAS you just setup and click START ALIAS and confirm it.
Your masternode should be now up and running!
You can check the masternode status by VPS and typing:
motion-cli masternode status
If your masternode is running it should print “Masternode successfully started”.
You can also check your MN status by local wallet - tools - console, just type:
masternode list full XXXXX
(Where XXXXX is yours first 5 character of TX_ID or VPS IP).

CONGRATULATIONS!
