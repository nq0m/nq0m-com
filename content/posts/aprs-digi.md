---
title: "Aprs Digi"
date: 2022-06-12T20:21:46-05:00
draft: false
---

***Note: This article was originally published to my blog in 2012.  I include it here as it was by far my most popular post.  As of this date (June 2022) I can make no guarantees on whether these instructions will still work or not.***

***Jeremy, NQ0M***

Recently I picked up a Raspberry Pi computer and have been setting it up to take on the Digi/Igate duties for WI0LA-10.  For those of you who don't know, the R-Pi is a $35 computer based on an ARM processor (similar to the processor used in smartphones).  It includes 2 USB ports, an ethernet jack, a HDMI video output (to plug into your HDTV), and an SD card slot.  The SD card serves in leiu of the hard drive.  This is how I set up my raspberry Pi Digipeater/I-gate.

First, all the various pieces of hardware.  I had everything I needed already on hand, but I'll run down all the parts that make it all work.

- Antenna - [KB9VBR J-Pole antenna](http://www.jpole-antenna.com/antennas/2-meter-amateur-radio-antennas/) - Mounted at about 25 feet AGL on military mast
- Radio - [Kenwood TM-231A](http://www.universal-radio.com/catalog/fm_txvrs/tm231a.html)
- TNC - [Kantronics KPC-3+](http://www.kantronics.com/products/kpc3.html)
- Computer - [Raspberry Pi](http://www.raspberrypi.org/)
- USB-Serial adapter cable - I recommend one using the FTDI Chipset as opposed to a Prolific chip, as experience has shown they do tend to work better.
- Operating System - Raspbian Linux (debian-derived, recommended OS for Raspberry) - I'm running the "wheezy" 2012-08-16 release.
- Other Necessary software - [APRX (digi/igate software)](http://wiki.ham.fi/Aprx.en)

For the purposes of this document, I will assume that you've gotten the radio and TNC talking to one another over serial (minicom is a good tool for this) - Setting that up is a little beyond the scope of my document, and there's already plenty of good information on doing that is available elsewhere.  I'll also assume you've put your KPC+ (or other TNC) into KISS mode.  I will be using the linux built-in AX.25 network support in the building of this system, which requires KISS mode.  If you're starting from scratch, and not using a TNC you already own, you might be better off getting a pre-built TNC-X, as it should work great with this setup, and perhaps allow you to completely eliminate the USB-Serial adapter.  Also, all Linux commands are assumed to be entered as root.

First, we need to get our ax.25 userland applications installed onto the R-Pi:

    apt-get install libax25 libax25-dev ax25-apps ax25-tools

Also, we need to get and comple the aprx digi/igate software - I'm using version 2.04 (svn 474) - there may be a newer version.


    cd /usr/src
    wget http://ham.zmailer.org/oh2mqk/aprx/aprx-2.04.svn474.tar.gz
    tar xvf aprx-2.04.svn474.tar.gz
    cd aprx-2.04.svn474/
    ./configure && make && make install


I like to run the AX.25 mheard daemon as well, so I can easily see what other stations my system is hearing.  For this to work correctly, we need to do the following:

    mkdir /var/ax25/mheard/

The mheard daemon will create a file within this directory storing information about stations heard.  Without creating it first, that process will fail to function.

Prior to setting up the AX.25 stack, you must create the ***/etc/ax25/axports*** file.  It will look like the following:

    packet  WI0LA-10        9600    128     2       144.390MHz 1200bps

The fields are as follows:  interface name, callsign, serial port speed, packet length, tx window, and a description of the interface.

Configuration of the ***/etc/aprx.conf*** file is next.  I usually make a backup of the original config file before editing it:


    cp /etc/aprx.conf /etc/aprx.conf.orig
    vi /etc/aprx.conf


Below is some of the various changes I made to the default aprx.conf.  It's quite well-commented, and documentation is also available on the project's web site listed above.  What I outline below should get you a functional configuration.


    mycall  WI0LA-10
    <aprsis>
    server   noam.aprs2.net
    </aprsis>
    <logging>
    pidfile /var/run/aprx.pid
    rflog /var/log/aprx/aprx-rf.log
    aprxlog /var/log/aprx/aprx.log
    </logging>
    <interface>
    ax25-device $mycall
    tx-ok true
    </interface>
    <beacon>
    beaconmode both
    cycle-size 10m
    beacon symbol "/#" lat "3755.69N" lon "09523.32W" comment "IARC Digi/I-Gate Local Repeater 147.375 179.9Hz"
    </beacon>
    <digipeater>
    transmitter $mycall
    <source>
    source $mycall
    relay-type digipeated
    </source>
    </digipeater>


You should also create the directory for the aprx log files:

    mkdir /var/log/aprx

Last, I create a simple shell script for starting up the AX.25 networking.  More enterprising users could probably integrate this into a bootscript to automatically start on boot, but this seems to work for me.  I call this script ***tnc_start***:

    #!/bin/bash
    #Create the AX.25 interface
    kissattach /dev/ttyUSB0 packet 44.122.1.59
    #Set some optional parameters
    #700ms txdelay (-t)
    #200ms slottime (-s)
    #persist of 32 (-r)
    #100ms txtail (-l)
    #half-duplex mode (-f)
    kissparms -p packet -t 700 -s 200 -r 32 -l 100 -f n
    #Start the aprx digipeater daemon
    aprx
    #Start the mheardd daemon
    mheardd

And, that about does it!

One thing to note - aprx can interface directly with a KISS TNC (without the use of the Linux AX.25 stack), however, if you do utilize the Linux AX.25 support, you can also have other applications accessing the TNC at the same time.  For example, I can have aprx accessing the TNC, with the callsign-ssid of WI0LA-10 (our club call used for the digi & igate), and I could also run Xastir using my personal call KE0MD.  Both can transmit, both can receive, each doing it's own thing, which is a hallmark of Linux.

