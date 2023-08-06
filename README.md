# NITS_VPN

Unlocking your NITS LAN in 20   minutes.

## Setting up Linux VPN Server

First up, you need a linux machine running which is connected to the internet but **NOT** through the NITS LAN. The most convenient way to get it would be a linux machine using a cloud provider like AWS, etc.

AWS, in short, allows you to use 100GB/month of bandwidth for your VPN per account for a year (12 months). Cloud related instructions below are for AWS, but you can proceed with a linux machine from any cloud provider.

### Creating up AWS Linux Machine

Register an account on [AWS](https://aws.amazon.com) and provide your *international* debit/credit card (Visa, MasterCard, etc.) details for it. To verify your identity, ₹ 2.00 will be deducted at first, but refunded later.

Login to the [AWS Console](http://console.aws.amazon.com).

Set your location as shown to a place where you want your VPN server to be (I'm selecting `Mumbai`):

![image](https://github.com/resyfer/nits_vpn/assets/74897008/7ca79558-d981-4887-8d84-1daebab878f2)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/5d01b634-fa7a-4398-b476-f8356a34db6b)

**NOTE**: Once server is created you **CAN NOT** change the location of the server. You can delete the server and start a new one in another location though.

Then search `EC2` -> `Instances` -> `Launch instances`:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/525dadfb-830c-445b-bd04-00c36dcd5360)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/d0d90d83-5926-471a-9ab1-98947d4f1eb3)

Enter any name you want for the server, select `Ubuntu` for the operating system. Then go to `Key Pair` down below and `Create new key pair` (you can use an existing one if you have it). Give the key pair a name and `Create key pair`. Download the key-pair file when popup for download shows up (automatically)

![image](https://github.com/resyfer/nits_vpn/assets/74897008/97bc6ee2-9c7f-4dee-b4fd-1caa8aed948a)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/2f96f6a0-97c9-4241-b154-a018f39383bb)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/9e95d293-72a3-4c4c-ab27-f1f2a1c05216)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/aa13d7b4-0c19-4732-bfef-c42967a4f1a9)

Wait till the `Instance check` and `Status check` of your server says `Running` and `2/2 checks passed` respectively. You can use the reload button to reload the information. Then select the server.

![image](https://github.com/resyfer/nits_vpn/assets/74897008/c76b4b0a-2af7-45b5-9e46-190e5bf16885)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/ddfb7669-25bd-4465-af9a-41e5803597ef)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/4c8fa961-8f56-41ce-9e5a-a1f46c20f8e1)


### Allowing VPN Internet Traffic

Ensuring your `EC2` instance is selected as shown above, check your EC2 instance's Security Group, select it and add the inbound traffic rules as shown below:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/c1755380-cdb9-42c9-a919-474c1b608eb3)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/01ee215d-3774-4e2e-9585-a4a7e502be81)

![image](https://github.com/resyfer/nits_vpn/assets/74897008/37de5f36-bc52-4f72-9900-11a870bf9460)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/a48000f1-8cf5-4774-8a7b-210d2b21687c)

![image](https://github.com/resyfer/nits_vpn/assets/74897008/58ede890-cfba-4106-9550-e53794c3b740)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/545936c0-b4ec-40a3-825c-db1ec69fff92)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/09ffbfa2-24f5-4594-9481-97a34a4163f3)


**NOTE**: I've kept the public IPv4 address visible to keep it beginner friendly, and I will delete the server immediately after writing this, so no point in you trying to use my servers 😄

Add the various rules as shown and click `Save rules`:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/ebc42ef4-1c09-49ce-9c0a-f9e96ed819ff)

Go back to `Instances`, select your instance and click `Connect`. Then `EC2 Instance Connect` -> `Connect`:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/5fc059c6-ae06-44a1-8773-a7b630db8d67)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/2cd0a14b-cfb4-4dd8-88bf-41900c616aae)

**NOTE**: If you face any error connecting to your instance, current cloud core members of GDSC NIT Silchar will be more than willing to help you connect using `AWS CloudShell` and `SSH`, but only provided you have your `Key Pair` safe with you.

### Setting Up OpenVPN Server
When you successfully connect to your `EC2` instance, you will see something like this:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/0eeade33-e32c-4305-8176-e53009f84043)

This is your public `IPv4 address`:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/ec0d88cd-a2f8-4b9f-a305-d2feb2aa019a)

You have to enter these commands (one by one please):
```sh
$ curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
$ chmod +x openvpn-install.sh
$ sudo bash openvpn-install.sh
```

Then enter your public `IPv4` address found above when prompted:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/8040104f-c5be-4db4-81b3-aa82787708c8)

