# Follow these instructions, if you would like to use the remote server for 3D Slicer
* [You need to install TurboVNC for your OS](https://sourceforge.net/projects/turbovnc/files/)
* the user name, password and server IP address provided in your workshop package. 

** For Windows:

1. Open command prompt (cmd.exe)
2. Navigate to the folder where TurboVNC installed by typing
cd c:\program files\TurboVNC\
3. In the command prompt, enter the command:
putty USERNAME@SERVERIP
where USERNAME and SERVERIP are the provided username and IP address in your workshop package. 
4. Enter your password in the putty window. You should be now logged into the server
5. In the putty window, enter the command:
vncserver
If this is the first time you are doing this, you will need to set up a vnc password for yourself. You can answer 'no' to the question about view-only password. 

Make a note of the output, as you will need the VNC screen number (value after column: see the screen capture below) 
6. Start the vncviewer.exe from the Start Menu and type in the 
SERVERIP:PORT
where PORT is the value from the output of your vncserver command in step 5.
7. Enter the password you set in step 5. You now should be connected to the Linux Desktop. 
8. To start Slicer or Rstudio, type:
vglrun +v Slicer or
vglrun +v rstudio
