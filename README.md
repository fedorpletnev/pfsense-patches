# pfsense-patches

Misc patches for pfSense

## General installation
1) Install "System Patches" package via Package Manager (System - Package Manager).
2) Add patch via System - Patches, set "Path Strip Count" to 0, "Base Directory" to "/"
3) Test & apply
4) ...
5) Profit!

## Yandex.PDD Dynamic DNS patch how-to
1) "Hostname": enter your domain name, e.g. "domain.tld",
2) "Username": enter your Yandex.PDD API token (https://tech.yandex.ru/pdd/doc/concepts/access-docpage/),
3) "Zone ID": enter your "record_id" of the subdomain (https://tech.yandex.ru/pdd/doc/reference/dns-list-docpage/),
4) "TTL": leave default or set not lower than 90 (that's mininum value Yandex.PDD allows, any ttl < 90 will result in ttl = 900)
5) Save
6) ...
7) Profit!

#### How to get subdomain record_id using cURL:
Assuming PddToken is "**1234567890ABC**", domain is "**domain.tld**", subdomain is "**sub**" (so fqdn will be *sub.domain.tld*):

```bash
curl -s -H 'PddToken: 1234567890ABC' 'https://pddimp.yandex.ru/api2/admin/dns/list?domain=domain.tld' \
| sed -e 's/[{}]/''/g' \
| awk -v k="text" '{n=split($0,a,","); for (i=1; i<=n; i++) print a[i]}' \
| grep -A 1 -B 6 -i '"subdomain": "sub"'
```

Output will be like:
```bash
 "content": "127.0.0.1"
 "domain": "domain.tld"
 "fqdn": "sub.domain.tld"
 "priority": ""
 "ttl": 90
 "record_id": 12345678
 "subdomain": "sub"
 "type": "A"
```
