# crevasse/readme
`crevasse` 规则处理使用说明

## 功能介绍
`crevasse` 是一个规则处理服务,它提供了许多处理功能
1. 支持对原配置文件任意新增/修改/删除,而并不会影响原配置文件
2. 规则与代理配置信息分离,切换配置文件不会受到影响
3. `Surge`全面支持,支持其`1~3`版本的大部分参数
4. 支持对原配置文件的规则策略进行修改
5. 支持对原配置文件的参数大项进行开启与关闭
6. 支持对规则类型进行开启与关闭,关闭的项会自动剔除
7. 自述文件`readme`正在完善中,还有很多没有写上!

## 使用方法
如需使用,则需要提供一个[conf标准配置文件](#conf标准配置文件示例)以及[用户数据配置文件](#用户数据配置文件示例)<br>
您可以选择 `远程配置文件` 或 `rawData` 方式<br>
注意: `rawData`有字数限制,超过一定字数将无法成功导入!
* [CLI命令行](https://github.com/crevasse/converter): 将在近期支持本地生成方式,无需通过网址导入!<br>
* 远程配置文件: `http://api.injected.me/rules/{用户数据配置文件url}/{保存名称}`<br>
* rawData: `http://api.injected.me/rules/{raw_base64用户数据配置文件}/{保存名称}`<br>
* 远程配置文件示例: `http://api.injected.me/rules/http://p3re7f39t.bkt.gdipper.com/userinfo.json?readme/surge`

## 规则转换
* `crevasse/converter`提供 [CLI命令行](https://github.com/crevasse/converter) 与 [HTTP](#) 两种模式<br>
* `crevasse/converter`会将 [conf标准配置文件](#conf标准配置文件示例) 转换为 [crevasse配置文件](#crevasse配置文件示例)<br>
* <del>由于`Github`似乎出现了一些问题,导致无法`release`上传`blob`附件,已添加至build目录<del><bar>
* CLI命令行模式 : 参考[英文版说明](https://github.com/crevasse/converter)<br>
* HTTP模式: `http://api.injected.me/convert/{conf标准配置文件url}`<br>
* HTTP模式示例: `http://api.injected.me/convert/http://p3re7f39t.bkt.gdipper.com/example.conf`<br>
* HTTP模式仅支持包含`Content-Length`响应标头的文件,配置文件需小于`1MB`<br>

## 支持类型
```text
规则类(小写): DOMAIN|DOMAIN-SUFFIX|DOMAIN-KEYWORD|IP-CIDR|IP-CIDR6
规则类(默认): USER-AGENT|PROCESS-NAME|URL-REGEX
重写类(网址): header|reject|302|307
重写类(标头): header-add|header-del|header-replace
地址类(地址): skip-proxy|bypass-tun|dns-server
一般类(杂项): loglevel|bypass-system|ipv6|interface|port|socks-interface
一般类(杂项): external-controller-access|use-default-policy-if-wifi-not-primary
一般类(杂项): socks-port|allow-wifi-access
复制类(杂项): hide-apple-request|hide-crashlytics-request|use-keyword-filter
复制类(杂项): keyword-filter
解密类(解密): enable|ca-passphrase|ca-p12|hostname|tcp-connection
其他类(其他): GEOIP|FINAL|host|ssid_setting
不支持(暂时): keystore(Surge3)|部分host类型(Surge1)|MapLocal(Surge2)
```

## 配置文件
[crevasse配置文件](crevasse配置文件示例)为`crevasse/crevasse`自动生成的配置文件,无需手动修改<br>
`crevasse/crevasse`会读取[conf标准配置文件](conf标准配置文件示例)进行自动匹配生成转换结果<br>
`crevasse/crevasse`目前仅支持`Surge`标准格式配置文件的读取<br>
`crevasse/crevasse`转换结果为`crevasse(hash-json)`格式,确保每条不会重复!

### 用户数据配置文件示例
```text
{
  "proxy_info":{
    "server_list":[
      {
        "type":"custom",
        "name":"server_a",
        "server":"proxy.baidu.com",
        "port":"8081",
        "method":"aes-256-cfb",
        "password":"password",
        "module":"enable",
        "option":{
          "obfs":"http",
          "obfs-host":"www.google.com"
        }
      },
      {
        "type":"custom",
        "name":"server_b",
        "server":"proxy.baidu.com",
        "port":"8081",
        "method":"aes-256-cfb",
        "password":"password",
        "module":"enable",
        "option":{
          "tfo":"true"
        }
      },
      {
        "type":"custom",
        "name":"server_c",
        "server":"proxy.baidu.com",
        "port":"8081",
        "method":"aes-256-cfb",
        "password":"password",
        "module":"enable"
      },
      {
        "type":"http",
        "name":"server_d",
        "server":"10.0.0.1",
        "port":"8081",
        "username":"username",
        "password":"password"
      },
      {
        "type":"https",
        "name":"server_e",
        "server":"10.0.0.1",
        "port":"8081",
        "username":"username",
        "password":"password",
        "option":{
          "skip-cert-verify":"true"
        }
      },
      {
        "type":"socks5",
        "name":"server_f",
        "server":"10.0.0.1",
        "port":"8081",
        "username":"username",
        "password":"password"
      },
      {
        "type":"socks5-tls",
        "name":"server_g",
        "server":"10.0.0.1",
        "port":"8081",
        "username":"username",
        "password":"password",
        "option":{
          "skip-cert-verify":"true"
        }
      }
    ],
    "group_list":[
      {
        "type":"ssid",
        "name":"group_a",
        "default":"server_e",
        "cellular":"server_a",
        "list":{
          "ssid_1":"server_b",
          "ssid_2":"server_c"
        }
      },
      {
        "type":"select",
        "name":"group_b",
        "list":[
          "server_a",
          "server_b",
          "server_c",
          "server_d",
          "server_e",
          "server_f"
        ]
      },
      {
        "type":"select",
        "name":"group_c",
        "list":[
          "server_a",
          "server_b",
          "server_c",
          "server_d",
          "server_e"
        ]
      },
      {
        "type":"select",
        "name":"group_d",
        "list":[
          "server_a",
          "server_b",
          "server_c",
          "server_d"
        ]
      },
      {
        "type":"select",
        "name":"group_e",
        "list":[
          "server_a",
          "server_b",
          "server_c"
        ]
      },
      {
        "type":"select",
        "name":"group_f",
        "list":[
          "server_a",
          "server_b"
        ]
      },
      {
        "type":"url-test",
        "name":"group_g",
        "list":[
          "server_a",
          "server_b",
          "server_c",
          "server_d",
          "server_e",
          "server_f"
        ],
        "option":{
          "url":"http://www.gstatic.com/generate_204",
          "interval":600,
          "tolerance":200,
          "timeout":5
        }
      },
      {
        "type":"fallback",
        "name":"group_h",
        "list":[
          "server_a",
          "server_b",
          "server_c",
          "server_d",
          "server_e",
          "server_f"
        ],
        "option":{
          "url":"http://www.gstatic.com/generate_204"
        }
      }
    ]
  },
  "rules_policy":{
    "Proxy":"group_a",
    "ProxyHTTP":"group_b",
    "SelectGroup":"group_c",
    "AutoTestGroup":"group_e",
    "SSIDGroup":"group_g",
    "ProxyHTTPS":"group_h"
  },
  "managed_info":{
    "interval":86400,
    "strict":"true"
  },
  "enable_info":{
    "label_type":[
      "label",
      "general",
      "skip-proxy",
      "bypass-tun",
      "dns-server",
      "replica",
      "rules",
      "geoip",
      "final",
      "host",
      "header_rewrite",
      "url_rewrite",
      "mitm"
    ],
    "rules_type":[
      "IP-CIDR6",
      "PROCESS-NAME",
      "URL-REGEX",
      "DOMAIN-SUFFIX",
      "DOMAIN-KEYWORD",
      "DOMAIN",
      "IP-CIDR",
      "USER-AGENT"
    ]
  },
  "output_format":"surge",
  "convert_info":"https://raw.githubusercontent.com/crevasse/readme/master/example/convert.json",
  "convert_patch":"https://raw.githubusercontent.com/crevasse/readme/master/example/patch.json",
  "module_url":"https://raw.githubusercontent.com/crevasse/readme/master/example/module"
}
```
### crevasse配置文件示例
```text
{
    "label": {
        "general": "[General]",
        "proxy": "[Proxy]",
        "proxy_group": "[Proxy Group]",
        "rule": "[Rule]",
        "host": "[Host]",
        "url_rewrite": "[URL Rewrite]",
        "ssid_setting": "[SSID Setting]",
        "mitm": "[MITM]"
    },
    "general": {
        "f4abffc6636fd17ec6f461d949cbae94bf9ca47a": {
            "name": "loglevel",
            "value": "notify"
        },
        "78f07ebebb30881512d74dd033e0e68d2d3116fc": {
            "name": "external-controller-access",
            "value": "apassword@127.0.0.1:8888"
        },
        "d44af3241107655f49971036eb9da5ad3728cdf4": {
            "name": "bypass-system",
            "value": "true"
        },
        "138f9a7b2427c2213b26486e3f27ed70c1e8435e": {
            "name": "ipv6",
            "value": "false"
        },
        "5ea2d81a89e0cdc89baf7cd1ff38cdfefa33ba78": {
            "name": "interface",
            "value": "0.0.0.0"
        },
        "062d8a9dec0fa507d19f3b3481b4980ca139455f": {
            "name": "port",
            "value": "6152"
        },
        "0e1620bacb977a0c4437851b8610ec2091d02450": {
            "name": "socks-interface",
            "value": "0.0.0.0"
        },
        "cafdc3e64c3ee2115f992349043d3c380805f0dc": {
            "name": "socks-port",
            "value": "6153"
        }
    },
    "skip-proxy": {
        "list": {
            "4b84b15bff6ee5796152495a230e45e3d7e947d9": "127.0.0.1",
            "ab55803d42d270e688c6d82e3fc0da4bf3abef01": "192.168.0.0\/16",
            "10174f2d4dc0b1d847982eaca8701ba50620fb01": "10.0.0.0\/8",
            "33a365d6b210443edc95a6eaf9b9e5d38d2e8f65": "172.16.0.0\/12",
            "47f84edecf6089c56dd1e90fa4ef7b855a495ada": "100.64.0.0\/10",
            "334389048b872a533002b34d73f8c29fd09efc50": "localhost",
            "27aee6329e499402c073d10324a3974ccd48dc3a": "*.local"
        }
    },
    "dns-server": {
        "list": {
            "53a636befcd6e3049a0eae5fbc44016512229221": "8.8.8.8",
            "2d2878d28198800cfc5ed8914aece068852d094e": "8.8.4.4"
        }
    },
    "rules": {
        "c4ffa61dba27eb9b001d0b73d1c99dd79f502b33": {
            "type": "DOMAIN-SUFFIX",
            "value": "appldnld.apple.com",
            "policy": "DIRECT",
            "option": null
        },
        "b45a37ca5745f5a898ffc92f935580709dff7b80": {
            "type": "DOMAIN-SUFFIX",
            "value": "adcdownload.apple.com",
            "policy": "DIRECT",
            "option": null
        },
        "dda7f51cc6fdb1b61b4dae3c371778de14a3a015": {
            "type": "DOMAIN-SUFFIX",
            "value": "swcdn.apple.com",
            "policy": "DIRECT",
            "option": null
        },
        "c3c88718cf665920664df79980adeb1ffa7f6828": {
            "type": "DOMAIN-SUFFIX",
            "value": "phobos.apple.com",
            "policy": "DIRECT",
            "option": null
        },
        "759730a97e4373f3a0ee12805db065e3a4a649a5": {
            "type": "DOMAIN-KEYWORD",
            "value": "google",
            "policy": "ProxyHTTP",
            "option": null
        },
        "cbe648909034c0624c205fe219d3fbd10052c715": {
            "type": "DOMAIN-KEYWORD",
            "value": "facebook",
            "policy": "SelectGroup",
            "option": null
        },
        "f2382cccfbc0084c09ec5d8436af1a5f7618380b": {
            "type": "DOMAIN-KEYWORD",
            "value": "blogspot",
            "policy": "AutoTestGroup",
            "option": null
        },
        "d5244a331aad290f924ed5ed8c070d65d2e0633e": {
            "type": "DOMAIN-KEYWORD",
            "value": "youtube",
            "policy": "SSIDGroup",
            "option": null
        },
        "de740f90f540b99783f0d940f7130377691f119a": {
            "type": "DOMAIN-SUFFIX",
            "value": "apple.com",
            "policy": "ProxyHTTPS",
            "option": null
        },
        "0bcef12a28cebdbad24d70c065d62893da3e5126": {
            "type": "DOMAIN-SUFFIX",
            "value": "ad.com",
            "policy": "REJECT",
            "option": null
        },
        "ab55803d42d270e688c6d82e3fc0da4bf3abef01": {
            "type": "IP-CIDR",
            "value": "192.168.0.0\/16",
            "policy": "DIRECT",
            "option": null
        },
        "10174f2d4dc0b1d847982eaca8701ba50620fb01": {
            "type": "IP-CIDR",
            "value": "10.0.0.0\/8",
            "policy": "DIRECT",
            "option": null
        },
        "33a365d6b210443edc95a6eaf9b9e5d38d2e8f65": {
            "type": "IP-CIDR",
            "value": "172.16.0.0\/12",
            "policy": "DIRECT",
            "option": null
        },
        "5736bac7c74b884f535783d02175a8989603b741": {
            "type": "IP-CIDR",
            "value": "127.0.0.0\/8",
            "policy": "DIRECT",
            "option": null
        }
    },
    "geoip": {
        "b0bc9abd90e2f7b3bfd695191e2d0034bb1b2f52": {
            "type": "GEOIP",
            "region": "CN",
            "policy": "DIRECT",
            "option": null
        }
    },
    "final": {
        "184e66f48794012a75e0fbfe0d7a5d2699dfe976": {
            "type": "FINAL",
            "policy": "ProxyHTTP",
            "option": null
        }
    },
    "host": {
        "3cc1224dcdb34dc16b24b24843714b06cbc30d4f": {
            "type": "host",
            "host": "abc.com",
            "redirect": "1.2.3.4"
        },
        "6e4f2b70355c0aa79d513dfa0baa6275a3a7f0f8": {
            "type": "host",
            "host": "*.dev",
            "redirect": "6.7.8.9"
        },
        "cf934d97a8012ba1c2d354d6cd39e77535fd0fb9": {
            "type": "host",
            "host": "foo.com",
            "redirect": "bar.com"
        },
        "91b6494ac79837919d80c01c89c2e70b4a5479a2": {
            "type": "host",
            "host": "bar.com",
            "redirect": "server:8.8.8.8"
        },
        "c60266a8adad2f8ee67d793b4fd3fd0ffd73cc61": {
            "type": "host",
            "host": "computer",
            "redirect": "server:system"
        }
    },
    "url_rewrite": {
        "5043ad9c42980d6da0aa716a7d745fce549834b9": {
            "type": "url_rewrite",
            "pattern": "^http:\/\/www.google.cn",
            "replace": "http:\/\/www.google.com",
            "policy": "header"
        },
        "9626c08481a6a594d2be60a2300c7b4cdf2967f5": {
            "type": "url_rewrite",
            "pattern": "^http:\/\/yachen.com",
            "replace": "https:\/\/yach.me",
            "policy": "302"
        }
    },
    "ssid_setting": {
        "1b8d2e145410fda75848add722fec749220c4809": {
            "type": "ssid_setting",
            "ssid": "\"SSID Here\"",
            "suspend": "true"
        }
    },
    "mitm": {
        "enable": "true",
        "hostname": {
            "fd29ad9f643f60dcded9b2ccb57c5659a3c7816d": "*google.com"
        },
        "ca-p12": "MIIJtQ.........",
        "ca-passphrase": "password"
    },
    "build": {
        "convert_ver": "0.1.0",
        "convert_time": 1517972495
    }
}
```
### crevasse补丁文件示例
```text
{
    "general": {
        "f4abffc6636fd17ec6f461d949cbae94bf9ca47a": {
            "name": "loglevel",
            "value": "warning"
        },
        "78f07ebebb30881512d74dd033e0e68d2d3116fc": null,
        "138f9a7b2427c2213b26486e3f27ed70c1e8435e": null,
        "5ea2d81a89e0cdc89baf7cd1ff38cdfefa33ba78": null,
        "062d8a9dec0fa507d19f3b3481b4980ca139455f": null,
        "0e1620bacb977a0c4437851b8610ec2091d02450": null,
        "cafdc3e64c3ee2115f992349043d3c380805f0dc": null
    },
    "skip-proxy": {
        "list": {
            "47f84edecf6089c56dd1e90fa4ef7b855a495ada": null,
            "142c46dacc94fa647f410100253f2d2cbbb391a3": "5.62.61.64"
        }
    },
    "dns-server": {
        "list": {
            "53a636befcd6e3049a0eae5fbc44016512229221": "8.26.56.26",
            "2d2878d28198800cfc5ed8914aece068852d094e": "8.20.247.20",
            "a3aaf6fac89a33e370981ed3aa345ec72aed9414": "199.85.126.20",
            "065e363f7346bfaa1e7a79fc628fa59f5906ba1b": "199.85.127.20"
        }
    },
    "rules": {
        "c4ffa61dba27eb9b001d0b73d1c99dd79f502b33": {
            "type": "DOMAIN-SUFFIX",
            "value": "appldnld.apple.com",
            "policy": "server_a",
            "option": "force-remote-dns"
        },
        "b45a37ca5745f5a898ffc92f935580709dff7b80": {
            "type": "DOMAIN-SUFFIX",
            "value": "adcdownload.apple.com",
            "policy": "DIRECT",
            "option": null
        },
        "ef7e43109e2d73bb05d600041a1cc9e224027b87": {
            "type": "DOMAIN-SUFFIX",
            "value": "taobao.com",
            "policy": "DIRECT",
            "option": null
        },
        "ab55803d42d270e688c6d82e3fc0da4bf3abef01": {
            "type": "IP-CIDR",
            "value": "192.168.0.0\/16",
            "policy": "REJECT",
            "option": null
        }
    },
    "final": {
        "184e66f48794012a75e0fbfe0d7a5d2699dfe976": {
            "type": "FINAL",
            "policy": "group_f",
            "option": null
        }
    },
    "geoip": {
        "da2b128800d4e129def769b7ec5decc00517fa8b": {
            "type": "GEOIP",
            "region": "US",
            "policy": "group_e",
            "option": null
        }
    },
    "host": {
        "3cc1224dcdb34dc16b24b24843714b06cbc30d4f": {
            "type": "host",
            "host": "abc.com",
            "redirect": "8.7.6.5"
        },
        "bda39e7fe8bed58f9d0775027b4439efc36477cf": {
            "type": "host",
            "host": "sparaxis",
            "redirect": "server:system"
        }
    },
    "url_rewrite": {
        "5043ad9c42980d6da0aa716a7d745fce549834b9": {
            "type": "url_rewrite",
            "pattern": "^http:\/\/www.google.cn",
            "replace": "http:\/\/www.google.co.jp",
            "policy": "header"
        },
        "9626c08481a6a594d2be60a2300c7b4cdf2967f5": {
            "type": "url_rewrite",
            "pattern": "^http:\/\/yachen.com",
            "replace": "https:\/\/injected.me",
            "policy": "302"
        }
    },
    "ssid_setting": {
        "1b8d2e145410fda75848add722fec749220c4809": {
            "type": "ssid_setting",
            "ssid": "\"SSID Here\"",
            "suspend": "false"
        }
    },
    "mitm": {
        "enable": "true",
        "hostname": {
            "fd29ad9f643f60dcded9b2ccb57c5659a3c7816d": "*.google.com",
            "012627e17fa974da91361ca7e5ece3547214ba0e": "injected.me"
        },
        "ca-p12": "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",
        "ca-passphrase": "15F5092B"
    },
    "build": {
        "convert_ver": "0.1.0",
        "convert_time": 1517733286
    }
}
```
### conf标准配置文件示例
```text
# Surge Config Example
# Version 2.0

[General]
# Log level: warning, notify, info, verbose (Default: notify)
loglevel = notify
# Skip domain or IP range. These hosts will not be processed by Surge Proxy. 
# (In macOS version when Set as System Proxy enabled, these hosts will be
# applied to system network proxy settings.)
skip-proxy = 127.0.0.1, 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12, 100.64.0.0/10, localhost, *.local
# Override the DNS server provided by system.
dns-server = 8.8.8.8, 8.8.4.4
# Enable external controller access. This is for Surge-CLI and other tools.
external-controller-access = apassword@127.0.0.1:8888

# These parameters below are only for iOS version
# Bypass system related connection to TUN interface, and automatically add rule 
# "IP-CIDR,17.0.0.0/8,DIRECT"
bypass-system = true
# Whether enable fully IPv6 support (Default: false)
ipv6 = false

# These parameters below are only for macOS version
# Server listen interface (Default: 127.0.0.1)
interface = 0.0.0.0
# HTTP server port (Default: 6152)
port = 6152
# SOCKS5 server listen interface (Default: 127.0.0.1)
socks-interface = 0.0.0.0
# SOCKS5 server port (Default: 6153)
socks-port = 6153

# This section declares proxy policy
# 
# OPTIONS for all proxy type:
#   interface: Optional (Default: null).
#   Force to use a specified outgoing network interface or address
#   (Only available in macOS)
#   Example: ProxyHTTP = http, 1.2.3.4, 443, username, password, interface = en2
#            en1 = direct, interface = en1
# 
# OPTIONS for proxy with tls enabled:
#   skip-common-name-verify: Optional, "true" or "false" (Default: false).
#   If this option is enabled, Surge will not verify whether the certificate 
#   common name field is matched.
[Proxy]
ProxyHTTP = http, 1.2.3.4, 443, username, password, skip-common-name-verify=false
ProxyHTTPS = http, 1.2.3.4, 443, username, password, tls=true
ProxySOCKS5 = socks5, 1.2.3.4, 443, username, password
ProxySOCKS5TLS = socks5, 1.2.3.4, 443, username, password, tls=true

# This section declares policy group
# A policy group may contain multiple policies. It can be a proxy policy, 
# another policy group or an internal policy (DIRECT and REJECT).
# There are three group types: "select", "url-test" and "ssid"
# 
# select: Select which policy will be used from user interface
# 
# url-test: Select which policy will be used by benchmarking speed to a URL
#   OPTIONS:
#   url: Required.
#   Specify which URL will be tested.
#   interval: Optional, s (Default: 600s).
#   Decide how long the benchmark result will be discarded.
#   tolerance: Optional, ms (Default: 100ms).
#   Policy will be changed only when the new winner has a higher score than the 
#   old winner's score plus the tolerance.
#   timeout: Optional, s (Default: 5s). 
#   Give up a policy if not finished until timeout.
# 
# ssid: Select which policy will be used by Wi-FI SSID
#   OPTIONS:
#   default: Required. 
#   The policy when no matched SSID option found.
#   cellular: Optional. 
#   The policy under cellular network. If not provide, the default policy will 
#   be used.
[Proxy Group]
SelectGroup = select, ProxyHTTP, ProxyHTTPS, DIRECT, REJECT
AutoTestGroup = url-test, ProxySOCKS5, ProxySOCKS5TLS, url = http://www.google.com/generate_204
SSIDGroup = ssid, default = ProxyHTTP, cellular = ProxyHTTP, SSIDName = ProxySOCKS5

# This section declares outgoing policy rule
# A rule contains three basic parts:
#          TYPE,         VALUE,         POLICY
# Example: DOMAIN-SUFFIX,apple.com,     DIRECT
#          IP-CIDR,      192.168.0.0/16,ProxyA
# 
# There are three domain based rule types: "DOMAIN", "DOMAIN-SUFFIX" and 
# "DOMAIN-KEYWORD"
# 
#   OPTIONS:
#   force-remote-dns: Optional (Default: false). 
#   If a request is matched by this rule, and the policy isn't DIRECT. The DNS 
#   lookup will always happen in remote proxy server, even if the request is 
#   handled by Surge TUN interface. See manual for more information.
# 
# There are two IP based rule types: "IP-CIDR" and "GEOIP".
# If a request is using a domain. Surge need to resolve this domain to 
# determine if the request should be matched. If DNS lookup fails, the rule 
# process will be aborted and an error raises.
# 
#   OPTIONS:
#   no-resolve: Optional (Default: false). 
#   Skip this rule if the request is using a domain.
[Rule]
DOMAIN-SUFFIX,appldnld.apple.com,DIRECT
DOMAIN-SUFFIX,adcdownload.apple.com,DIRECT
DOMAIN-SUFFIX,swcdn.apple.com,DIRECT
DOMAIN-SUFFIX,phobos.apple.com,DIRECT

DOMAIN-KEYWORD,google,ProxyHTTP
DOMAIN-KEYWORD,facebook,SelectGroup
DOMAIN-KEYWORD,blogspot,AutoTestGroup
DOMAIN-KEYWORD,youtube,SSIDGroup

DOMAIN-SUFFIX,apple.com,ProxyHTTPS
DOMAIN-SUFFIX,ad.com,REJECT

IP-CIDR,192.168.0.0/16,DIRECT
IP-CIDR,10.0.0.0/8,DIRECT
IP-CIDR,172.16.0.0/12,DIRECT
IP-CIDR,127.0.0.0/8,DIRECT

GEOIP,CN,DIRECT
FINAL,ProxyHTTP

# This section declares local DNS map
# This is a equivalent function to /etc/hosts, with wildcard and alias support.
[Host]
abc.com = 1.2.3.4
*.dev = 6.7.8.9
foo.com = bar.com
bar.com = server:8.8.8.8
computer = server:system

# This section declares URL rewrite rules for HTTP request
# There are 3 rewrite types: "header", "302" and "reject"
# 
# Header Mode:
# Surge will modify the request header and redirect the request to another host 
# if necessary. The client will not be conscious of this rewrite action. 
# You can't redirect to an URL with HTTPS scheme. And you can't redirect a
# HTTPS request.
# 
# 302 Mode:
# Surge will simply return a 302 redirect response. HTTPS requests can be
# redirected if MitM is enabled.
# 
# Reject Mode:
# Reject the request if the pattern is matched
[URL Rewrite]
^http://www.google.cn http://www.google.com header
^http://yachen.com https://yach.me 302

# This section is only available in iOS. 
# You can specify some special option for a WiFi network by SSID
# 
# OPTIONS:
# suspend: "true" or "false". 
# Surge will suspend under this network. Please notice that Surge will still 
# work if you start Surge under this SSID directly. This option is only 
# triggered while network switching.
[SSID Setting]
"SSID Here" suspend=true

# See manual for more info: http://manual.nssurge.com/mitm.html
[MITM]
enable = true
# Surge will only decrypt traffic to hosts which are declared here. Prefix
# wildcard is allowed.
# Some applications has strict security policy to use pinned certificates or
# CA. Enabling decryption to these hosts may casue problems.
hostname = *google.com
ca-p12 = MIIJtQ.........
ca-passphrase = password
```
## 配置说明
```text
& proxy_info -> 代理信息
      + server_list -> 服务器信息
            * type -> 服务器类型(http/https/custom/socks5/socks5-tls)
            * name -> 代理名称(除英文外均需要unicode编码)
            * server -> 地址(ip/domain)
            * port -> 端口
            * method -> 加密方式
            * password -> 密码
            * module -> 模块状态(enable/disable)
            * option -> 选项(可选)
      + group_list -> 代理组信息
            * type -> 代理组类型(url-test/select/ssid/fallback)
            * name -> 代理组名称(除英文外均需要unicode编码)
            @ default -> (对于ssid组需要填写)
            @ cellular -> (对于ssid组需要填写)
            * list -> 服务器名称
            * option -> 选项(可选)
& rules_policy -> 规则策略
      @ policy_name -> replace_name
& managed_info -> 自动更新选项
      + interval -> 间隔时间
      + strict -> 强制更新
& enable_type -> 启用类型
      + label_type -> 需要使用的大项类型
      + rules_type -> 需要使用的规则类型
& output_format -> 输出格式(仅支持surge)
& convert_info -> 转换的规则文件地址(>1MB)
& convert_patch -> 规则补丁文件地址(>1MB)
& module_url -> 模块地址
```

## 配置补丁
[配置补丁](#crevasse补丁文件示例)也就是[crevasse配置文件](#crevasse配置文件示例)里的`convert_patch`条目<br>
`crevasse`会自动下载`convert_patch`文件并以递归方式进行合并<br>
[配置补丁](#crevasse补丁文件示例)目前需要手动编写,后续会添加至CLI命令行,通过交互式CLI制作<br>
[配置补丁](#crevasse补丁文件示例)与[crevasse配置文件](#crevasse配置文件示例)的格式相同

## 支持帮助
* 如有问题请联系 `burpsuite[at]mail.ru`
* 社交媒体: [Twitter](https://twitter.com/OAuth4) [Slack](https://join.slack.com/t/crevasse/shared_invite/enQtMzEwMDYzMzQxMDU2LTU2NjcyZjA1MmVkNzVhYWMwZDZjNjkyM2E2MWVmYjBkOWQyODk0MDBhMjQzN2ZhNmYyNjNjZDcyMTA2Mzk5NmM) [Discord](https://discord.gg/r8zDbW)

## License

MIT
