# Manage and connect to WIFI

**Last updated**: March 4, 2019

## Step by step guide

1. Install required packages `wireless-tools`, `network-manager`.

2. Display wireless devices using `iwconfig`:

    ```bash
    $ iwconfig

    lo        no wireless extensions.

    eth0      no wireless extensions.

    wlan0     IEEE 802.11  ESSID:off/any
              Mode:Managed  Access Point: Not-Associated
              Retry short limit:7   RTS thr:off   Fragment thr:off
              Power Management:on
    ```

3. Check status of device using `ip`:

    ```bash
    $ ip a

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether b8:27:eb:0d:38:af brd ff:ff:ff:ff:ff:ff
        inet 192.168.2.126/24 brd 192.168.2.255 scope global dynamic eth0
           valid_lft 85767sec preferred_lft 85767sec
        inet6 fe80::ba27:ebff:fe0d:38af/64 scope link
           valid_lft forever preferred_lft forever
    3: wlan0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
        link/ether b8:27:eb:58:6d:fa brd ff:ff:ff:ff:ff:ff
    ```

4. Set `wlan0` up using `nmcli`:

    ```bash
    $ sudo nmcli radio wifi on
    ```

5. Check status of wifi:

    ```bash
    $ nmcli radio wifi
    ```

    and

    ```bash
    $ nmcli dev status
    ```

6. Scan for wifi:

    ```bash
    $ nmcli dev wifi list

    IN-USE  BSSID              SSID                           MODE   CHAN  RATE        SIGNAL  BARS  SECURITY
        F0:F7:F9:F7:F3:FC  home_wifi                      Infra  3     270 Mbit/s  100     ▂▄▆█  WPA2
        10:C2:5A:0B:74:70  Fuen6Kli007                    Infra  1     130 Mbit/s  60      ▂▄▆_  WPA1 WPA3
        58:90:43:A5:B9:24  Sunrise_2.4GHz_A5B920          Infra  1     195 Mbit/s  57      ▂▄▆_  WPA1 WPA2
    ```

7. Connect to the wifi network (`SSID=home_wifi`):

    ```bash
    $ sudo nmcli --ask dev wifi connect home_wifi

    Password: ••••••••••••••••••••••••••••••••••••
    Device 'wlan0' successfully activated with 'ffffa3ff-ffc0-44ff-9792-2ff39f2affe28'.
    ```
