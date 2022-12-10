# APU2 LEDs
Enable LEDs on PC Engine APU2 boards with Debian 11 (Power, HDD activity, network link and network activity)

## Load kernel module on boot

Create a file in `/etc/modules-load.d/` (for example `/etc/modules-load.d/leds.conf`) with the following content:
```
ledtrig-disk
ledtrig-netdev
```

## LED scripts

Create two scripts to enable and disable all LEDs.

Enable script:
``` bash
echo "none" > /sys/class/leds/apu2\:green\:led1/trigger
echo "1" > /sys/class/leds/apu2\:green\:led1/brightness
echo "disk-activity" > /sys/class/leds/apu2\:green\:led2/trigger
echo "netdev" > /sys/class/leds/apu2\:green\:led3/trigger
echo "1" > /sys/class/leds/apu2\:green\:led3/link
echo "1" > /sys/class/leds/apu2\:green\:led3/rx
echo "1" > /sys/class/leds/apu2\:green\:led3/tx
echo "enp1s0" > /sys/class/leds/apu2\:green\:led3/device_name
```

Disable script:
``` bash
echo "0" > /sys/class/leds/apu2\:green\:led1/brightness
echo "none" > /sys/class/leds/apu2\:green\:led2/trigger
echo "none" > /sys/class/leds/apu2\:green\:led3/trigger
```

Remember to make the scripts executable with `chmod +x my_script.sh`!

## Automate the LEDs

In order to enable the leds at boot and disable the leds at night I created some entries in crontab with `sudo crontab -e`.

```
0 23 * * * /path_to_my_script/apu_leds_off.sh
0 6 * * * /path_to_my_script/apu_leds_on.sh
@reboot /path_to_my_script/apu_leds_on.sh
```

## ToDo
 - Add scripts to the repo
 - Write a install script
 
