# OPNsense Config

## Phase 1

![](https://i.tzockt.beer/u/QXepoJ.png)

Pre-Shared Key kann mit folgenden Befehl in Linux erstellt werden:
`head -c 4096 /dev/urandom | sha256sum | cut -b1-32`
![](https://i.tzockt.beer/u/o4pCPx.png)

![](https://i.tzockt.beer/u/ikJ5ry.png)

## Phase 2

![](https://i.tzockt.beer/u/a5btcj.png)


# Fritzbox Config

```conf

vpncfg {
connections {
  enabled = yes;
  conn_type = conntype_lan;
  name = "VPN zur OPN";
  always_renew = yes;
  reject_not_encrypted = no;
  dont_filter_netbios = yes;
  localip = 0.0.0.0;
  local_virtualip = 0.0.0.0;
  remoteip = 0.0.0.0;
  remote_virtualip = 0.0.0.0;
  remotehostname = "opn.domain.de";
  localid {
    fqdn = "fritzbox.domain.de";
    }
  remoteid {
    fqdn = "opn.domain.de";
    }
  mode = phase1_mode_aggressive;
  phase1ss = "all/all/all";
  keytype = connkeytype_pre_shared;
  key = "<Generierter Pre-Shared Key>";
  cert_do_server_auth = no;
  use_nat_t = no;
  use_xauth = no;
  use_cfgmode = no;
  phase2localid {
    ipnet {
      ipaddr = 192.168.170.0; <- Lokales Netz
      mask = 255.255.255.0;
      }
    }
  phase2remoteid {
    ipnet {
      ipaddr = 192.168.0.0; <- Remote Netz
      mask = 255.255.252.0;
      }
    }
  phase2ss = "esp-aes256-3des-sha/ah-no/comp-lzs-no/pfs";
  accesslist = "permit ip any 192.168.0.0 255.255.252.0"; <- Lokales Netz
  }
  ike_forward_rules = "udp 0.0.0.0:500 0.0.0.0:500",
                      "udp 0.0.0.0:4500 0.0.0.0:4500";
}

```