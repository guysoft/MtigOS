MtigOS
======

An out of the box `Raspberry Pi <http://www.raspberrypi.org/>`_ "Raspberry Pi OS" distro that lets you receive, store and graph sensor information from ESP8266 chips.

It uses and MTIG stack: `Mosquitto <https://mosquitto.org>`_, `Telegraf <https://www.influxdata.com/time-series-platform/telegraf/>`_, `InfluxDB <https://www.influxdata.com>`_ and  `Grafana <https://grafana.com/>`_ which are all pre-configured to work together. They automatically update using Docker.


Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/MtigOS>`_

How to use it?
--------------
`Blog post with step-by-step instructions here <https://guysoft.wordpress.com/mtigos/>`_

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``mtigos-wpa-supplicant.txt`` at the root of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card
#. Hostname is ``mtigos`` (not ``raspberrypi`` as usual), username: ``pi`` and inital password is: ``raspberry``
#. After a few mintues you should be able to access ``http://mtigos.local/``
#. You can change the settings of the MTIG stack in the files located at ``/boot/docker-compose/mtig/``
#. Flash an ESP8266 with the `sketches here <https://github.com/guysoft/Mqtt_Wifi_manager>`_. Full blog post with schematics here.


Requirements
------------
* Raspberrypi all versions should work
* 2A power supply
* Esp8266 with one of the supported sensors

Features
--------

* `Mosquitto <https://mosquitto.org>`_, `Telegraf <https://www.influxdata.com/time-series-platform/telegraf/>`_, `InfluxDB <https://www.influxdata.com>`_ and  `Grafana <https://grafana.com/>`_ which are all pre-configured to work together.
* Pre-installed dashboards for grafana to display Temperature, moisture and PH. 


Developing
----------

Requirements
~~~~~~~~~~~~

#. Docker or Vagrant, docker recommended
#. Docker-compose - recommended if using docker build method, instructions assume you have it
#. Downloaded `Raspbian Lite <https://downloads.raspberrypi.org/raspbian_lite/images/>`_ image.
#. Root privileges for chroot
#. Bash
#. sudo (the script itself calls it, running as root without sudo won't work)

Build MtigOS
~~~~~~~~~~~~

MtigOS can be built using docker running either on an intel or RaspberryPi (supported ones listed).
Build requires about 4.5 GB of free space available.
You can build it assuming you already have docker and docker-compose installed issuing the following commands::

    
    git clone https://github.com/guysoft/MtigOS.git
    cd MtigOS/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspios_lite_armhf_latest'
    cd ..
    sudo docker-compose up -d
    sudo docker exec -it mtigos-build build
    
Building MtigOS Variants
~~~~~~~~~~~~~~~~~~~~~~~~

MtigOS supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo docker exec -it mtigos-build build [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build MtigOS in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd MtigOS/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd MtigOS/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd MtigOS/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building MtigOS, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
