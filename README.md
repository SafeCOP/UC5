# SAFECOP UC5: SIMULATION TOOL

This is a repository to setup the simulation environment for the *SAFECOP project*.

The simulation setup is composed by three parts:
1. SUMO (Simulation of Urban MObility http://sumo.dlr.de), an open source traffic simulation suite.
2. OMNeT++ (https://www.omnetpp.org), an extensible simulation library and framework for building network simulators. It includes an IDE based on Eclipse and it is released under the ACADEMIC PUBLIC LICENSE.
3. Veins (Vehicles in Network Simulation https://veins.car2x.org), an open source framework for running vehicular network simulations. It is based on OMNeT++ and SUMO. The SAFECOP project primarily uses and extends veins, providing smart policies for vehicles and traffic lights, along with scenarios where they can be tested.

## Installation

This repository provides a set of scripts to setup a virtual machine where all the development and testing of the SAFECOP simulation environment can be done.

The setup requires two steps: 1) creation of the VM; 2) execution of the setup scripts inside the newly created VM.
Before starting, make sure to install all the prerequisites and that your network connection is stable.

#### Prerequisites

The creation of the VM is based on Vagrant (https://www.vagrantup.com/).
In order to create the VM you first have to install Vagrant. Most GNU/linux distributions provide packages, but you can always download and install the latest version from [here](https://www.vagrantup.com/docs/installation/).

Vagrant will be used to setup a virtual machine for VirtualBox, so you need to install VirtualBox as well before starting, because Vagrant will invoke some VirtualBox commands under the hood.
Most GNU/linux distributions provide packages, but you can [download and install the latest version](https://www.virtualbox.org/wiki/Downloads).

#### Creation of the VM

After you have installed Vagrant and VirtualBox you can start building the VM. Simply clone this repository and run:

```shell
cd $YOUR_PATH/safecop/vagrant
./script.sh
```
The script will create an Ubuntu 16.04 VM, with all the libraries necessary for SAFECOP already installed.

#### Setting up the VM

After Vagrant has finished setting up the VM, open VirtualBox and start the newly created `safecop` machine.
Log in with the user named `ubuntu`, password `safecop`.

##### Optional step - Install VirtualBox Guest Additions
If you want at this step you can install the VirtualBox guest additions from the graphical interface. I tried to find a reliable way to do that from Vagrant but I always ended up with unstable results, breaking the GUI of the VM.

##### Setup the SAFECOP simulation environment
Then open a terminal and run:

```shell
git config --global user.name "Your Name"
git config --global user.email "your.email@mail.com"
```

This step is necessary because you will now run a setup script which uses `git`.
After you have done this, simply run:

```shell
./setup.sh
```

and wait for a while.

The script will clone three repositories in the home directory: one for SUMO, one for OMNeT++, and one for Veins.
The SAFECOP simulation environment is tested with SUMO verion `0.29.0`, OMNeT++ verions `5.0`, and Veins version `4.5`.
The Veins repositories already includes the modifications and the extensions developed for the SAFECOP project.

## Running the simulation

You are now ready to run the simulations.

In order to do that, you have to do the following:

1.Install SUMO system-wide:

```shell
cd sumo/sumo
sudo make install
```

2.Start SUMO from Veins, so that it listens for requests from OMNeT++ in the backround. To do that run the following commands:

```shell
cd veins
./sumo-launchd.py -vv -c sumo-gui
```

3.In another terminal open the OMNeT++ IDE:

```shell
cd omnetpp-5.0
source setenv
omnetpp
```

The last command will open Eclipse with the OMNeT++ plugin.

4.From the OMNeT++ IDE import the veins directory

5.Run the simulation. This will open two windows: the first for the OMNeT++ network simulation, the second for the SUMO vehicle simulation. The second will not show until you hit 'Play' in the OMNeT++ simulation window.
