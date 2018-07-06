# llc-platooning-app 

This is a guide of using the platoon control algorithm evaluation platform.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Connections

A Linux PC (or virtual machine) needs to be connected to the MK5 nodes via the Ethernet switch.
Note: Making sure the devices in the LAN (local-area network) are all interconnected. This can be checked by SSH commands.


### Running the demo
Unzip the project zip file. The generated folder includes a folder ("llc-platooning-app") containing the source code and a Makefile realizing test automation.

1. Move the "llc-platooning-app" to "mk5/stack/apps"

2. 
```
cd workspace-mk5
mkdir PCA_test
cd PCA_test
ln -s ~/mk5/stack/apps/llc-platooning-app ./
```
3. Move the unzipped Makefile to "workspace-mk5/PCA_test/"

4. For successfully executing the example, the *MK5 IP addresses* and the *Log directory* may need to be modified.

5. The test automation can be simply started by
```
make install
make run
```
Then, the script will do the followings:
1. Compile the project and generate the executable.
2. Copy the executable to the MK5 nodes.
3. Generate the configuration files for each node, according to the parameters in the Makefile.
4. Copy the configuration files to the MK5 nodes.
5. Start the application in different nodes in the downstream direction (from the end of the platoon to the leader).
6. Retrieve the logs and the time difference data.

**Note 1**: The log files are automatically generated on different MK5s after each test, named after LogX, where 'X' is the platoon member ID the node is configured. 

**Note 2**: The logging system records the status of the node every 10ms. A row of data is generated every logging cycle. 

**Alternatively**, the executable can be copied to the MK5 devices and started manually, by
```
sudo ./llc-platooning-app -f platoontest.conf
```
**Note 1**: The MK5s share the same executable. The parameters of each node are specified using the configuration files. 

**Note 2**: To conduct tests correctly, the nodes should be started in the downstream direction (from the end to the leader).

## Log Analysis

MATLAB scripts which can process the test logs are provided.
* Validation3.m processes the timestamps of the log files which were recorded based on synchronized device time.
* Validation4.m examines the string stability. It needs Log1.csv, Log2.csv, Log3.csv and Log4.csv to execute

Add additional notes about how to deploy this on a live system

## Building

You might want to make some changes to the platform. The building of the project requires the CohdaMobility MKx SDK. First enter the source code directory ("llc-platooning-app"), and type 
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
make doc
```
The web virsion of document can be found at "./doc/html/index.html".
The generation of the .pdf document requires Latex environment.
## Authors

* **Sijie Zhu** - [Github](https://github.com/sijiezhu)
