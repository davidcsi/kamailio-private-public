# kamailio-public-private
Configuration for a Kamailio in a Public/Private network.

Installing RTPENGINE

```
git clone https://github.com/sipwise/rtpengine.git
cd rtpengine
```

INSTALL 
```
apt-get install -y \
debhelper \
default-libmysqlclient-dev  \
gperf  \
iptables-dev \
libavcodec-dev \
libavfilter-dev \
libavformat-dev \
libavutil-dev \
libbencode-perl  \
libcrypt-openssl-rsa-perl  \
libcrypt-rijndael-perl  \
libcurl4-openssl-dev  \
libdigest-crc-perl  \
libdigest-hmac-perl  \
libevent-dev \
libglib2.0-dev \
libhiredis-dev  \
libio-multiplex-perl  \
libio-socket-inet6-perl  \
libiptc-dev  \
libjson-glib-dev  \
libnet-interface-perl  \
libpcre3-dev  \
libsocket6-perl  \
libssl-dev \
libswresample-dev \
libsystemd-dev  \
libxmlrpc-core-c3-dev \
markdown  \
zlib1g-dev \
dkms \
module-assistant && \
libnfsidmap2 && \
libtirpc1 && \
rpcbind && \
keyutils && \
nfs-common
```

INSTALL

Getting g729 From https://deb.sipwise.com/spce/mr6.2.1/pool/main/b/bcg729/
```
wget https://deb.sipwise.com/spce/mr6.2.1/pool/main/b/bcg729/libbcg729-0_1.0.4+git20180222-0.1~bpo9+1_amd64.deb
sudo dpkg -i libbcg729-0_1.0.4+git20180222-0.1~bpo9+1_amd64.deb

wget https://deb.sipwise.com/spce/mr6.2.1/pool/main/b/bcg729/libbcg729-dev_1.0.4+git20180222-0.1~bpo9+1_amd64.deb
sudo dpkg -i libbcg729-dev_1.0.4+git20180222-0.1~bpo9+1_amd64.deb
```

CHECK ALL IS OK WITH 

```
cd rtpengine
dpkg-checkbuilddeps
dpkg-buildpackage
```
You should get no errors back.

```
cd ..

dpkg -i ngcp-rtpengine-kernel-dkms_7*.deb && \
dpkg -i ngcp-rtpengine-utils_7*.deb && \
dpkg -i ngcp-rtpengine-daemon_7*.deb && \
dpkg -i ngcp-rtpengine-iptables_7*.deb & \
dpkg -i ngcp-rtpengine-recording-daemon_7*.deb & \
dpkg -i ngcp-rtpengine_7*.deb & \
dpkg -i ngcp-rtpengine-kernel-source_7*.deb
```

AND SET VALUES SO IT STARTS ON BOOT 

`vim /etc/default/ngcp-rtpengine-daemon`

### Configure RTPEngine
Edit `/etc/rtpengine/rtpengine.conf`

and Set some paramters:
```
[rtpengine]
table = 0
interface = internal/[YOUR-INTERNAL-IP];external/[YOUR-EXTERNAL-IP]
listen-ng = 22222
listen-tcp = 25060
listen-udp = 12222
listen-cli = 9900
timeout = 60
silent-timeout = 30
tos = 184
pidfile = /var/run/ngcp-rtpengine-daemon.pid
fork = yes
port-min = 30000
port-max = 40000
log-level = 2
log-facility = daemon
```


### Starting RTPEngine
`service ngcp-rtpengine-daemon start`

Then start kamailio:
`service kamailio restart`
