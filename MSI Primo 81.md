MSI Primo 81

# status

u-boot --- ok
kernel --- ok
usb --- ng (Linux 4.7であればokだったが...)
display -- partical

# spec

Allwinner A31s

# dts

https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/log/arch/arm/boot/dts/sun6i-a31s-primo81.dts              

# MIPI DSIを有効化する? (Linux 4.10-)

https://kernelnewbies.org/Linux_4.10

ただしstab

# USB Host が動かないのをどう対処すべきか。 (Linux 4.9)


https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/tree/arch/arm/boot/dts/axp22x.dtsi

下記の disable を解除したほうがいいと思うが、定義はしたのか? --> していない

        usb_power_supply: usb_power_supply {
                compatible = "x-powers,axp221-usb-power-supply";
                status = "disabled";
        };

&usb_power_supply {
        status = "okey";
};


https://github.com/torvalds/linux/commits/master/arch/arm/boot/dts/axp22x.dtsi

---> やはり動かない


MFD は有効になっているか?
http://cateee.net/lkddb/web-lkddb/AXP20X_POWER.html

lkddb of "" "" "x-powers,axp221-usb-power-supply" : CONFIG_AXP20X_POWER CONFIG_POWER_SUPPLY : drivers/power/supply/axp20x_usb_power.c # in 4.9–4.10, 4.11-rc+HEAD


だめなら 4.10 も試したい


# USB OTGがいけるか検討 (Linux 4.9)

この一文がある

&usb_otg {
        /* otg support requires support for AXP221 usb-power-supply and GPIO */
        dr_mode = "host";
        status = "okay";
};

https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/tree/arch/arm/boot/dts/axp22x.dtsi

こちらには既に定義がある

        usb_power_supply: usb_power_supply {
                compatible = "x-powers,axp221-usb-power-supply";
                status = "disabled";
        };

裏のコードもある

https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/tree/drivers/power/supply/axp20x_usb_power.c?id=refs/tags/v4.10.5

あと GPIO は? -> まだない? (AXP209 にはある。)

