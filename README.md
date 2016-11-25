# zcash-proxy

ZCash and ZClassic mining stratum with Web interface.

[Latest stable release](https://github.com/sammy007/zcash-proxy/releases).

### Features

* Your personal solo ZCash & ZClassic stratum
* Rigs availability monitoring
* Keep track of accepts, rejects, blocks stats
* Easy detection of sick rigs
* Daemon failover list

![](https://cdn.pbrd.co/images/2chfdJzhD.png)

### Configuration

Just copy *config.example.json* to *config.json* and specify stratum endpoint URL and upstream URLs.

```javascript
{
  "proxy": {
    // Stratum host:port
    "listen": "0.0.0.0:9999",
    "clientTimeout": "20m",
    "blockRefreshInterval": "1s",
    "estimationWindow": "15m",
    "luckWindow": "24h",
    "largeLuckWindow": "72h",
    "nonceOffset": 0,
    // Recommended diff for 5xGPU rigs
    "difficulty": 512
  },

  // t-address for block rewards. NOT EXCHANGE!
  "coinbase": "t1K9CtKh4rZXEVfjhNovozLeLysyx9AwYrH",

  "frontend": {
    // Web-UI host:port
    "listen": "0.0.0.0:8080",
    "login": "admin",
    "password": "",
    "hideIP": false
  },

  "upstreamCheckInterval": "5s",

  // List of zcash daemons
  "upstream": [
    {
      "name": "Main",
      "url": "http://127.0.0.1:8232",
      "login": "rpcuser",
      "password": "rpcpassword",
      "timeout": "10s"
    },
    {
      "name": "Backup",
      "url": "http://127.0.0.2:8232",
      "login": "rpcuser",
      "password": "rpcpassword",
      "timeout": "10s"
    }
  ]
}
```

#### Running

    ./zcash-proxy config.json 2>&1 | tee -a log.txt

#### Mining

    nheqminer -l 192.168.1.2:9999 -u workerId -p x

#### Sharding

If you want to run many proxy instances with a single daemon use unique `nonceOffset` per instance.

#### Stats API

All the stats are available in JSON format at `http://your.host:8080/stats` URI. Change `your.host` to a frontend hostname or IP address.

#### DevFee

This proxy charges `2.5%` devfee. Fee is a part of coinbase transaction.

## Copyright

Copyright &copy; 2016 sammy007.

This is proprietary software. No warranty, explicit or implicit, provided. Reverse engineering, redistribution, repackaging are not allowed.
