# src: https://linuxize.com/post/how-to-install-asterisk-on-ubuntu-20-04/
sudo apt update
sudo apt install wget build-essential git autoconf subversion pkg-config libtool

cd /usr/src/
sudo git clone -b next git://git.asterisk.org/dahdi/linux dahdi-linux
cd dahdi-linux
sudo make
sudo make install

cd /usr/src/
sudo git clone -b next git://git.asterisk.org/dahdi/tools dahdi-tools
cd dahdi-tools
sudo autoreconf -i
sudo ./configure
sudo make install
sudo make install-config
sudo dahdi_genconf modules

cd /usr/src/
sudo git clone https://gerrit.asterisk.org/libpri libpri
cd libpri
sudo make
sudo make install

cd /usr/src/
sudo git clone -b 18 https://gerrit.asterisk.org/asterisk asterisk-18

cd asterisk-18/

sudo contrib/scripts/get_mp3_source.sh
sudo contrib/scripts/install_prereq install
sudo ./configure
sudo make menuselect
# select: format_mp3, chan_mobile, channel > sip
sudo make -j2
sudo make install

sudo make samples
sudo make config
sudo ldconfig

sudo adduser --system --group --home /var/lib/asterisk --no-create-home --gecos "Asterisk PBX" asterisk
# -- ^^^ done

sudo nano /etc/default/asterisk
# now uncomment two lines i.e., 

```txt
AST_USER="asterisk"
AST_GROUP="asterisk"
```

sudo usermod -a -G dialout,audio asterisk
sudo chown -R asterisk: /var/{lib,log,run,spool}/asterisk /usr/lib/asterisk /etc/asterisk
sudo chmod -R 750 /var/{lib,log,run,spool}/asterisk /usr/lib/asterisk /etc/asterisk
sudo systemctl start asterisk

sudo asterisk -vvvr

sudo ufw allow 5060/udp
sudo ufw allow 10000:20000/udp


-- for bluetooth setup ---
sudo apt-get install bluetooth


-- IMPORTANT: You need to add bluetooth form vir-manager's menu via "Add Hardware" button and then you would be able to add using "USB Host Device" option. Yo!


---- for editing chan_mobile.conf file ---
sudo vi /etc/asterisk/chan_mobile.conf


# hci0    48:E2:44:B2:3F:C2
