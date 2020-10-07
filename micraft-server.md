# Overview
In this lab, you set up a game application—a Minecraft server.

The Minecraft server software will run on a Compute Engine instance.

You use an n1-standard-1 machine type that includes a 10-GB boot disk, 1 virtual CPU (vCPU), and 3.75 GB of RAM. This machine type runs Debian Linux by default.

To make sure there is plenty of room for the Minecraft server's world data, you also attach a high-performance 50-GB persistent solid-state drive (SSD) to the instance. This dedicated Minecraft server can support up to 50 players.


# Objectives
In this lab, you learn how to perform the following tasks:

Customize an application server

Install and configure necessary software

Configure network access

Schedule regular backups


# Create the VM
on google cloud console

        using the gcloud console to create vm instances 
        under Identity and API access > Access scopes	Set access for each API Storage	Read Write


# Add a new disk


|properties   |  Value (type value or select option as specified) |
|-------------|---------------------------------------------------|
| name        | minecraft-disk                                    |
| disk type   | SSD Persistent Disk                               |
| source type |  None (blank disk)                                |
| size        | 50                                                |
| encryption  |Google-managed key                                 |


# networking

|properties          |  Value (type value or select option as specified) |
|--------------------|---------------------------------------------------|
| network tag        | minecraft-server                                  |
| network interfaces | Click default to edit the interface               |
| source type        |  None (blank disk)                                |
| external ip-address| Create IP Address                                 |
| name               |mc-server-ip                                       |



# Task 2: Prepare the data disk
### To create a directory that serves as the mount point for the data disk, run the following command:

              sudo mkdir -p /home/minecraft

### To format the disk, run the following command:

            sudo mkfs.ext4 -F -E lazy_itable_init=0,\
            lazy_journal_init=0,discard \
            /dev/disk/by-id/google-minecraft-disk

### To mount the disk, run the following command:

            sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/minecraft


# Task 3: Install and run the application

The Minecraft server runs on top of the Java Virtual Machine (JVM), so it requires the Java Runtime Environment (JRE) to run. Because the server doesn't need a graphical user interface, you use the headless version of the JRE. This reduces the JRE's resource usage on the machine, which helps ensure that the Minecraft server has enough room to expand its own resource usage if needed.


### Install the Java Runtime Environment (JRE) and the Minecraft server

In the SSH terminal for mc-server, to update the Debian repositories on the VM, run the following command:

        sudo apt-get update

### After the repositories are updated, to install the headless JRE, run the following command:

        sudo apt-get install -y default-jre-headless

### To navigate to the directory where the persistent disk is mounted, run the following command:


        cd /home/minecraft

### To install wget, run the following command:

        sudo apt-get install wget

### To download the current Minecraft server JAR file (1.11.2 JAR), run the following command:

        sudo wget https://launcher.mojang.com/v1/objects/d0d0fe2b1dc6ab4c65554cb734270872b72dadd6/server.jar

# Initialize the Minecraft server

1 To initialize the Minecraft server, run the following command:

        sudo nano eula.txt

Change the last line of the file from eula=false to eula=true
       
        sudo java -Xmx1024M -Xms1024M -jar server.jar nogui

# Create a virtual terminal screen to start the Minecraft server

If you start the Minecraft server again now, it is tied to the life of your SSH session: that is, if you close your SSH terminal, the server is also terminated. To avoid this issue, you can use screen, an application that allows you to create a virtual terminal that can be "detached," becoming a background process, or "reattached," becoming a foreground process. When a virtual terminal is detached to the background, it will run whether you are logged in or not.

### To install screen, run the following command:

        sudo apt-get install -y screen

To start your Minecraft server in a screen virtual terminal, run the following command: (Use the -S flag to name your terminal mcs)

        sudo screen -S mcs java -Xmx1024M -Xms1024M -jar server.jar nogui

### Detach from the screen and close your SSH session

To detach the screen terminal, press Ctrl+A, Ctrl+D. The terminal continues to run in the background. To reattach the terminal, run the following command:

        sudo screen -r mcs


# Task 4: Allow client traffic
Up to this point, the server has an external static IP address, but it cannot receive traffic because there is no firewall rule in place. Minecraft server uses TCP port 25565 by default. So you need to configure a firewall rule to allow these connections.

        use the console to create a firewall to allow tcp:25565


# Task 5: Schedule regular backups

#### ssh to the vm instances created first

Create a globally unique bucket name, and store it in the environment variable YOUR_BUCKET_NAME. To make it unique, you can use your Project ID. Run the following command:

        export YOUR_BUCKET_NAME=<Enter your bucket name here>

To create the bucket using the gsutil tool, part of the Cloud SDK, run the following command:

        gsutil mb gs://$YOUR_BUCKET_NAME-minecraft-backup   

## Create a backup script

1 In the mc-server SSH terminal, navigate to your home directory:

        cd /home/minecraft    

2 To create the script, run the following command:

        sudo nano /home/minecraft/backup.sh   

3 Copy and paste the following script into the file:

        #!/bin/bash
        screen -r mcs -X stuff '/save-all\n/save-off\n'
        /usr/bin/gsutil cp -R ${BASH_SOURCE%/*}/world gs://${YOUR_BUCKET_NAME}-minecraft-backup/$(date "+%Y%m%d-%H%M%S")-world
        screen -r mcs -X stuff '/save-on\n'

4 To make the script executable, run the following command:

        sudo chmod 755 /home/minecraft/backup.sh

#     Test the backup script and schedule a cron job

1 In the mc-server SSH terminal, run the backup script:

        . /home/minecraft/backup.sh


2 In the mc-server SSH terminal, open the cron table for editing:

        sudo crontab -e
When you are prompted to select an editor, type the number corresponding to nano, and press ENTER.

3  At the bottom of the cron table, paste the following line:

        0 */4 * * * /home/minecraft/backup.sh

_That line instructs cron to run backups every 4 hours_

# Task 6: Server maintenance

        sudo screen -r -X stuff '/stop\n'

to stop the server
