# Konfigurasi-Pisah-Traffic-Dengan-RAW-dan-Content
Konfigurasi Pisah Traffic Dengan RAW dan Content

# may/01/2024 16:30:14 by RouterOS 6.48.2
# software id = BT5U-7MUZ
#
# model = 951Ui-2nD
# serial number = 7C2607D47E83
/interface bridge
add name=bridgeSW
/interface ethernet
set [ find default-name=ether1 ] comment=ISP
/interface l2tp-client
add connect-to=103.31.132.90 disabled=no name=l2tp-out1 password=nanda user=\
    nanda
add connect-to=103.31.133.52 disabled=no name=l2tp-out2 password=nanda user=\
    nanda
/interface wireless
set [ find default-name=wlan1 ] disabled=no mode=ap-bridge ssid=GAMEMODE
/interface list
add name=WAN
add name=LAN
/interface wireless security-profiles
set [ find default=yes ] authentication-types=wpa-psk mode=dynamic-keys \
    supplicant-identity=MikroTik wpa-pre-shared-key=02198132114 \
    wpa2-pre-shared-key=02198132114
/ip hotspot profile
set [ find default=yes ] html-directory=hotspot
/ip pool
add name=dhcp ranges=192.168.11.10-192.168.11.254
/ip dhcp-server
add address-pool=dhcp disabled=no interface=bridgeSW name=dhcp1
/queue simple
add name=TOTAL_SPEED target=192.168.11.0/24
add max-limit=4M/4M name="0. Trafik Medsos" packet-marks=Paket-Medsos parent=\
    TOTAL_SPEED target=192.168.11.0/24
add max-limit=3M/3M name="1. Koneksi Youtube" packet-marks=Paket-Youtube \
    parent=TOTAL_SPEED target=192.168.11.0/24
add max-limit=5M/5M name="2. Koneksi Game" packet-marks=Paket-Game parent=\
    TOTAL_SPEED target=192.168.11.0/24
/interface bridge port
add bridge=bridgeSW interface=ether3
add bridge=bridgeSW interface=wlan1
/ip neighbor discovery-settings
set discover-interface-list=!dynamic
/interface list member
add interface=ether1 list=WAN
add interface=bridgeSW list=LAN
/ip address
add address=192.168.11.1/24 interface=bridgeSW network=192.168.11.0
/ip dhcp-client
add disabled=no interface=ether1
/ip dhcp-server network
add address=192.168.11.0/24 gateway=192.168.11.1
/ip dns
set allow-remote-requests=yes servers=8.8.8.8,8.8.4.4,1.1.1.1,1.0.0.1
/ip firewall address-list
add address=192.168.11.0/24 list=IP-Lokal-Client
add address=192.168.0.0/24 list=IP-Lokal-Client
/ip firewall mangle
add action=mark-connection chain=prerouting comment="Koneksi Medsos" \
    dst-address-list=Medsos-List new-connection-mark=Koneksi-Medsos \
    passthrough=yes src-address-list=IP-Lokal-Client
add action=mark-packet chain=forward connection-mark=Koneksi-Medsos \
    new-packet-mark=Paket-Medsos passthrough=no
add action=mark-connection chain=prerouting comment="Koneksi Youtube" \
    dst-address-list=Youtube-List new-connection-mark=Koneksi-Youtube \
    passthrough=yes src-address-list=IP-Lokal-Client
add action=mark-packet chain=forward connection-mark=Koneksi-Youtube \
    new-packet-mark=Paket-Youtube passthrough=no
add action=mark-connection chain=prerouting comment="Koneksi Game" \
    dst-address-list=Game-List new-connection-mark=Koneksi-Game passthrough=\
    yes src-address-list=IP-Lokal-Client
add action=mark-packet chain=forward connection-mark=Koneksi-Game \
    new-packet-mark=Paket-Game passthrough=no
/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1
/ip firewall raw
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting comment=MEDSOS content=\
    .facebook.com dst-address-list=!IP-Lokal-Client src-address-list=\
    IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.facebook.net \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.fbcdn.net \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.fbsbx.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=fb.com dst-address-list=\
    !IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=fb.gg dst-address-list=\
    !IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=fbwat.ch \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=messenger.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=m.me dst-address-list=\
    !IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.instagram.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.cdninstagram.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=twitter.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.twimg.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=t.co dst-address-list=\
    !IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.tiktokcdn.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.tiktokcdn-us.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.ibyteimg.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.ibytedtos.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.tiktok.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Medsos-List \
    address-list-timeout=1h chain=prerouting content=.tiktokv.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Youtube-List \
    address-list-timeout=1h chain=prerouting comment="YOUTUBE DAN VIDEO" \
    content=.youtube.com dst-address-list=!IP-Lokal-Client src-address-list=\
    IP-Lokal-Client
add action=add-dst-to-address-list address-list=Youtube-List \
    address-list-timeout=1h chain=prerouting content=.ytimg.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Youtube-List \
    address-list-timeout=1h chain=prerouting content=.googlevideo.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Youtube-List \
    address-list-timeout=1h chain=prerouting content=yt3.ggpht.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Youtube-List \
    address-list-timeout=1h chain=prerouting content=youtubei.googleapis.com \
    dst-address-list=!IP-Lokal-Client src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting comment="PORT GAME" \
    dst-address-list=!IP-Lokal-Client dst-port=\
    5000-5221,5224-5227,5229-5241,5243-5287,5289-5509,5517,5520-5529 \
    protocol=tcp src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port=\
    5551-5559,5601-5700,8443,9000-9010,9443,10003,30000-30300 protocol=tcp \
    src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port="2702,3702,4001-4009,5000-5221,5224-5241,5243-52\
    87,5289-5509,5517,5520-5529" protocol=udp src-address-list=\
    IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port=\
    5551-5559,5601-5700,8001,8130,8443,9000-9010,9120,9992,10003,30000-30300 \
    protocol=udp src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port=\
    6006,6008,6674,7006-7008,7889,8001-8012,9006,9137,10000-10012,11000-11019 \
    protocol=tcp src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port=\
    6006,6008,6674,7006-7008,7889,8008,8001-8012,8130,8443,9008,9120 \
    protocol=udp src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port=\
    12006,12008,13006,15006,20561,39003,39006,39698,39779,39800 protocol=tcp \
    src-address-list=IP-Lokal-Client
add action=add-dst-to-address-list address-list=Game-List \
    address-list-timeout=4h chain=prerouting dst-address-list=\
    !IP-Lokal-Client dst-port=10000-10015,10100,11000-11019,12008,13008 \
    protocol=udp src-address-list=IP-Lokal-Client
/ip route
add comment="Remote Nurulfajri" distance=1 dst-address=10.101.104.0/24 \
    gateway=l2tp-out1
add comment="Remote Nurulfajri" distance=1 dst-address=10.101.105.0/24 \
    gateway=l2tp-out1
add comment="Remote Client" distance=1 dst-address=192.11.0.0/24 gateway=\
    l2tp-out1
/system clock
set time-zone-name=Asia/Jakarta
/system scheduler
add interval=1d4h name="Auto Reboot" on-event="/system reboot" policy=\
    ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon \
    start-date=may/01/2024 start-time=16:26:02
