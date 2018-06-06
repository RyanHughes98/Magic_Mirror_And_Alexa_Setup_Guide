# MySmartMirror
<h1>Raspberry Pi Magic Mirror and Alexa App Setup Guide</h1>

This guide can be used to setup and configure the Magic Mirror and Alexa applications.

While many similar guides exist already the Auto Starting of the new Alexa Standalone App isn't well doccumented so I made that part of the guide is particularally detailed as it is likly going to be more helpful than the other parts of the guide.

<em><strong>This guide has been modified and not tested prior to the modification, should you have any issues feel free to contact me but I can't guarentee a response or help as this guide was originally only written to be part of a College Project and I no longer have the required hardware to test the guide.<strong></em>

<h2>Step 1: Install NOOBS (Operating System)</h2>

In order to install NOOBS, you must have a micro SD card, micro SD to SD card adapter and a computer with a SD card slot.

1. Download NOOBS
2. Download a SD card formatter
3. Insert the Micro SD card into the adapter and insert that into the computer
4. Use the SD card formatter to format the Micro SD card
5. Extract the noobs ZIP folder onto the Micro SD card
6. Safely remove the MicroSD card


<h2>Step 2: Setup the Hardware</h2>

Alexa is very specific in the way its audio has to be setup, therefore, we recommend you should follow the setup closely, unless you are an advanced user.

1. Attach the Pi to a display using a HDMI cable, should the display not have a HDMI input you can use an adapter.
2. Attach a speaker via the 3.5 mm jack.
3. Attach a microphone via a USB port, use a USB sound card if your chosen microphone has a 3.5mm connection.
4. Power the Pi with a micro USB charging cable.
5. Insert the Micro SD card.


<h2>Step 3: Configure the Pi</h2>

1. Power on the Pi.
2. Select and Install Raspbian when prompted. This installation will take several minutes.
3. When the installation finishes, you will see the desktop. Navigate through the menus to find the configuration settings. Set the country, time zone and WIFI location to Ireland, or the country that the device will be used in.
4. Turn on VNC and SSH to allow for remote connection.
5. Navigate to audio settings in the menus.
6. Setup the microphone’s (USB device) controls. Set it to be a microphone and adjust the volume levels. Should the microphone be muted, unmute it.
7. Setup the speaker controls. Set it to be a speaker and adjust the volume levels to your desired level.
8. Setup up the WIFI (top right), connect to a network and enter the password.
9. Reboot the device.


<h2>Step 4: Magic Mirror Installation</h2>

<h3>Step 4.1: Magic Mirror Initial Installation</h3>

1. Open a terminal.
2. Enter the command below: 

        bash -c "$(curl -sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)".
3. Wait for a prompt, and when prompted if you want to use pm2 for auto starting, enter “y”.
4. Install pm2 with the command below: 

        sudo npm install -g pm2
5. Start pm2 with the command below: 

        pm2 startup 

Useful Information: As the Magic Mirror will start whenever it is closed due to pm2, in order to have it out of the way for you to continue with installation press “CTRL + m” to minimise it rather than closing it.


<h3>Step 4.2: Magic Mirror Configuration</h3>

1. Open a terminal.
2. Change to the Magic Mirror directory: 

        cd MagicMirror
3. Open the configuration file with the command: 

        sudo nano config/config.js
4. Edit the configuration by changing positions of modules and changing what they display. 

Dealing with Errors: Should the configuration file have errors in it, the Magic Mirror will display an error message rather than the usual display. It will recommend that you run the configuration file through an error checker but in order to save time instead I recommend that you keep a working copy of the configuration file which you can revert to should you have any errors as error checkers won’t always be able to help fix the errors, rather just show where they are.

<h3>Step 4.3: Magic Mirror 3rd Party Modules</h2> 

In order to use 3rd party modules you must first install them and their dependencies before you can add them to the configuration file. 
Note: Every module is different and therefore they are likely to have different installation steps. Please, refer to the installation guide that is available with each module and should you need further assistance, refer to the forums for help with individual modules.   Why you should install 3rd party modules While the default modules that come with the Magic Mirror are good, they barely scratch the surface of what is possible to achieve with the Magic Mirror software.

<h3>3rd party modules that were easily installed</h3>

<h4>Currently Installed:</h4>

