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
### Create a SSH tunel server
`ngrok tcp 22`

### Create a ngrok system service
#### Add tunnel config to ngrok.yml

Run `ngrok config edit`
Add bellow config to `/home/<user-name>/.config/ngrok/ngrok.yml`

```
tunnels:
  ssh-tunel:
    proto: tcp
    addr: 22
```

#### Create ngrok system service 
`cd /etc/systemd/system`

Create `ngrok.service` file like that:
```
[Unit]
Description=Share local port(s) with ngrok
After=syslog.target network.target

[Service]
Type=simple
Restart=always
RestartSec=1min
StandardOutput=null
StandardError=null
ExecStart=/usr/local/bin/ngrok start --log /var/log/ngrok.log --config /home/${USER_NAME}/.config/ngrok/ngrok.yml --all
ExecStop=/usr/bin/killall ngrok

[Install]
WantedBy=multi-user.target
```

Restart ngrok.service

`systemctl enable ngrok.service && systemctl start ngrok.service`
