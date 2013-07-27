This is a userspace application which accesses the adc via /dev/iio in continuous sampling mode.

The application scans the scan_elements folder in /dev/iio/devices/iio:deviceX/scan_elements for enabled channels.

Creates a data structure.

Sets the buffer size. Enables the buffer. And reads from the dev file for the driver.

The source code is located under kernel sources "drivers/staging/iio/Documentation/generic_buffer.c".

How to compile:

arm-arago-linux-gnueabi-gcc --static generic_buffer.c -o generic_buffer

or

<path_to_cross-compiler/cross-compiler-prefix->-gcc --static generic_buffer.c -o generic_buffer

Check the blog post for more details on how to use this application
http://beagleboard-gsoc13.blogspot.com/2013/07/sampling-analogue-signals-using-adc-on.html
