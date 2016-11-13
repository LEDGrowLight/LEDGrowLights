# LEDGrowLights

This project documents my attempts to automate [LED grow lights](https://en.wikipedia.org/wiki/Grow_light) using a raspberry pi controller. I am using a GPIO board and simple bash scripts in order to automate the process. The best led grow lights have automation systems built in, but this script should allow you to automate system regardless of whether they have a controller or not.

This whole system is especially important with LED Grow lights. If you're spending tons of money on the highest quality [growgear](http://www.growgear.net), you don't want to blow your operation just because you forgot to flip the lights off one night.

By the same token, you don't want to drop all your money on a high end LED grow light just to get a simple timer. Instead, you can pickup something cheap and use a $20 raspberry pi to automate the whole system.

If you know your way around a command line, the process is easy to automate with the GPIO class.

#Turning on your led grow light:
```
on script [on.sh]
!/bin/bash
echo Exporting pin $1.
echo $1 > /sys/class/gpio/export
echo Setting direction to out.
echo out > /sys/class/gpio/gpio$1/direction
echo Setting pin high.
echo 1 > /sys/class/gpio/gpio$1/value
```

Chmod the file to be executable, then execute it against a variable identifying the pin you wanted to turn on. For a single light, you could connect the controller across a single pin and then run "./on.sh 1" to execute the first pin. You can simplify this with a script, but for now we're going to leave it variable controlled. If you have more than one LED grow light, this would allow you to control them seperately.

you can also stack grow lights to specific outputs. For example, you could have a bank of 5 lights on an 18/6 cycle and another bank on a 12/12. That's the main reason you don't want to hard code the pin into the script.

#turning it off:

```
#!/bin/bash
echo Setting pin low.
echo 0 > /sys/class/gpio/gpio$1/value
echo Unexporting pin $1
echo $1 > /sys/class/gpio/unexport
```
once again, chmod the script +x so it's executable, then run ./off.sh (or whatever you called it + your desired pin)

At this point, you can run cron jobs to schedule your light cycles. You can set up another bash script to set all of your variables independantly, you can quickly just hash out the script line by line. The entire process only takes a few cycles, so it really makes no difference.

If anyone has a high end LED grow light and wants to contribute a script for the onboard controller, we'd be happy to see it. If not, i'll update this project at a later date with a system to run the controllers.
