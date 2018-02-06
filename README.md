# crevasse/readme
`crevasse` 规则处理使用说明

## 功能介绍
`crevasse` 是一个规则处理服务,它提供了许多处理功能
1. 支持对原配置文件进行任意修改删除,而这些操作并不会影响原配置文件
2. 规则与代理配置信息分离,切换配置文件不会受到影响
3. `Surge`全面支持,支持其`1~3`版本的大部分参数
4. 支持对原配置文件的规则策略进行修改
5. 支持对原配置文件的参数大项进行开启与关闭
6. 支持对规则类型进行开启与关闭,关闭的项会自动剔除

## 使用方法
如需使用,则需要提供一个[conf标准配置文件]()以及[用户数据配置文件]()<br>
您可以选择 `远程配置文件` 或 `rawData` 方式<br>
注意: `rawData`有字数限制,超过一定字数将无法成功导入!
* 远程配置文件: `http://api.injected.me/rules/{目标url}/{保存名称}`<br>
* rawData: `http://api.injected.me/rules/{base64数据}/{保存名称}`

## 规则转换
* `crevasse/converter`提供 [CLI命令行]() 与 [HTTP]() 两种模式<br>
* `crevasse/converter`会将 [conf标准配置文件]() 转换为 [crevasse配置文件]()<br>
* CLI命令行模式 : 参考[英文版说明](https://github.com/crevasse/converter)<br>
* HTTP模式 : `http://api.injected.me/convert/{conf标准配置文件url}`<br>
* HTTP模式仅支持包含`Content-Length`响应标头的文件,配置文件需小于`1MB`<br>

## 配置文件
[crevasse配置文件]()为`crevasse/crevasse`自动生成的配置文件,无需手动修改<br>
`crevasse/crevasse`会读取[conf标准配置文件]()进行自动匹配生成转换结果<br>
`crevasse/crevasse`目前仅支持`Surge`标准格式配置文件的读取<br>
`crevasse/crevasse`生成结果为`crevasse(hash-json)`格式,每条都是独一无二的!

### 用户数据配置文件
``` text
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
        "name":"server_g",
        "server":"10.0.0.1",
        "port":"8081",
        "username":"username",
        "password":"password"
      },
      {
        "type":"socks5-tls",
        "name":"server_f",
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
        "name":"Group",
        "default":"server_e",
        "cellular":"server_a",
        "list":{
          "ssid_1":"server_b",
          "ssid_2":"server_c"
        }
      },
      {
        "type":"select",
        "name":"group_a",
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
        "name":"group_b",
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
        "name":"group_c",
        "list":[
          "server_a",
          "server_b",
          "server_c",
          "server_d"
        ]
      },
      {
        "type":"select",
        "name":"group_d",
        "list":[
          "server_a",
          "server_b",
          "server_c"
        ]
      },
      {
        "type":"select",
        "name":"group_e",
        "list":[
          "server_a",
          "server_b"
        ]
      },
      {
        "type":"url-test",
        "name":"group_f",
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
      }
    ]
  },
  "rules_policy":{
    "Proxy":"group_a"
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
  "convert_info":"https://raw.githubusercontent.com/sparaxis/sparaxis/master/example/convert.json",
  "convert_patch":"https://raw.githubusercontent.com/sparaxis/sparaxis/master/example/patch.json",
  "module_url":"https://google.com/module"
}
```

### crevasse配置文件
``` text
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
        "convert_time": 1517803056
    }
}
```
