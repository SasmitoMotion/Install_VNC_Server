# Install_VNC_Server
Install VNC Server to Raspberry Pi

On your Pi (using a monitor or via SSH), install the TightVNC package:

    sudo apt-get install tightvncserver
  
Next, run TightVNC Server which will prompt you to enter a password and an optional view-only password:

    tightvncserver
  
Start a VNC server from the terminal. This example starts a session on VNC display zero (:0) with full HD resolution:

    vncserver :0 -geometry 1920x1080 -depth 24
  
Now, on your computer, install and run the VNC client and on a Linux machine install the package xtightvncviewer:

    sudo apt-get install xtightvncviewer
  
Create a file containing the following shell script to run VNC Server for your Raspberry Pi:

    #!/bin/sh
    vncserver :0 -geometry 1920x1080 -depth 24 -dpi 96
  
Save this file as name : vnc.sh and make this file executable

    chmod +x vnc.sh
  
check your file vnc.sh by command line to look your file execute or not :

    ls -la

Then you can run it at any time with:

    ./vnc.sh
  
To run at boot and log into a terminal on the Pi as root:

    sudo su
  
Navigate file to the directory /etc/init.d/:

    cd /etc/init.d/
  
Create a new file here containing the following script:

    #! /bin/sh
    # /etc/init.d/vncboot
    
    ### BEGIN INIT INFO
    # Provides: vncboot
    # Required-Start: $remote_fs $syslog
    # Required-Stop: $remote_fs $syslog
    # Default-Start: 2 3 4 5
    # Default-Stop: 0 1 6
    # Short-Description: Start VNC Server at boot time
    # Description: Start VNC Server at boot time.
    ### END INIT INFO
    
    USER=pi
    HOME=/home/pi
    
    export USER HOME
    
    case "$1" in
        start)
        echo "Starting VNC Server"
        #Insert your favoured settings for a VNC session
         su - pi -c "/usr/bin/vncserver :0 -geometry 1280x800 -depth 16 -pixelformat rgb565"
        ;;
   
    stop)
        echo "Stopping VNC Server"
        /usr/bin/vncserver -kill :0
        ;;
    
    *)
        echo "Usage: /etc/init.d/vncboot {start|stop}"
        exit 1
        ;;
    esac
    
    exit 0
  
Save this file as vncboot and make this file executetable:
  
    chmod 755 vncboot
  
check your file to execute file like vncboot on your command :
  
    ls -la
  
Enable dependency-based boot sequencing:

    update-rc.d /etc/init.d/vncboot defaults
  
If enabling dependency-based boot sequencing was successful, you will see this:

    update-rc.d: using dependency based boot sequencing
  
But if you see this:

    update-rc.d: error: unable to read /etc/init.d//etc/init.d/vncboot
  
then try the following command:

    update-rc.d vncboot defaults
  
Reboot your Raspberry Pi and you should find a VNC server already started.

Referrence : https://www.raspberrypi.org/documentation/remote-access/vnc/ https://www.raspberrypi.org/documentation/remote-access/vnc/linux.md
https://www.realvnc.com/products/vnc/raspberrypi/ http://www.instructables.com/id/Setting-up-a-VNC-Server-on-your-Raspberry-Pi/step2/Installing-a-VNC-Server-on-your-Raspberry-Pi/
http://elinux.org/RPi_VNC_Server
