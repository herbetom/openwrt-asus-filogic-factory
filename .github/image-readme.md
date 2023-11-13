# Asus Filogic Factory Image

## Supported devices

 - TUF-AX4200


## Instructions

1. Download the Factory-Initramfs image.
   Place it into a web-server directory root as openwrt.bin.
   
   ```
   $ mkdir -p /tmp/openwrt-webroot
   $ cp <openwrt-factory-initramfs> /tmp/openwrt-webroot/openwrt.bin
   $ python3 -m http.server -d /tmp/openwrt-webroot 8080
   ```
   
2. Open the router configuration page. Complete the first-time setup wizard.
   Enable SSH Access (with Password-Login) on the Router.
   Find this Setting at Administration --> System
   
    SSH-Port 22

3. Connect to the device using ssh
   ```
   $ ssh admin@192.168.50.1
   ```

4. Modify the `/etc/hosts` file using `vi` on the device.
   Add an entry to the bottom of the file that resolves to the computer serving the factory-initramfs image.
   Name the DNS name `openwrtimage.lan`

5. Transfer the OpenWrt factory-initramfs to the device by downloading it from the http server using wget
   ```
   wget -O /tmp/openwrt.bin http://openwrtimage.lan:8080/openwrt.bin
   ```

7. Erase the linux-partition

   ```
   $ mtd-erase -d linux
   ```
   

8. Write OpenWrt to the `linux` partition

   ```
   $ mtd-write -i /tmp/openwrt.bin -d linux
   ```

9. Power-Cycle your router.

10. Transfer the OpenWrt sysupgrade image to the router using `scp`.

    ```
    $ scp -O /path/to/<openwrt-sysupgrade.bin> root@192.168.1.1:/tmp
    ```

11. Install OpenWrt persistently using `sysupgrade`.

    ```
    $ sysupgrade -n /tmp/<openwrt-sysupgrade.bin>
    ```
