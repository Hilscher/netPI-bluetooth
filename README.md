## Bluetooth

Made for [netPI](https://www.netiot.com/netpi/), the Raspberry Pi 3B Architecture based industrial suited Open Edge Connectivity Ecosystem

### Debian with SSH, dbus and latest bluez bluetooth stack 

The image provided hereunder deploys a container with latest bluetooth protocol stack to enable bluetooth communications in a container.

Base of this image builds [debian](https://www.balena.io/docs/reference/base-images/base-images/) with enabled [SSH](https://en.wikipedia.org/wiki/Secure_Shell), a source code compiled bluez stack [bluez](http://www.bluez.org/) and [firmware](https://github.com/OpenELEC/misc-firmware/tree/master/firmware/brcm) for the onboard BCM bluetooth chip BCM43438.

#### Container prerequisites

##### Host network

The container needs the Docker "Host" network stack to be shared with the container. 

Hint: Using this mode makes port mapping unnecessary since all the container's used ports are exposed to the host. This is why the container's used SSH server port `22` is getting available on the host without a discrete port mapping.

##### Privileged mode

Only the privileged mode option lifts the enforced container limitations to allow usage of bluetooth in a container.

##### Host device

To grant access to the BCM chip the `/dev/ttyAMA0` host device needs to be exposed to the container.

To prevent the container from failing to load the BCM chip with firmware(when restarted), the BCM chip is physically reset by the container each time it is started. To grant access to the reset logic the `/dev/vcio` host device needs to be exposed to the container.

#### Getting started

STEP 1. Open netPI's landing page under `https://<netpi's ip address>`.

STEP 2. Click the Docker tile to open the [Portainer.io](http://portainer.io/) Docker management user interface.

STEP 3. Enter the following parameters under **Containers > Add Container**

* **Image**: `hilschernetpi/netpi-bluetooth`

* **Network > Network**: `Host`

* **Restart policy"** : `always`

* **Runtime > Devices > add device**: `Host "/dev/ttyAMA0" -> Container "/dev/ttyAMA0"` and `Host "/dev/vcio" -> Container "/dev/vcio"`

* **Runtime > Privileged mode** : `On`

STEP 4. Press the button **Actions > Start/Deploy container**

Pulling the image may take a while (5-10mins). Sometimes it takes so long that a time out is indicated. In this case repeat the **Actions > Start/Deploy container** action.

#### Accessing

The container starts the SSH server and the bluetooth device hci0 automatically.

Login to it with an SSH client such as [putty](http://www.putty.org/) using netPI's IP address at port `22`. Use the credentials `root` as user and `root` as password when asked and you are logged in as root.

Use bluez tools such as bluetoothctl, hciconfig, hcitool as usual. For a simple test call [bluetoothctrl](https://wiki.archlinux.org/ind#### Accessing

The container starts the SSH service automatically when started.

Login to it with an SSH client such as [putty](http://www.putty.org/) using netPI's IP address at your mapped port. Use the credentials `root` as user and `root` as password when asked and you are logged in as root user `root`.

Use Debian shell commands as usual.

#### Automated build

The project complies with the scripting based [Dockerfile](https://docs.docker.com/engine/reference/builder/) method to build the image output file. Using this method is a precondition for an [automated](https://docs.docker.com/docker-hub/builds/) web based build process on DockerHub platform.

DockerHub web platform is x86 CPU based, but an ARM CPU coded output file is needed for Raspberry systems. This is why the Dockerfile includes the [balena](https://balena.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/) steps.

#### License

View the license information for the software in the project. As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).
As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.ex.php/bluetooth) to start the bluetooth interactive command utility. Input `scan on` to discover nearby bluetooth devices.

[![N|Solid](http://www.hilscher.com/fileadmin/templates/doctima_2013/resources/Images/logo_hilscher.png)](http://www.hilscher.com)  Hilscher Gesellschaft fuer Systemautomation mbH  www.hilscher.com
