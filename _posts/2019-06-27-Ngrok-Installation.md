---
layout: post
title: Ngrok Installation
tags: [Utility]
---

Ngrok secure introspectable tunnels to localhost webhook development tool and debugging tool. Here is how to install Ngrok server on Linux 18.04 and client on Mac OS X.

# Server configuration

#### Install Golang

```bash
sudo apt-get install golang
```

#### Test if Golang is installed successfully.

```bash
go version
```

#### Clone ngrok

```bash
git clone https://github.com/inconshreveable/ngrok.git
cd ngrok
```

#### Generate certification

```bash
export NGROK_DOMAIN="yourdomain.com"
openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
openssl genrsa -out device.key 2048
openssl req -new -key device.key -subj "/CN=$NGROK_DOMAIN" -out device.csr
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000
```

Copy(Replace) the certs generated above to `assets/client` and `assets/server/tls`

```bash
cp rootCA.pem assets/client/tls/ngrokroot.crt
cp device.crt assets/server/tls/snakeoil.crt
cp device.key assets/server/tls/snakeoil.key
```

#### Build Ngrok

```bash
make clean
make release-server
```

The default operating system is Linux. 

#### Start Ngrokd server

```bash
./bin/ngrokd -domain="yourdomain.com" -httpAddr=":8081" -httpsAddr=":8082"
```

If you see output like this, it means your Ngrokd server runs successfully.

```bash
[16:41:56 CST 2017/04/20] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [registry] [tun] No affinity cache specified
[16:41:56 CST 2017/04/20] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [metrics] Reporting every 30 seconds
[16:41:57 CST 2017/04/20] [INFO] (ngrok/log.Info:112) Listening for public http connections on [::]:80
[16:41:57 CST 2017/04/20] [INFO] (ngrok/log.Info:112) Listening for public https connections on [::]:443
[16:41:57 CST 2017/04/20] [INFO] (ngrok/log.Info:112) Listening for control and proxy connections on [::]:4443
[16:41:57 CST 2017/04/20] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [tun:627acc92] New connection from 42.53.196.242:9386
[16:41:57 CST 2017/04/20] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [tun:627acc92] Waiting to read message
[16:41:57 CST 2017/04/20] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [tun:627acc92] Reading message with length: 159
```

# Client configuration

Cd to the Ngrok folder on the server, and type:

```bash
make release-client GOOS=darwin GOARCH=amd64
```

If you want to use it in other operating systems:

- Linux 32bit: GOOS=linux GOARCH=386
- Linux 64bit: GOOS=linux GOARCH=amd64
- Windows 32bit: GOOS=windows GOARCH=386
- Windows 64bit: GOOS=windows GOARCH=amd64
- MAC 32bit: GOOS=darwin GOARCH=386
- MAC 64bit: GOOS=darwin GOARCH=amd64
- ARM: GOOS=linux GOARCH=arm

Copy the generated client bin file to your client machine, which is in `./bin/`

In the client machine, write a new file named `ngrok.cfg`:

```bash
server_addr: "yourdomain.com:4443"
trust_host_root_certs: false
```

Run the client:

```bash
ngrok -proto=tcp -config=ngrok.cfg 8080

# Or ngrok http -config=ngrok.cfg [-subdomain=yoursubdomain] 80
```
