# Duckiebot DB21M Post-build Setup
> This guide was created at my supervisor’s request for Hacettepe engineers learning in the Robotics & AI Lab with Duckiebots.  
That said, it’s also helpful for anyone who’s feeling a bit lost during the post-build setup of the DB21M Duckiebot.

After building my Duckiebot, I ran into a few hiccups during the post-build process.  
Honestly, I found that the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/intro.html) wasn’t super clear (at least for me).  
The order of steps felt a bit confusing, the information was scattered, and some parts were a bit too wordy.  

So, I decided to summarize and reorder the key steps into a simple, easy-to-follow guide.  
Hopefully, this will save future students some time (and frustration!) and make the process a lot smoother.


## SD Card Initialization
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
Connect to the Duckiebot using an Ethernet cable.  
You’re now directly linked to the robot and can run pretty much any command on it.  

That said, it’s not ideal to have a moving robot tethered to your laptop with a cable.  
So, here’s a simple guide to set it up over Wi-Fi.  
(In my opinion, this method is easier than the one in the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/intro.html) — but feel free to use whichever works for you!)


## Dashboard Setup
Check out the [official documentation](https://docs.duckietown.com/daffy/opmanual-duckiebot/setup/setup_dashboard/index.html) for setting up the dashboard.  
Once it’s running, make sure to test all the hardware components from the dashboard and confirm that everything is working properly.

