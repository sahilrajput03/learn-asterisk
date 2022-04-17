# abandoned - setup -

1. too much broken things with bluetooth
2. - tired of 2 days configuration. and youtubing..

```bash
sudo systemctl restart asterisk

vi.chan_mobile
# vi.chan_mobile is aliased to `sudo nvim /etc/asterisk/chan_mobile.conf'

asteriskrv
# asteriskrv is aliased to `sudo asterisk -rvvvv'

hcitool dev # to get our pc's bluetooth mac address.
hcitool scan # scan nearby bluetooth devices
sudo cp ~/chan_mobile.conf /etc/asterisk/chan_mobile.conf
```

# Theory

Soft Phone is an application with which we can connect to asterisk server so multiple people connected to asterisk server via soft phone application they would be able to call each other.

PRI Cards: Pri cards are offered by netowrk providers and they come variants like 16 lines, etc. A 16 lines pricard will be able to call 16 people at a time. A pricard has same phone number for all 16 lines.

GSM Gateways: GSM Gateway is similar to PRI cards. GSM Gateways comes with 4, 8, 16, 32 ports. So we can use that many no. of sims to call via asterisk. GSM Gateway has advantage over pric cards bcoz GSM Gateways can have different numbers i.e., each sim card.

# Installation via ANTICCONFUSION

```bash
asterisk -rvvv
> sip show peers



# while doing `module load chan_` i get below error:
Error loading module 'chan_': /usr/lib/asterisk/modules/chan_.so: c
```

## insatllation

```bash
# clone from aur
cd src/asterisk-19.3.1

make uninstall # if you have installed earlier please do..
./configure
make menuconfig
make
sudo make install
# src: https://askubuntu.com/a/1005350/702911

sudo systemctl status asterisk
sudo systemctl start asterisk
```

```
module load chan_mobile.so
mobile search
mobile show devices # this will show any connected device.

#hcitool scan
6C:24:A6:9C:A6:4E (vivo 1915)
#hcitool dev
#(my laptop) address=48:E2:44:B2:3F:C2


sudo rfcomm bind 3 6C:24:A6:9C:A6:4E # addressOfMyMobile with its port (we saw when we did `mobile search`)
ls -l /dev/rf*
core restart now
mobile show devices
```
