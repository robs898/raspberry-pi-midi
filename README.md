# raspberry-pi-midi
Play MIDI via raspberry pi

## Intro
Do you have a keyboard that only outputs MIDI (no headphone audio)?

Do you need a cheap/small MIDI converter?

You can use this guide to convert the keyboard MIDI output to sound via a raspberry pi.

Some example setups:
```
MIDI keyboard -> USB -> Raspberry Pi -> headphone jack -> headphones
MIDI keyboard -> USB -> Raspberry Pi -> HDMI -> TV speakers
```

This worked using a Raspberry Pi Model B Plus Rev 1.2, M-Audio Keystation 88 and some Sony headphones.

## Setup
Starting from fresh Raspbain Stretch Lite build (https://www.raspberrypi.org/downloads/raspbian/)

1. Update and install
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install jackd2
sudo apt-get install fluidsynth fluid-soundfont-gm
```

2. Write the following to `/etc/dbus-1/system-local.conf`
```
<?xml version="1.0"?> <!--*-nxml-*-->
<!DOCTYPE busconfig PUBLIC "-//freedesktop//DTD D-BUS Bus Configuration 1.0//EN"
        "http://www.freedesktop.org/standards/dbus/1.0/busconfig.dtd">

<busconfig>

        <policy context="default">
                <allow own="org.freedesktop.ReserveDevice1.Audio0"/>
                <allow own="org.freedesktop.ReserveDevice1.Audio1"/>
        </policy>

</busconfig>
```

## Run
Execute script `audio` from this repo. You can move it to `/bin/audio` for ease of use.

Change the `aconnect 20:0 128:0` line to match your MIDI input/output. `128:0` is your fluidsynth out and for me `20:0` was my MIDI keyboard on USB input. Check your inputs with `aconnect -i` and outputs with `aconnect -o`.

## Auto-run
If you want it to auto run on boot of your pi:

1. Use raspi-config to auto login:

`sudo raspi-config`

`Boot Options` -> `Console Autologin`

2. Add the `audio` script to the .bashrc file of the `pi` user:

`echo 'audio' >> ~/.bashrc`

## Troubleshooting
- Switch audio to headphones from hdmi: https://www.raspberrypi.org/documentation/configuration/audio-config.md
- Pulseaudio conflicts? Check Teds' MIDI guide in sources

## Sources
- http://tedfelix.com/linux/linux-midi.html
- https://wiki.linuxaudio.org/wiki/raspberrypi



