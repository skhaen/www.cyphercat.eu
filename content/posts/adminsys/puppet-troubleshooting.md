+++
author = "Skhaen"
categories = ["puppet", "adminsys", "ssl"]
date = 2017-02-10T21:43:03Z
description = ""
draft = false
slug = "puppet-troubleshooting"
tags = ["puppet", "adminsys", "ssl"]
title = "[Puppet] troubleshooting"

+++

This article is a copy/paste of the conference "[Puppet troubleshooting](https://docs.google.com/presentation/d/1AT2j97HV_y2QNH_HFKwZ6ExQDKvfC_lt6qBZaai4ATo/edit#slide=id.p)" by [Thomas Uphill](https://twitter.com/uphillian) at [PuppetConf2016](https://puppetconf2016.sched.com/event/6fjC/puppet-troubleshooting-thomas-uphill-wells-fargo). It's a memo for myself, I don't want to reread the 70 slides when I want to find something...


### FIRST

* document current state
* discover recent changes (commits, ...)
* change **ONE** thing at a time. If it doesn't fix the problem, revert it.

> When you have eliminated all which is impossible, then whatever remains, however improbable, must be the truth <small>Sir Arthur Conan Doyles</small>


### Can't find puppet

```
# puppet config print server
# puppet config print ca_server
# puppet config print confdir
```

```
# host puppet
# nslookup puppet
# dig puppet
# getent hosts puppet
# ping puppet
```

### Can't connect 

```
# nc -v puppet.example.com 8140

# gnutls-cli --port 8140 puppet.example.com
# openssl s_client -connect puppet.example.com:8140
```

### Authentification

```
curl --cert certs/cottage.pem --key private_keys/cottage.pem.orig --cacert certs/ca.pem https://puppet.example.com:8140/puppet/v3/environments | jq
```

### Certificate problem

* already signed (clean)
* dates off - expired CA, expired cert

```
puppet cert fingerprint host.example.com
puppet cert print host.example.com
puppet cert clean host.example.com

openssl x509 -in cert.pem -text 
```

### OpenSSL

* View Certificate: `openssl x509 -in ca.pem -text -noout |less`
* View Fingerprint: `openssl x509 -in ca.pem -fingerprint -noout openssl x509 -in ca.pem -fingerprint -noout -sha256`
* Verify Certificate: `openssl verify -CAfile ca.pem puppet.pem`

Puppet shortcuts :

* View Certificate: `puppet cert print ca`
* View Fingerprint: `puppet cert fingerprint ca`
* List Certificates: `puppet cert list -a`

### Modulepath

```
# puppet config print modulepath --environment production
# puppet config print modulepath --environment master
```
### Service

```
/sbin/service $service status
/etc/init.d/$service status
/usr/bin/systemctl is-active $service
```
### Notify chaining

```
notify {'something': 
}->exec{'thingthatfails': 
}->notify{'after': }
```

