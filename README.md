This is a userspace application which accesses the adc via /dev/iio in continuous sampling mode.

The main work has already been done by TI for the am335x dev kit. This repo follows the instructions
given on http://processors.wiki.ti.com/index.php/AM335x_ADC_Driver%27s_Guide

the generic_buffer.c file in the main folder is already patched.

Originals are given in the original folder.

The originals are based on 3.2 as the driver was forward ported from 3.2. 
Although, I dont think there is any difference between the userspace access methods.

The basic flow is as follows.

The application scans the scan_elements folder in /dev/iio/devices/iio:deviceX/scan_elements for enabled channels.

Creates a data structure.

Sets the buffer size. Enables the buffer. And reads from the dev file for the driver.

The original generic_buffer.c is generic and can ideally access every IIO based driver. Which is the reason IIO was made.
To provide a common interface system in the middle.

The am335x one is a bit different as it doesn't use triggers. So the IIO trigger functionality has to be bypassed. These are what the patch does.

To run, follow the instructions on the http://processors.wiki.ti.com/index.php/AM335x_ADC_Driver%27s_Guide

They are reproduced below with one modification. The 3.11 adc driver name has changed. Now it is known as "TI-am335x-adc"

 Sample Application

The source code is located under kernel sources "drivers/staging/iio/Documentation/generic_buffer.c". Since our driver is not trigger based we need to modify this application to bypass the trigger detection. Please apply patch Media:Generic_buffer.patch on top of the application generic_buffer.c in order to bypass the trigger conditions.

How to compile:

arm-arago-linux-gnueabi-gcc --static generic_buffer.c -o generic_buffer

or

<path_to_cross-compiler/cross-compiler-prefix->-gcc --static generic_buffer.c -o generic_buffer

Then copy the generic_buffer program on your target board and follow below sequence -

Enable the channels:

root@arago-armv7:~# echo 1 > /sys/bus/iio/devices/iio\:device0/scan_elements/in_voltage4_en

Check the mode:

root@arago-armv7:~# cat /sys/bus/iio/devices/iio\:device0/mode
oneshot

root@arago-armv7:~# echo continuous > /sys/bus/iio/devices/iio\:device0/mode

Finally, the generic_buffer application does all the "enable" and "disable" actions for you. You will only need to specify the IIO driver. Application takes two arguments, buffer length to use (256 in this example) the default value is 128 and the number of iterations you want to run (3 in this example).

root@arago-armv7:~# ./generic_buffer -n TI-am335x-adc -l 256 -c 3

