# Wartime Internet simulator

## Background

The name of this repository is taken from the original repository this one was forked from - [wardns](https://github.com/g0v/wardns). It's aim was to simulate the disruption to the Internet caused by all their cables being cut. The original author summised that during a wartime scenario, while there would probably be satellite connectivity it would be prioritised for military and government use.  Therefore, it can be assumed that all international services will be disrupted including Line, Facebook, YouTube, Google, Google Maps, Gmail, etc.

Additionally it drew attention to the fallicy of asking citizens to "look for air raid shelter locations on Google Maps". In this situation Google Maps, a service that is presumably hosted outside Taiwan, would not work. 

Thus "Wartime Internet simulator" was created to review and test how many of their planned scenarios involved using overseas networks by seeing what services are still available when you can only access domestic networks.

## This fork

Australia is also an island. This fork has been modified for Australia.

This program is a DNS server. By changing your computer's DNS pointer to this server, you can only connect to domestic Australian networks.

## Installation

```
$ git clone https://github.com/g0v/wardns
$ cd worddns
$ curl https://cdn.jsdelivr.net/npm/geolite2-city@1.0.0/GeoLite2-City.mmdb.gz | gunzip > GeoLite2-City.mmdb
$ python3 -m venv venv
```

## Starting the DNS server
```
$ source venv/bin/activate
$ pip3 install -r requirements.txt
$ python3 dnsserver.py
```

## Test by changing your computer's DNS to 127.0.0.1

## License
The code is licensed under BSDLicense
