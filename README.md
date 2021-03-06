# deploy-freenas

deploy-freenas.py is a Python script to deploy TLS certificates to a FreeNAS server using the FreeNAS API.  This should ensure that the certificate data is properly stored in the configuration database, and that all appropriate services use this certificate.  It's intended to be called from a Let's Encrypt client like [acme.sh](https://github.com/Neilpang/acme.sh) after the certificate is issued, so that the entire process of issuance (or renewal) and deployment can be automated.

# Installation
This script can run on any machine running Python 3 that has network access to your FreeNAS server, but in most cases it's best to run it directly on the FreeNAS box.  Change to a convenient directory and run `git clone https://github.com/danb35/deploy-freenas`.

# Usage

The relevant configuration takes place in the `deploy_config` file.  You can create this file either by copying `depoy_config.example` from this repository, or directly using your preferred text editor.  Its format is as follows:

```
[deploy]
password = YourReallySecureRootPassword
cert_fqdn = foo.bar.baz
connect_host = baz.bar.foo
verify = false
privkey_path = /some/other/path
fullchain_path = /some/other/other/path
protocol = https://
port = 443
```

Everything but the password is optional, and the defaults are documented in `depoy_config.example`.

Once you've prepared `deploy_config`, you can run `deploy_freenas.py`.  The intended use is that it would be called by your ACME client after issuing a certificate.  With acme.sh, for example, you'd add `--deploy-hook "/path/to/deploy_freenas.py"` to your command.

There is an optional paramter, `-c` or `--config`, that lets you specify the path to your configuration file. By default the script will try to use `deploy_config` in the script working directoy:

```
/path/to/deploy_freenas.py --config /somewhere/else/deploy_config
```