Answers to the various prompts:
```
Do you want to enable IPv6 support (NAT)? [y/n]: n

What protocol do you want OpenVPN to use?
UDP is faster. Unless it is not available, you shouldn't use TCP.
   1) UDP
   2) TCP
Protocol [1-2]: 2

What DNS resolvers do you want to use with the VPN?
   1) Current system resolvers (from /etc/resolv.conf)
   2) Self-hosted DNS Resolver (Unbound)
   3) Cloudflare (Anycast: worldwide)
   4) Quad9 (Anycast: worldwide)
   5) Quad9 uncensored (Anycast: worldwide)
   6) FDN (France)
   7) DNS.WATCH (Germany)
   8) OpenDNS (Anycast: worldwide)
   9) Google (Anycast: worldwide)
   10) Yandex Basic (Russia)
   11) AdGuard DNS (Anycast: worldwide)
   12) NextDNS (Anycast: worldwide)
   13) Custom
DNS [1-12]: 9

Enable compression? [y/n]: n

Customize encryption settings? [y/n]: n
```

Press `Enter`. Again press `Enter` when a screen similar to this shows up:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/ddf6dd68-88d1-4162-b741-64d7e6f6c258)

Then enter a client name (anything you want) as well as the other options:
```
Client name: my_fav_vpn

Do you want to protect the configuration file with a password?
(e.g. encrypt the private key with a password)
   1) Add a passwordless client
   2) Use a password for the client
Select an option [1-2]: 1
```

Output the `<your client name>.ovpn`'s contents using following command. Then select content -> right click -> copy (better than using Ctrl + C or Ctrl + Shift + C as keybinding are problematic using browsers):
```sh
$ cat /home/ubuntu/*.ovpn
```

**NOTE**: Do remember to copy all of the file contents by scrolling down while selecting (starting from `client` and ending at `</tls-crypt>`).

On your *own PC*, create a file with the same name (`<your client name>.ovpn`) and open it in a text editor (notepad, vs code, etc.) and paste the contents into it. Example:
```
client
proto tcp-client
remote 43.205.103.45 1194
dev tun
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
verify-x509-name server_ZuwP3W2OHzdAZZ92 name
auth SHA256
auth-nocache
cipher AES-128-GCM
tls-client
tls-version-min 1.2
tls-cipher TLS-ECDHE-ECDSA-WITH-AES-128-GCM-SHA256
ignore-unknown-option block-outside-dns
setenv opt block-outside-dns # Prevent Windows 10 DNS leak
verb 3
<ca>
-----BEGIN CERTIFICATE-----
MIIB1zCCAX2gAwIBAgIUMdqlnx9rT7JMlRsWvbaYFKOR15kwCgYIKoZIzj0EAwIw
HjEcMBoGA1UEAwwTY25fT0ZaUWVlbDAyWk5BUWlEODAeFw0yMzA4MDYwNjI0MzZa
Fw0zMzA4MDMwNjI0MzZaMB4xHDAaBgNVBAMME2NuX09GWlFlZWwwMlpOQVFpRDgw
WTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAATBjm8MkaquS4wOTHKMQPc4KBFt78LU
NJ6OG15A8GJtYAvfyElUaO9mYAbT6C3XX2map7A5XOHiu2Ejox64tNG/o4GYMIGV
MAwGA1UdEwQFMAMBAf8wHQYDVR0OBBYEFPqrKtZBdyCpiW31rwO0jiXzKwwSMFkG
A1UdIwRSMFCAFPqrKtZBdyCpiW31rwO0jiXzKwwSoSKkIDAeMRwwGgYDVQQDDBNj
bl9PRlpRZWVsMDJaTkFRaUQ4ghQx2qWfH2tPskyVGxa9tpgUo5HXmTALBgNVHQ8E
BAMCAQYwCgYIKoZIzj0EAwIDSAAwRQIgXrrzDoRpAFKMVJ0HUU0QGYJkdhMbuPzq
YmqdZbrrD04CIQDH9V6Sa+SDFKvlakk90Lga9xru/C7lovfdHtTgTyUKdg==
-----END CERTIFICATE-----
</ca>
<cert>
-----BEGIN CERTIFICATE-----
MIIB3TCCAYKgAwIBAgIQSsTVyrSooU+IMrqEMMyVJzAKBggqhkjOPQQDAjAeMRww
GgYDVQQDDBNjbl9PRlpRZWVsMDJaTkFRaUQ4MB4XDTIzMDgwNjA2MjYxNloXDTI1
MTEwODA2MjYxNlowFTETMBEGA1UEAwwKbXlfZmF2X3ZwbjBZMBMGByqGSM49AgEG
CCqGSM49AwEHA0IABMx0SC7RkOm7pxJEYgrwoZ5wr2hIg2D4BoS6qg/49sV5F9aj
xd0q4k2Hddz432Pe91T62v5e8AZKQlInBjSx6c6jgaowgacwCQYDVR0TBAIwADAd
BgNVHQ4EFgQU4KQzT2iDvgknStAObRDWBYouiyUwWQYDVR0jBFIwUIAU+qsq1kF3
IKmJbfWvA7SOJfMrDBKhIqQgMB4xHDAaBgNVBAMME2NuX09GWlFlZWwwMlpOQVFp
RDiCFDHapZ8fa0+yTJUbFr22mBSjkdeZMBMGA1UdJQQMMAoGCCsGAQUFBwMCMAsG
A1UdDwQEAwIHgDAKBggqhkjOPQQDAgNJADBGAiEA4yzK6BjCIHR+p6Be0MBtNtN7
KcSbJn0BeBJdb7eaou0CIQCXgWd1OfiDxmPdQivIk5sfeGBCddzyWY0zOm7rJYO+
mw==
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
MIGHAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBG0wawIBAQQgycfhjfQXuIV246Q/
g5jq3T30RHl5Ph1cXX/WND62L3OhRANCAATMdEgu0ZDpu6cSRGIK8KGecK9oSINg
+AaEuqoP+PbFeRfWo8XdKuJNh3Xc+N9j3vdU+tr+XvAGSkJSJwY0senO
-----END PRIVATE KEY-----
</key>
<tls-crypt>
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
d2ca751b48512b391e52cae68386b65b
4b40a411b37376d22e3eb554c6743ce8
c85ac99c132edc3ce182e60e16354f52
2c1e8cf0dca687233d67efc0e44959f1
ab533f84dc57f09ec2b6e3a7f19d0c47
b741e8b5e59364ab4e8872b3c5aab6a2
40f9e8ce8e59c6aec182144ac9fc8fed
0ef44abfa543dea7eaf84c34eefd7ced
78242366f820a376ea8a93b8221e9d37
ffd0e1846f54220769c9f08e2b258079
4b2a5df24c0efccd2f772e45c11d6056
fa95d29c03f4dcc69442c784ae89729d
b2de0a8329d9d53a6c2cc5061e4c7ec2
a05ff4f200e5ad99cad2f84a2d74ccf2
f3c0075999a5a9fd45b47f5440a59bef
3546b17b0dbc5f9cd19fd8c29760d44c
-----END OpenVPN Static key V1-----
</tls-crypt>
```

