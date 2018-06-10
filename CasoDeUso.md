# Configurando a linha do Grok

Para configurar a log do IIS utilizando os campos reais da minha log, utilizei o site [Grok debug](https://grokdebug.herokuapp.com/).

No campo da mensagem inclui as seguintes linha da log do IIS:
```
2018-05-16 03:00:00 172.17.120.30 GET /ICareCustomerBillingUI/Recharge/Index nochace=Tue%20May%2015%2023:59:55%20UTC-0300%202018&_=1526439595543 80 - 172.17.120.6 Mozilla/5.0+(compatible;+MSIE+9.0;+Windows+NT+6.1;+Trident/5.0) 200 0 0 29343 1073 4851
```

E no campo do Grok, incluir a configuração:
```
%{TIMESTAMP_ISO8601:timestamp} %{IPORHOST:S-IP} %{WORD:CS_Method} %{URIPATH:cs_uri_stem} (?:%{NOTSPACE:cs_query}|-) %{NUMBER:S_Port} %{NOTSPACE:CS_Username} %{IP:clientip} %{NOTSPACE:cs_username} %{NUMBER:sc_substatus} %{NUMBER:SC_SubStatus} %{NUMBER:SC_Win32_Status} %{NUMBER:SC_Bytes} %{NUMBER:CS_Bytes} %{NUMBER:Time_Taken}
```

Resultado:

```
{
  "timestamp": [
    [
      "2018-05-16 03:00:00"
    ]
  ],
  "YEAR": [
    [
      "2018"
    ]
  ],
  "MONTHNUM": [
    [
      "05"
    ]
  ],
  "MONTHDAY": [
    [
      "16"
    ]
  ],
  "HOUR": [
    [
      "03",
      null
    ]
  ],
  "MINUTE": [
    [
      "00",
      null
    ]
  ],
  "SECOND": [
    [
      "00"
    ]
  ],
  "ISO8601_TIMEZONE": [
    [
      null
    ]
  ],
  "S": [
    [
      "172.17.120.30"
    ]
  ],
  "HOSTNAME": [
    [
      "172.17.120.30"
    ]
  ],
  "IP": [
    [
      null
    ]
  ],
  "IPV6": [
    [
      null,
      null
    ]
  ],
  "IPV4": [
    [
      null,
      "172.17.120.6"
    ]
  ],
  "CS_Method": [
    [
      "GET"
    ]
  ],
  "cs_uri_stem": [
    [
      "/ICareCustomerBillingUI/Recharge/Index"
    ]
  ],
  "cs_query": [
    [
      "nochace=Tue%20May%2015%2023:59:55%20UTC-0300%202018&_=1526439595543"
    ]
  ],
  "S_Port": [
    [
      "80"
    ]
  ],
  "BASE10NUM": [
    [
      "80",
      "200",
      "0",
      "0",
      "29343",
      "1073",
      "4851"
    ]
  ],
  "CS_Username": [
    [
      "-"
    ]
  ],
  "clientip": [
    [
      "172.17.120.6"
    ]
  ],
  "cs_username": [
    [
      "Mozilla/5.0+(compatible;+MSIE+9.0;+Windows+NT+6.1;+Trident/5.0)"
    ]
  ],
  "sc_substatus": [
    [
      "200"
    ]
  ],
  "SC_SubStatus": [
    [
      "0"
    ]
  ],
  "SC_Win32_Status": [
    [
      "0"
    ]
  ],
  "SC_Bytes": [
    [
      "29343"
    ]
  ],
  "CS_Bytes": [
    [
      "1073"
    ]
  ],
  "Time_Taken": [
    [
      "4851"
    ]
  ]
}
```

## Entendimentos

Depois de alguns testes entendi que:
- Não utilizar "-" no nome dos campos. O grok corta no primeiro "-"
- No resultado acima está aparecendo BASE10 NUM. Isso deve ser causado por problema com valores em branco onde a configuração está se perdendo
- O nome dos campos do GROK tornam-se variáveis. Não repetir o campo.

## Referência
- Estudar na próxima vez [grok filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)
