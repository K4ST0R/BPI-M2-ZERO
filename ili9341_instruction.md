# Connecting a ili9341 screen to the BPI-M2-ZERO

## OS
Armbian Kernel 6.0.9

Tested on : https://forum.banana-pi.org/t/bananapi-bpi-m2-zero-new-image-release-armbian-bullseye/14448

## Connection diagram
```
LCD pin - BPI-M2Z pin
    VCC - 2 (5v)
    GND - 6 (GND)
  RESET - 31 (PA8)
     DC - 33 (PA9)
    LED - 7  (PA6)
    CLK - 23 (SPI0_CLK)
     CS - 24 (SPI0_CS)(PC3)
    DIN - 19 (SPI0_MOSI)(PC0)
     DO - 21 (SPI0_MISO)(PC1)
```

## Add overlay

- As root create /root/ili9341bpiz.dts file
```
# nano /root/ili9341bpiz.dts
```

- Copy content of overlay/spi0_ili9341_overlay.txt

- Buil and install with
```
# armbian-add-overlay ili9341bpiz.dts
```

## Set graphic

- Create file /etc/modules-load.d/98-fbtft.conf 
```
# nano /etc/modules-load.d/98-fbtft.conf
```
- Copy in it : 
```
fbtft
fbtft_device
```

- Create file /usr/share/X11/xorg.conf.d/99-fbdev.conf
- Copy in it :
```
Section "Device"  
Identifier "piscreen"
Driver "fbdev"
Option "fbdev" "/dev/fb0"
EndSection
```

## Change console font (optional)

```
# sudo dpkg-reconfigure console-setup
```

- Select options :
```
Encoding to use on the console: <UTF-8>
Character set to support: <Guess optimal character set>
Font for the console: Terminus (default is VGA)
Font size: 6x12 (framebuffer only)
```

## REBOOT!

## Thanks
Special thanks to alleiny for his guide : 
https://forum.banana-pi.org/t/waveshare-3-5-inch-display-touch-input/11733/4