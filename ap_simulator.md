## How to setup an Ardupilot simulator

These instructions assuming that you are either running Ubuntu 18/20 (within Windows WSL 1 is acceptable)

Get prereqs
```
sudo apt-get update
sudo apt-get install git
sudo apt-get install gitk git-gui
```
Clone repo
```
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git checkout Plane-4.3.1
```
Setup up build environment
```
Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
```
Run the simulator, which will build the code the first time
```
cd ArduPlane
../Tools/autotest/sim_vehicle.py -j4 -f quadplane
```
Once the simulator is build up and running (it will take several minutes the first time you do it), you will need to set 2 key parameters by typing the following:
```
param set Q_MAV_TYPE 20
param set BRD_SERIAL_NUM 4126
```
At this point, you have ArduPlane 4.1.3 running and it will by default open a port on localhost:14550. Run the GCS and it should automatically connect to the simulator. Windows may ask you for permission to open the port, be sure and ALLOW the connection.



