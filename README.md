# geoiplookup 

lookup IP in MMDB files via mmdblookup and output JSON

Using system's MMDB if found in one of (in this order) :

- `$MAXMIND_DATADIR` if defined and a directory
- `./GeoIP`
- `./`
- `/var/lib/GeoIP/`
- `/usr/local/share/GeoIP`
- `/usr/share/GeoIP`

## Usage

```
Usage: ./geoiplookup [ -c ] | <ip>
   -c : check if usable (check DBs and one fixed IP) and exit 0 if all OK

```

## Requirements 

- jq
- mmdblookup

```bash
sudo apt install jq mmdb-bin
```

## Examples

With results
```bash
$ ./geoiplookup 193.52.64.1
{
  "asn": 2200,
  "asorg": "Renater",
  "country": "France",
  "continent": "Europe"
}
```

With no results in DB
```bash
MAXMIND_DATADIR=../maxmind-geoip-client-update/databases/ ./geoiplookup 193.52.64.1
{
  "asn": 0,
  "asorg": "NOT FOUND",
  "country": "NOT FOUND",
  "continent": "NOT FOUND"
}
```

In a shell script : 
```bash
    if "$geoiplookup_dir"/geoiplookup check > /dev/null 2>&1
    then
        # using with jq
        "$geoiplookup_dir"/geoiplookup "$ip" | jq .
        exit $?
    fi
```

