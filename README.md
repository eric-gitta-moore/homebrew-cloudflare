# homebrew-cloudflare

- 添加 curl 对 ECH 的支持


## quickstart

```shell
# Clean up any old version of curl you may have already tried to install
brew remove -f curl

# Download the curl ruby install script provided by cloudflare
wget https://github.com/eric-gitta-moore/homebrew-cloudflare/raw/master/curl.rb

# Install curl via that script from the latest git repos
# brew install --HEAD -s curl.rb
# brew install --formula --HEAD -s curl.rb
brew tap-new local/custom
cp curl.rb $(brew --repository local/custom)/Formula/
brew install --HEAD -s local/custom/curl

# Tell your cli to use the curl version just installed (if you're using zsh, othwerise you might need `~/.bashrc`)
echo 'export PATH="/usr/local/opt/curl/bin:$PATH"' >> ~/.zshrc

# Reload your config
source ~/.zshrc

# Double check it's using the right curl
which curl # Should output "/usr/local/opt/curl/bin/curl"

# Double check http3
$ curl --version | grep HTTP3
  Features: alt-svc AsynchDNS brotli HTTP2 HTTP3 IDN IPv6 Largefile libz MultiSSL NTLM NTLM_WB SSL UnixSockets zstd

# Try curl on any HTTP/3 enabled sites.
curl --http3 https://blog.cloudflare.com -I

❯ curl --version
curl 8.18.0-DEV (aarch64-apple-darwin24.5.0) libcurl/8.18.0-DEV BoringSSL zlib/1.2.12 brotli/1.2.0 zstd/1.5.7 libidn2/2.3.8 libssh2/1.11.1 nghttp2/1.68.0 quiche/0.24.6 librtmp/2.3 mit-krb5/1.7-prerelease OpenLDAP/2.6.10
Release-Date: [unreleased]
Protocols: dict file ftp ftps gopher gophers http https imap imaps ipfs ipns ldap ldaps mqtt pop3 pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp ws wss
Features: alt-svc AsynchDNS brotli ECH GSS-API HSTS HTTP2 HTTP3 HTTPS-proxy HTTPSRR IDN IPv6 Kerberos Largefile libz NTLM SPNEGO SSL threadsafe UnixSockets zstd
```

## 测试是否生效

> sni=encrypted

```sh
❯ curl --ech hard --doh-url https://doh.pub/dns-query https://cloudflare-ech.com/cdn-cgi/trace 
fl=1023f55
h=cloudflare-ech.com
ip=2400:a860:2:c::3
ts=1765352669.337
visit_scheme=https
uag=curl/8.18.0-DEV
colo=LAX
sliver=none
http=http/3
loc=CN
tls=TLSv1.3
sni=encrypted
warp=off
gateway=off
rbi=off
kex=X25519
```

## ECH + 优选IP

```sh
❯ curl -v \
    --ech ecl:AEX+DQBBmQAgACDEGEQ619JgOICZmPOH5d4I2c68Kg9/et66cS9Rmd5DaQAEAAEAAQASY2xvdWRmbGFyZS1lY2guY29tAAA= \
    --connect-to pasyun-sg01-sdwan.larkoffice.netlib.re:443:openai.com:443 \
    https://pasyun-sg01-sdwan.larkoffice.netlib.re/cdn-cgi/trace
fl=998f78
h=pasyun-sg01-sdwan.larkoffice.netlib.re
ip=203.208.167.148
ts=1765352989.000
visit_scheme=https
uag=curl/8.18.0-DEV
colo=SIN
sliver=none
http=http/2
loc=SG
tls=TLSv1.3
sni=encrypted
warp=off
gateway=off
rbi=off
kex=X25519
```


Reference:
- https://gist.github.com/xmlking/cff9510dac9281d29390392cbbb033a8