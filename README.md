# Wartime Internet simulator

## Background

The name of this repository is taken from the original repository this one was forked from - [wardns](https://github.com/g0v/wardns). It's aim was to simulate the disruption to the Internet caused by all their cables being cut. The original author summised that during a wartime scenario, while there would probably be satellite connectivity it would be prioritised for military and government use.  Therefore, it can be assumed that all international services will be disrupted including Line, Facebook, YouTube, Google, Google Maps, Gmail, etc.

Additionally it drew attention to the fallicy of asking citizens to "look for air raid shelter locations on Google Maps". In this situation Google Maps, a service that is presumably hosted outside Taiwan, would not work. 

Thus "Wartime Internet simulator" was created to review and test how many of their planned scenarios involved using overseas networks by seeing what services are still available when you can only access domestic networks.

## This fork

Australia is also an island. This fork has been modified for Australia.

This program is a DNS server. By changing your computer's DNS pointer to this server, you can only connect to domestic Australian networks.

## Important limitations and caveats
This simulator has significant technical limitations that make it an incomplete representation of a true cable-cut scenario.

### GeoIP Database Accuracy: 

The simulator relies on the GeoLite2 GeoIP database to determine server locations. This database:

- Is based on IP registration data, not physical server location
- Can be months out of date
- Has accuracy issues (Maxmind estimates that there is 99.8% accuracy at the country level. For IPs located within the US, 80% and 66% accuracy for cities.
- Cannot detect CDN edge nodes that may be physically in Australia but registered elsewhere. That is, it will incorrectly think the node is overseas because it's IP address is geolocated overseas.

### CDN and Anycast Networks: 

The code attempts to filter Cloudflare IPs (104.16.0.0 - 104.31.255.255) but:

- Only blocks a subset of Cloudflare's IP ranges
- Doesn't account for other major CDNs (Akamai, Fastly, AWS CloudFront, Azure CDN, etc.)
- Many "overseas" services use Australian CDN nodes that would remain operational even with cable cuts
- The simulator would block access to these Australian-hosted cached content

### DNS-Only filtering: 

This simulator only filters through DNS. Therefore

- Services using hardcoded IPs would bypass this test entirely.
- Applications with DNS caching would bypass this test and continue working until cache expires
- Does not simulate actual network routing failures or packet loss
- This simulator cannot block services that use DNS-over-HTTPS or DNS-over-TLS with external resolvers

### False negatives (Services that WOULD work in a real scenario but are blocked in this simulation)

- (maybe) International services with Australian data centers (Microsoft Azure Australia, AWS ap-southeast-2, Google Cloud Sydney)
- Cached CDN content physically located in Australia
- Peered content within Australian internet exchanges (IX Australia)

### False positives (Services that WOULD NOT work but appear available in this simulation)

- Services resolving to Australian IPs but dependent on overseas backend infrastructure (like cloud??)
- Services that resolve to local IPs but require overseas authentication or API calls (like foreign streaming services)
- Hybrid applications where the frontend loads but functionality fails

## Installation

```
$ git clone https://github.com/technoid99/wardns
$ cd wardns
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
