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

