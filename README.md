# Beez Coin

Shell script to install a `Beez Coin Masternode` on a Linux server running Ubuntu 16.04. 
Supports IPv4, IPv6 and a single master node per single VPS.

## Installation

The installation script will do everything needed to setup your VPS with the latest updates and perform all steps required to install the Beez masternode without any interaction or programming knowledge. 

When you run the script it will tell you what it will do on your system. Once completed there is a summary of the information you need to be aware of regarding your node setup which you can copy/paste to your local PC. 
```
Make sure you keep the script output information safe for later.
Never show or tell anyone what your private key is or they can steal your coins.
```

You will need to run the script before setting up your node in your local wallet as it will generate a `private key` for you to use in your `masternode.conf` file.
To get started with the installation, login to your VPS as the root user and run the two lines below.

```
wget https://raw.githubusercontent.com/BeezCrypto/beez-mn-install-script/master/install-beez.sh
bash install-beez.sh
```

&nbsp;

## How to setup your masternode with this script and a cold wallet on your PC
```
Run the script first so you have the information when it finishes, to complete your cold wallet setup.
```
The script assumes you are running a cold wallet on your local PC and this script will execute on a Ubuntu Linux server. The steps involved are:

 1. Run the masternode installation script as per the [instructions above](https://github.com/BeezCrypto/beez-mn-install-script#installation).
 2. When you are finished this process you will get some information on what has been done as well as some important information you will need for your cold wallet setup.
 3. **Copy/paste the output of this script into a text file and keep it safe.**

You are now ready to configure your local wallet and finish the masternode setup:

 1. Make sure you have downloaded the latest wallet from https://github.com/BeezCrypto/BeezCore/releases
 2. Install the wallet on your local PC
 3. Start the wallet and let it completely synchronize to the network - this will take some time
 4. Create a new `Receiving Address` from the wallets `File` menu and name it appropriately, e.g. MN-1
 5. Unlock your wallet and send **exactly 10,000 BZZ** to the address created in step #4
 6. Wait for the transaction from step #5 to be fully confirmed. Look for a tick in the first column in your transactions tab
 7. Once confirmed, open your wallet console and type: `masternode outputs`
 8. Open your masternode configuration file from the wallets `Tools` menu item.
 9. In your masternodes.conf file add an entry that looks like: `[address-name from #4] [ip:port of your VPS from script output] [privkey from script output] [txid from from #7] [tx output index from #7]` - 
 10. Your masternodes.conf file entry should look like: `MN-1 127.0.0.2:45292 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0` and it must be all on one line in your masternodes config file. Make sure there are no blank lines in the file and no leading or trailing spaces on any of the lines in the file. Do not copy/paste from MS Word, if you are pasting any content use a plain text editor like Notepad, Textpad, etc. or you may get errors when the wallet starts.
 11. Save and close your masternodes.conf file
 12. Close your wallet and restart
 13. Go to Masternodes
 14. Click the row for the masternode you just added
 15. Right click the masternode row, then click Start Alias
 16. Your node should now be running successfully, wait for some time for it to connect with the network and the `Active` time to start counting up.


&nbsp;

## Masternode commands (VPS)
Because the masternode runs under a user account, you cannot login as root to your server and run commands like `beez-cli masternode status`, if you do you will get an error. You need to first switch to the `beez-mn1` user. **So make sure you keep the output of the script after it runs.**

Each time you run the script to install a node it will automatically create a new `beez-mn1` user for the node to run under. 

The masternode runs as a service, so if you do not stop the service and just try to stop the daemon process, it will restart again. You can start and stop the masternode when logged in as root or the `beez-mn1` user by running the following two commands:

#### To stop your masternode 
```
systemctl stop beez-mn1.service
```

#### To start your masternode 
```
systemctl start beez-mn1.service
```

#### To interactively switch from the root user to the masternode user, you can run:
```
 su - beez-mn1
```
If you are asked for a password, it is in the script output you received when you installed the masternode, you can right click and paste the password (which will not be shown), then press Enter.
The following commands can then be run against the node which is running as the `beez-mn1` user.

#### To query your masternodes status
```
 beez-cli masternode status 
```

#### To query your masternode information
```
 beez-cli getinfo
```

#### To query your masternodes sync status
```
 beez-cli mnsync status
```

## Removing a masternode and user account (VPS)
If something goes wrong with your installation or you want to remove the masternode, you can do so with the following command.
```
 userdel -r beez-mn1
```
This will remove the user and its home directory. You then re-run the installation script and it will re-create everything for the `beez-mn1` user.

&nbsp;

## Security
The script will set up the required firewall rules to only allow inbound node communications, whilst blocking all other inbound ports and all outbound ports.

The [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page) package is also used to mitigate DDoS attempts on your server.

Despite this script needing to be run as `root` you should secure your Ubuntu server as normal with the following precautions:

 - disable password authentication
 - disable root login
 - enable SSH certificate login only

If the above precautions are taken you will need to `su root` before running the script. You can find a good tutorial for securing your VPS [here](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04).

&nbsp;

## Disclaimer
Whilst effort has been put into maintaining and testing this script, it will automatically modify settings on your Ubuntu server - use at your own risk. By downloading this script you are accepting all responsibility for any actions it performs on your server.

&nbsp;







