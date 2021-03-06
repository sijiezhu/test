# llc-platooning-app 

This is a guide for using the platoon control algorithm evaluation platform.

## Getting Started
### Connections

A Linux PC (or virtual machine) needs to be connected to the MK5 nodes via the Ethernet switch.
**Note**: Make sure the devices in the LAN (local-area network) are all interconnected. This can be checked by SSH commands, for example
```
ssh fe80::6e5:48ff:fe20:0064
```
Sometimes you need to specify the network interface name to connect to the MK5 devices.
```
ssh fe80::6e5:48ff:fe20:0064%eth0
```
### Running the demo
Unzip the project zip file. The generated folder includes a folder ("llc-platooning-app") containing the source code and a Makefile realizing test automation.

1. Move the "llc-platooning-app" to "mk5/stack/apps"

2. Create a project directory in "workspace-mk5" and shortcut to the source code.
```
cd workspace-mk5
mkdir PCA_test
cd PCA_test
ln -s ~/mk5/stack/apps/llc-platooning-app ./
```
3. Move the unzipped Makefile to "workspace-mk5/PCA_test/".

4. For successfully executing the example, the *MK5 IP addresses* and the log directory *$(LOG_DIR)* in the *Makefile* might need to be modified.

5. The test automation can be simply started by
```
cd ~/workspace-mk5/PCA_test
make install
```
The script will do the followings:
1. Compile the project and generate the executable.
2. Copy the executable to the MK5 nodes.

Then, to run the test:
```
make run
```
The script will do the followings:
1. Generate the configuration files for each node, according to the parameters specified in the Makefile.
2. Copy the configuration files to the MK5 nodes.
3. Start the application in different nodes in the downstream direction (from the end of the platoon to the leader).
4. Retrieve the logs and the time difference data.

**Note 1**: To run the test, the leader node needs a *.csv* file providing reference acceleration; followers need the *radar_variance.csv* to introduce variance for the virtual radar.

**Note 2**: The log files are automatically generated on different MK5s after each test, named after LogX, where 'X' is the platoon member ID the node is configured. 

**Note 3**: The logging system records the status of the node every 10ms. A row of data is generated every logging cycle. 

**Alternatively**, the executable can be copied to the MK5 devices and started manually, by
```
sudo ./llc-platooning-app -f platoontest.conf
```
**Note 1**: The MK5s share the same executable. The parameters of each node are specified using the configuration files. 

**Note 2**: To conduct tests correctly, the nodes should be started in the downstream direction (from the end to the leader). A two seconds delay is needed for avoiding time synchronization error.

### Syntax of .conf
LLC=$(Radio),$(Channel),$(Power),$(Bitrate),$(MessageInterval),$(vehicleId),$(DesOrAct),$(PLOEG_FREQ),$(TIMEHEADWAY),$(ENGINE_LAG_TIME_CONSTANT)

More detail can be found in the *Configuration file parsing* section of the **application doc**.

## Log Analysis

MATLAB scripts which can analyze the test logs are provided. They need *Log1.csv*, *Log2.csv*, *Log3.csv*, *Log4.csv*, three *time synchronization files*, and *platoontest.conf* to execute
* Visualization.m helps you quickly visualize logs.
* StringstabilityAnalysis.m examines the string stability. 

Add additional notes about how to deploy this on a live system

## Building

You might want to make some changes to the platform. The building of the project requires the CohdaMobility MKx SDK. First, enter the source code directory ("llc-platooning-app"), and type 
```
make mk5
```
The generated executable "llc-platooning-app" and the configuration files should be copied to the MK5 nodes manually by "SCP" command.

**Alternatively**, you can compile the code in the project directory by 
```
make install
```
## Documentation

The specific documentation of the application can be generated by Doxygen, by 
```
cd ~/workspace-mk5/PCA_test/llc-platooning-app
make doc
```
The web version of the document can be found at "~/workspace-mk5/PCA_test/llc-platooning-app/doc/html/index.html".

**Note**: The generation of the .pdf document requires Latex environment. Ignore the Doxygen error if you do not have Latex installed.

## Authors

* **Sijie Zhu** - [Github](https://github.com/sijiezhu)
