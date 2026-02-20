# Wardriving with the Pineapple Pager
We can use the pager to do some nice wardriving, but to have GPS with it, we have a couple options, using the [Glytch GPS Module](https://shop.hak5.org/products/glytch-gps-mod-for-pager) or [Mobile2GPS](https://github.com/ryanpohlner/mobile2gps)
# Glytch GPS Module
This is form fitting and simple to use, but it does cover the USB-C port so we are unable to charge it. With the Module plugged in, we go to `Settings -> GPS -> Serial Device -> Select TTYUSB0`  and that's it. Pretty straight forward. The GPS is very weak inside from my experience, so it may not show your coordinates immediately. 
# Mobile2GPS
This is my preferred way to go as it's super easy, leave the USB-C port open, and makes the pager easier to carry. To start we need to connect our pager to our machine so we can either SSH into it or access the Virtual Pager at http://172.16.51.1:1471 .

We will need to have the Management AP setup for our phone to connect to. To ensure this is setup we go to: `Settings -> Network -> Management AP Setup`.

<img width="478" height="224" alt="2026-02-20_12-47" src="https://github.com/user-attachments/assets/24f51a04-4383-4128-889b-7b6c81a99a05" />

Give it a network name, setup the encryption type, and the passphrase.

We need to clone the repository if you're on MacOS or Linux, you can either download and extract the .zip file or use `git`. Also download the mobile2gps from the [releases page](https://github.com/ryanpohlner/mobile2gps/releases/) or with wget. 
```
git clone https://github.com/ryanpohlner/mobile2gps.git && cd mobile2gps

wget https://github.com/ryanpohlner/mobile2gps/releases/download/v1.6/mobile2gps
```
<img width="957" height="843" alt="Pasted image 20260220130945" src="https://github.com/user-attachments/assets/3da484bf-dfd2-4d31-8c6d-384cac8e4b12" />

From here we need to make the mobile2gps2gps directory on our pager under `/mmc/root/payloads/user/general/`. Through SSH or the Virtual Pager:
```
mkdir /mmc/root/payloads/user/general/mobile2gps
```
<img width="710" height="361" alt="Pasted image 20260220130531" src="https://github.com/user-attachments/assets/ce1271dc-bf4a-45a6-966a-3d5eed26efb7" />

Now we need to move the following files to our pager:
- payload.sh
- mobile2gps
- index.html

Using scp or [WinSCP](https://winscp.net/eng/download.php) if you're on Windows.
```
scp payload.sh root@172.16.52.1:/mmc/root/payloads/user/general/mobile2gps/
scp mobile2gps root@172.16.52.1:/mmc/root/payloads/user/general/mobile2gps/
scp index.html root@172.16.52.1:/mmc/root/payloads/user/general/mobile2gps/
```
<img width="953" height="325" alt="Pasted image 20260220131058" src="https://github.com/user-attachments/assets/c5f2530b-17ce-4f76-abed-7c04e4bd456b" />


Make sure our phone is connected to our Management AP. On the pager go to: `Payloads -> general -> mobile2gps -> Launch`.
<img width="490" height="223" alt="Pasted image 20260220125040" src="https://github.com/user-attachments/assets/5d598f12-eda3-42bf-a5af-fcc37e243741" />

Once here, press the Up Arrow to start the server for the GPS.
<img width="496" height="225" alt="Pasted image 20260220125128" src="https://github.com/user-attachments/assets/3bcc1f68-0a6c-4ee2-b654-9ad32346e0fa" />


Now on our phone we go to: https://172.16.52.1:1993 and press `Start`.
<img width="1080" height="1708" alt="signal-2026-02-20-131458" src="https://github.com/user-attachments/assets/61e083ad-3be3-4e08-ad18-bab86003ea97" />

On the pager we can press the right arrow to verify we have GPS coordinates.
<img width="491" height="231" alt="Pasted image 20260220125336" src="https://github.com/user-attachments/assets/fd050b9e-2732-4316-b4a6-c3a52d2a991f" />

We can also verify by going to: `Settings -> GPS`.

<img width="482" height="212" alt="Pasted image 20260220125442" src="https://github.com/user-attachments/assets/2091659d-744d-45c1-a123-97aa27542bf5" />

From here, we can start recon mode, press the Green button and go down to enable `Wigle Mode`. This will save a wigle ready .csv file for us to use.

<img width="480" height="216" alt="Pasted image 20260220125611" src="https://github.com/user-attachments/assets/c49e29d1-bc22-418a-a4fc-8dfdca5486c8" />
<img width="482" height="232" alt="Pasted image 20260220125628" src="https://github.com/user-attachments/assets/9fd4773e-ebd5-4bfa-9a7b-90b0263cc45b" />

Now we can either get the files off of our Pager using scp (or WinSCP) to upload to Wigle. OR we can use [wigle-toolkit](https://github.com/hak5/wifipineapplepager-payloads/blob/master/library/user/exfiltration/wigle-toolkit/payload.sh). This will allow us to upload our .csv files for wigle in `/mmc//root/loot/wigle/`.