-MMM-Wallpaper -MMM-DublinRTPI -MMM-Remote-Control -MMM-SoccerLiveScore -MMM-Wallpaper

<h4>Currently Uninistalled:</h4>	

-MMM-AlexaPi -phone-notification-mirror

<h4>3rd Party Modules that were hard to install or didn’t install</h4>

-MMM-awesome-alexa -MMM-MirrorMirrorOnTheWall


<h2>Step 5. Installing Alexa</h2>

Installing Alexa’s standalone version on the Raspberry Pi can be broken down into three steps:

Step 5.1: Registering your device on an Amazon Developer Account.

Step 5.2: Installing and setting up Alexa on your device.

Step 5.3: Editing the Alexa source code and enabling AutoStarting of the Alexa app.

<h3>Step 5.1: Registering the Device</h3>

Before you can register the device, you must first create an Amazon Developer account. The process of creating a developer account is relatively simple and therefore I didn’t find it necessary to create a guide to help doing so. Once you have an Amazon Developer account created, you can follow these steps:

1. Log into your Amazon Developer Account and click on the Alexa Tab.
2. Register a new device.
3. Name the device type and display name. We used “Pi” for both, take note of the ID.
4. Click “Next”.
5. On the Security Profile screen, click “Create a new profile”.
6. Under the general tab, give the profile a name and a description, then click “Next”.
7. Click the Web settings tab and copy the Client ID.

<h3>Step 5.2: Cloning and Installing Alexa on the Pi</h3>

1. Open a terminal
2. Enter: 

        wget https://raw.githubusercontent.com/alexa/avs-device-sdk/master/tools/Install/setup.sh && wget https://raw.githubusercontent.com/alexa/avs-device-sdk/master/tools/Install/config.txt && wget https://raw.githubusercontent.com/alexa/avs-device-sdk/master/tools/Install/pi.sh
3. Open and edit the config.txt file to include the Client ID and Product ID from step 1 by using the command: 

        sudo nano config.txt
4. Run the setup script and follow any prompts by using the command: 

        sudo bash setup.sh config.txt
5. Initialise the app by entering: 

        sudo bash startsample.sh
6. Note: the 6-digit code from the start of the command. New lines will continually appear pushing it up.
7. Go to https://www.amazon.com/code on any browser (on any device) and enter the 6-digit code from the previous step.
8. Click “Allow”.

<h3>Step 5.3: Enable AutoStart</h3>
1. Open a terminal.
2. Change the directory to the SampleApps source code by entering: 
        
        cd avs-device-sdk/SampleApp/src
3. Open the UserInputManager.cpp by using: 

        sudo nano UserInputManager.cpp
4. Modify part of the code to add the following lines: 

        std::this_thread::sleep_for(std::chrono::hours(1)); 
        
        continue; 
        
 So that the code will now look like: 
 
        m_interactionManager->begin(); 
        while (true) { 
          std::this_thread::sleep_for(std::chrono::hours(1)); 
          continue; 
          char x; 
          std::cin >> x;
5. Open a new terminal.
6. Create and open a file with: 

        sudo nano alexa.sh
7. Add the following to the file:

        #!bin/bash
        sudo /bin/bash /home/pi/startsample.sh
8. Make the new file executable with: 

        sudo chmod +x /home/pi/alexa.sh
9. Create and open a system.service file with: 

        sudo nano /lib/systemd/system/alexa.service and enter
10. Enter the following into the new file: 
        
        [Unit] 
        Description=Amazon 
        Alexa After=network.target network-online.target

        [Service]    
        Type=idle         
        ExecStart=/bin/bash        
        /home/pi/alexa.sh        
        WorkingDirectory=/home/pi/build/SampleApp/src         
        User=pi

        [Install]        
        WantedBy=multi-user.target

11. Change the permission of the service file using: 

        sudo chmod 644 /lib/systemd/system/alexa.service
12. Recompile the source code with: 
  
        sudo bash setup.sh config.txt
13. Reload daemon with: 

        sudo systemctl daemon-reload
14. Enable the service you created with: 

        sudo systemctl enable alexa.service
15. Start the service you created with: 

        sudo systemctl start alexa.service
16. Check the status of the newly created service: 

        sudo systemctl status alexa.service
17. Reboot the device to make sure everything works.
