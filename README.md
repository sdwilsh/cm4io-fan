# cm4io-fan

kernel module and device tree overlay to add support for the EMC2301 fan controller on the Raspberry Pi Compute Module 4 IO Board.

*Works with 5.10.y 64-bit kernels only.*

Uses Traverse Technologies' EMC2301 [hwmon driver](https://gitlab.traverse.com.au/ls1088firmware/traverse-sensors) for their [ten64](https://www.crowdsupply.com/traverse-technologies/ten64) board, which you should definitely check out because it's awesome.

## Usage
1. Install dkms if you haven't already:
```
sudo apt install dkms
```
2. Download the latest release from the releases page, and install it:
```
sudo apt install ./cm4io-fan.dpkg
```
3. Add this line to your /boot/config.txt (adjust the rpm values for your specific fan, defaults are 3500 / 5500) and reboot.
   See below for more config options.
```
dtoverlay=cm4io-fan,minrpm=1000,maxrpm=5000
```

## Config options
The device tree overlay has a few options, here's the equivalent of a `/boot/overlays/README` info section:

```
Name:   cm4io-fan
Info:   Raspberry Pi Compute Module 4 IO Board fan controller
Load:   dtoverlay=cm4io-fan,<param>[=<val>]
Params: minrpm              RPM target for the fan when the SoC is below 
                            mintemp (default 3500)
        maxrpm              RPM target for the fan when the SoC is above
                            maxtemp (default 5500)
        midtemp             Temperature (in millicelcius) at which the fan
                            begins to speed up (default 50000)
        midtemp_hyst        Temperature delta (in millicelcius) below mintemp
                            at which the fan will drop to minrpm (default 2000)
        maxtemp             Temperature (in millicelcius) at which the fan 
                            will be held at maxrpm (default 70000)
        maxtemp_hyst        Temperature delta (in millicelcius) below maxtemp
                            at which the fan begins to slow down (default 2000)
```

## Usage without the .deb

1. Download the latest source .tar.gz from the releases page
2. Untar it to `/usr/src/cm4io-fan-<version>` and run the dkms install:
```
tar -xzvf cm4io-fan-0.1.0.tar.gz -C /usr/src/
sudo dkms install cm4io-fan/0.1.0
```

Add the config.txt line and reboot.

### many thanks to:
- [taudac/taudac-driver-dkms](https://github.com/taudac/taudac-driver-dkms) for helping me work out how the shit DKMS works
- [Traverse Technologies](https://traverse.com.au) for the [kernel module](https://gitlab.traverse.com.au/ls1088firmware/traverse-sensors)