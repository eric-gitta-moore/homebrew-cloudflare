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
```

Reference:
- https://gist.github.com/xmlking/cff9510dac9281d29390392cbbb033a8