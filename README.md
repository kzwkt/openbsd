https://www.openbsd.org/faq/faq4.html#Download

https://cdn.openbsd.org/pub/OpenBSD/7.9/amd64/install79.img

dd if=install79.img of=/dev/sdb bs=1M

cfdisk

make partition and change its type to Openbsd data

boot usb, choose install to openbsd area / partition

mount sets, sd1

select partition b

install all sets

reboot

usb tethering via android

dmesg shows urndis0 device

set /etc/hostname.urndis0

sh /etc/netstart urndis0

inet autoconf

fw_update

reboot

ifconfig shows iwm0

 /etc/hostname.iwm0
 '''
join SSID wpakey PASS
inet autoconf
inet6 autoconf
'''
sh /etc/netstart iwm0

pkg_add nano xclip fastfetch firefox  vulkan-tools  intel-media-driver libva-utils zathura-pdf-mupdf mpv nnn 





