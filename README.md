NI_USB-6501
===========

<i>python driver for NI USB-6501</i>

The ni_usb_6501 is a digital IO module for National Instrument's NI USB-6501 adapter.
Unfortunately NI's Linux driver is excessively large (>60MB), difficult to install
and doesn't offer off-the-shelf support for python.

This python driver is based on Marc Schutz's pioneer work on c driver
(https://github.com/schuetzm/ni-usb-6501)

INSTALLATION

1. Install using pip: 
```bash
pip install ni_usb_6501@git+https://github.com/shengzhiheng/NI_USB-6501
```

2. Change the permissions of the USB device node by creating a udev rule.
e.g. add the following line (and file) to a file in /etc/udev/rules.d/usb.rules

  SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", MODE="0664", GROUP="usbusers"

  This will set the owner of the device node to root:usbusers rather than root:root
  After that add user to the usbusers group for enabling access to the device.

  adduser _<user>_ usbusers

  (Make sure you have group usbusers)

...and you are good to go.

TODO
- Counter operations

EXAMPLE

```python
    dev = get_adapter()

    if not dev:
        raise Exception("No device found")

    dev.set_io_mode(0b11111111, 0b11111111, 0b00000000, 0b11111111, 0b00000000, 0b00000000)

    dev.write_port(0, 0b11001100)
    dev.write_port(1, 0b10101010)
    dev.change_pin_io(0, 1, 1, 0)
    dev.write_pin(0, 1, 1)

    print(bin(dev.read_port(2)))

    dev.release_interface()     # clean exit, allows direct reuse without to replug the ni6501
    del dev
```
