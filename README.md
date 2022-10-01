# Select a tunnel server
We have many tunel server to create a SSH tunnel, such as: Ngrok, CloudFlare, ...

# Ngrok
## Install
### Register a new account on Ngrok website:
https://dashboard.ngrok.com/signup

### Download & Install Ngrok
https://dashboard.ngrok.com/get-started/setup

### Add an authen token to local host
Go to `your-authtoken` page and get authentoken 

https://dashboard.ngrok.com/get-started/your-authtoken

`ngrok config add-authtoken <your-token>`

Authen-token will be added at `/home/<user-name>/.config/ngrok/ngrok.yml`

### Create a ngrok system service
#### Add tunnel config to ngrok.yml
`sudo mkdir -p /opt/ngrok`

`sudo vim /opt/ngrok/ngrok.yml`

Add bellow config

```
version: "2"
authtoken: <your-authentoken>

tunnels:
  ssh-tunnel:
    proto: tcp
    addr: 22
```

#### Create ngrok system service 
`sudo vim /lib/systemd/system/ngrok.service`

Add below config

```
[Unit]
Description=ngrok
After=network.target

[Service]
ExecStart=/usr/local/bin/ngrok start ssh-tunnel --log /var/log/ngrok.log --config /opt/ngrok/ngrok.yml 
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
IgnoreSIGPIPE=true
Restart=always
RestartSec=3
Type=simple

[Install]
WantedBy=multi-user.target
```

Restart ngrok.service

`systemctl enable ngrok.service`

`systemctl start ngrok.service`

Verify ngrok service

`ps -A | grep ngrok`

`cat /var/log/ngrok`

### SSH over ngrok tunnel
Go to https://dashboard.ngrok.com/cloud-edge/endpoints to get tcp URL

Like that: `tcp://0.tcp.ap.ngrok.io:13816`

`ssh <user_name>@0.tcp.ap.ngrok.io -p 13816`
