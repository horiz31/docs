## How to set up an Ardupilot simulator

These instructions assume that you are either running Ubuntu 18 or 20 LTS (within Windows WSL 1 is acceptable) and that the device you are using is connected to the Internet. Note that WSL 2 may not allow the networking access you need.

Get prereqs:
```
sudo apt-get update
sudo apt-get install git
sudo apt-get install gitk git-gui
```
Clone repo:
```
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git checkout Plane-4.3.1
```
Set up build environment:
```
Tools/environment_install/install-prereqs-ubuntu.sh -y
```
Reload the path:
```
. ~/.profile
```
Run the simulator, which will build the code the first time:
```
cd ArduPlane
../Tools/autotest/sim_vehicle.py -j4 -f quadplane
```
Once the simulator is build up and running (it will take several minutes the first time you do it), you will need to set 2 key parameters by typing the following:
```
param set Q_MAV_TYPE 20
param set BRD_SERIAL_NUM 4097
```
At this point, you have ArduPlane 4.3.1 running and it will by default open a port on localhost:14550. Run the GCS and it should automatically connect to the simulator. Windows may ask you for permission to open the port, be sure and ALLOW the connection.

You may stop the simulator using ```Ctrl-C```  
#### Be sure the simulator is not running before connecting to an actual vehicle

## So you to start up the simulator at a custom location?
You will need to edit the Locations.txt file  
If the simulator is running, stop it and exit using ```Ctrl-C```  
install an editor
```
sudo apt-get install nano
```
Edit the file
```
nano ~/ardupilot/Tools/autotest/locations.txt
```
use the arrow keys to scroll down and add a new entry
```
NAME=Lat,Lon,Altitude,0
```
Lat and Lon are decimal degrees, altitude is meters above sea level. For example, Knoxville would be:
```
Knoxville=35.92310,-84.22301,900,0
```
Save the file and exit
```
Ctrl-o <enter>
Ctrl-x
```
You may now start the simulator specifying the name of your new location
```
cd ~/ardupilot/ArduPlane
../Tools/autotest/sim_vehicle.py -j4 -f quadplane -L <YOUR LOCATION>
```
For example, to start the Knoxville location would be 
```
../Tools/autotest/sim_vehicle.py -j4 -f quadplane -L Knoxville
```





