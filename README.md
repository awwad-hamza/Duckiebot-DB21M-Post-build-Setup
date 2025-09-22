# Duckiebot DB21M Post-build Setup
![](images/hacettepe.jpg)
![](images/duckiebot.jpg)
> This guide was created at my supervisor’s request for Hacettepe engineers learning in the Robotics & AI Lab with Duckiebots.  
That said, it’s also helpful for anyone who’s feeling a bit lost during the post-build setup of the DB21M Duckiebot.

After building my Duckiebot, I ran into a few hiccups during the post-build process.  
Honestly, I found that the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/intro.html) wasn’t super clear (at least for me).  
The order of steps felt a bit confusing, the information was scattered, some bugs I faced didn't have solutions in the documentation, and some parts were a bit too wordy.  

So, I decided to summarize and reorder the key steps into a simple, easy-to-follow guide.  
Hopefully, this will save future students some time (and frustration!) and make the process a lot smoother.


## SD Card Initialization
I started from this point and assumed that all steps prior to the SD card initialization in the [Laptop Setup](https://docs.duckietown.com/daffy/opmanual-duckiebot/setup/setup_laptop/index.html) section have already been completed.

Insert your SD card into your computer using an adapter, then choose a **HOSTNAME** for your Duckiebot.  
(I used `DB21M` for configuratoin since that’s what we have in the lab.)

```
dts init_sd_card --hostname HOSTNAME --type duckiebot --configuration DB21M --wifi WIFI
```
> **Note:** Some Duckiebots in the lab are already set up and labeled with their hostnames, so you can skip this step.  
>  
> If you’re not sure whether the SD card of a Duckiebot is already configured, you can:


1. Insert the SD card into your computer using an adapter.
2. Open the **boot partition** (it’s usually the only visible partition).
3. Find a file named **`hostname`**.
4. Open it with any text editor — it will just have the hostname inside (for example: `duckiebot123`).

If the SD card is already setup you can skip this step.

## Networking
On your host computer, run the following command (replace `HOSTNAME` with your Duckiebot’s hostname).  
This removes any old SSH keys for that host, preventing connection issues:  

```
ssh-keygen -f "$HOME/.ssh/known_hosts" -R "HOSTNAME.local"
```

Then, connect to the Duckiebot using an Ethernet cable.  
You’re now directly linked to the robot and can run pretty much any command on it.  

That said, it’s not ideal to have a moving robot tethered to your laptop with a cable.  
So, here’s a simple guide to set it up over Wi-Fi.  
(In my opinion, this method is easier than the one in the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/intro.html) — but feel free to use whichever works for you!)

### To be able to use wifi on your Duckiebot
1. first ssh to your duckiebot (the default password for all duckiebots is "quackquack")
```
ssh duckie@hostname.local
```
2. Turn on Wi-Fi and edit the NetworkManager config
```
nmcli radio wifi on
sudo nano /etc/NetworkManager/NetworkManager.conf
```
3. Look for
```
[ifupdown]
managed=false
```
If you see `managed=false`, change it to:
```
[ifupdown]
managed=true
```
if it is already set to `true` you are ready to go

4. Restart Network Manager
```
sudo systemctl restart NetworkManager
```

### Now to connect to a specific Wi-Fi network from your duckiebot
1. List available networks
```
nmcli device wifi list
```
2. Connect to a specific Wi-Fi network
```
nmcli device wifi connect "SSID_NAME" password "YOUR_PASSWORD"
```
If the connection is successful, you’ll see a confirmation like `successfully activated`

To see your current connections:
```
nmcli connection show
```

Now you can disconnect the Ethernet cable and connect your computer to the same Wi-Fi network as your Duckiebot.  
Once both are on the same network, the Duckiebot should appear when you run:  

```bash
dts fleet discover
```

> **Notes:**
> - Connecting the Duckiebot to the university network or eduroam can be tricky. Even if it connects, it often causes problems when trying to access the Duckiebot from your computer. Using a mobile hotspot or a home network usually works much better.
> - Many issues happen simply because you’re not sure whether you’re SSH’ed into the Duckiebot or running commands on your host computer. Double-check which terminal you’re in before troubleshooting.

## Debugging & Updates
Before doing anything else, make sure your Duckiebot software and Duckiebattery are up to date.  
Follow the official instructions here:

- [Duckiebot Update](https://docs.duckietown.com/daffy/opmanual-duckiebot/debugging_and_troubleshooting/update/index.html)  
- [Duckiebattery Update](https://docs.duckietown.com/daffy/opmanual-duckiebot/debugging_and_troubleshooting/update/duckiebattery-update.html)

If you encounter errors with Docker, they’re often caused by incorrect date & time settings or other minor issues.  
You can fix this on the Duckiebot by running:

```bash
sudo date -s "yyyy-mm-dd hh:mm:ss"
sudo apt update
sudo apt install --reinstall ca-certificates
sudo systemctl restart docker
sudo reboot
```

## Dashboard Setup
Check out the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/setup/setup_dashboard/index.html) for setting up the dashboard.  
Once it’s running, make sure to test all the hardware components from the dashboard and confirm that everything is working properly.

if your duckiebot does not recognize the HUT (Dashboard > Robot > Components) check out this piece of documentation [Re-flash Microcontroller](https://docs.duckietown.com/daffy/opmanual-duckiebot/debugging_and_troubleshooting/reflash_mc/index.html).

> **Note:** If the ToF sensor is not detected, go back to step 56 in the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/assembly/db21m/index.html).  
> The issue is most likely due to following the instructions in the `Attention` section.  
> 
> To fix it, reconnect the ToF sensor while **ignoring the `Attention` note**:  
> 1. Connect the ToF sensor to the front bumper using channel 6 with the short 4-pin cable.  
> 2. Then, connect channel 6 from the bumper to its place on the Raspberry Pi using the long 4-pin cable.  
>
> This should resolve the detection issue.


## Duckiebot Operations and Calibrations
You’re now ready to test the basic operations of your Duckiebot and perform the necessary calibrations.  
Follow the official documentation for guidance [here](https://docs.duckietown.com/daffy/opmanual-duckiebot/operations/handling/db21.html), and you can also try the [lane following demo](https://docs.duckietown.com/daffy/opmanual-duckiebot/demos/index.html) to make sure everything is working properly.

Once that’s done, you can explore the [learning experiences](https://docs.duckietown.com/daffy/opmanual-duckiebot/lx/index.html) or dive straight into writing your own software for the Duckiebot after reviewing the [developer manual](https://docs.duckietown.com/daffy/devmanual-software/intro.html).