## OpenVPN Client

These instructions are supposed to be performed on **your own** machine.

### Windows
Download `OpenVPN Connect` from the [OpenVPN Downloads page](https://openvpn.net/client/client-connect-vpn-for-windows/) and install it and open it. Then add a proxy as shown (enter your LAN proxy, mine is `172.16.2.11` with port `3128`) and save:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/1d86707d-56c8-484c-ac3f-e42db6390ed4)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/61a2c291-033b-420a-8eb1-e7733c570615)
![image](https://github.com/resyfer/nits_vpn/assets/74897008/1280f2ee-806e-4564-8c59-2c90d45541f9)

![image](https://github.com/resyfer/nits_vpn/assets/74897008/92478bf4-8218-431a-8f33-e037ffcda945)

Go back to `Profiles` -> + button -> `Upload file` -> Upload the `<your client name>.ovpn` file -> Give it a preferred name -> `Connect`.

Stop the connection in mid-way (click the switch). It can't connect yet as proxy is not configured. Click the pen icon beside it (Edit) -> Under `Proxy` select your proxy -> Save (top right) -> then connect by clicking the switch:

![image](https://github.com/resyfer/nits_vpn/assets/74897008/7ed616f5-354f-463e-acf8-9262d0b34555)

The speed current speed will be as fast as your LAN's speed. It doesn't slow it down. The ping that gets added to you for the VPN is `your old ping + ping proportional to distance between you and openvpn server`.

### Linux

#### Fedora
```
$ sudo dnf install openvpn
```

If your file is at `/path/to/openvpn/file.ovpn` then connect to your VPN server using the following command in your terminal:
```
$ sudo openvpn --http-proxy 172.16.2.11 3128 --config /path/to/openvpn/file.ovpn
```
**NOTE**: Assuming the LAN proxy has host as `172.16.2.11` and port as `3128` (you can change them to whatever you want).
**NOTE**: Unless you know how to make this into a daemon, keep the terminal running to keep your PC connected to your VPN.
