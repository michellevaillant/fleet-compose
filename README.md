## add logstash pipelines
todo
## setup winlogbeat
### configure winlogbeat.yml
1 setup templates, dashboards etc in elastcisearch/kibana  
1.1 configure elasticsearch output and kibana
```
setup.dashboards.enabled: true
setup.kibana:
  host: "http://<kibana>:5601"
output.elasticsearch:
  hosts: ["http://<elasticsearch>:9200"]
```  
1.2 start setup ```winlogbeat.exe setup```   
1.3 disable elasticsearch output 
```
# output.elasticsearch:
#  hosts: ["http://<elasticsearch>:9200"]
```
2 enable and configure logstash output in winlogbeat.yml
```
output.logstash:
  hosts: ["<logstash>:5044"]
```
### install service
1 run powershell script ```install-service-winlogbeat```
2 star service ```winlogbeat```
## enroll with fleet and server setup
in kibana -> fleet -> change output in settings:
Hosts -> https://es01:9200  
pull the CA certificate from container:  
docker cp elk-csv-import-es01-1:/usr/share/elasticsearch/config/certs/ca/ca.crt /tmp/.  
get fingerprint of ca:  
openssl x509 -fingerprint -sha256 -noout -in /tmp/ca.crt | awk -F"=" {' print $2 '} | sed s/://g  
insert fingerprint in settings CA trusted fingerprint.  
get ca crt content with cat.  
insert in settings yaml:  
```
ssl:
  certificate_authorities:
  - |
    -----BEGIN CERTIFICATE-----
    MIIDSjCCAjKgAwIBAgIVALBoRgizMYUxU7XUYVxGS2KV72ENMA0GCSqGSIb3DQEB
    CwUAMDQxMjAwBgNVBAMTKUVsYXN0aWMgQ2VydGlmaWNhdGUgVG9vbCBBdXRvZ2Vu
    ZXJhdGVkIENBMB4XDTI0MTIxNTE3NTgyN1oXDTI3MTIxNTE3NTgyN1owNDEyMDAG
    A1UEAxMpRWxhc3RpYyBDZXJ0aWZpY2F0ZSBUb29sIEF1dG9nZW5lcmF0ZWQgQ0Ew
    ggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDIlUza635AtJF4AFnWcA3/
    ePDUiTONGy7+JgZ2sUk5BpgPZmzObuxHRh2DfJ4NjA0AqJOHZvvyLnsWyBL/IJGf
    auMw0DkI8e8H9X6/gYGCrn5Jvi6r16Gca8NGOZ8pR9k0lOqwm7uJ2pmW5vlD813j
    6PxqRVd/NOo2m6yKAsTbfW3RK0izq3uK2FTSP4/Df4j6gsQYwRKFHhzU4cclW8zG
    PuyDV0LWgKMmoup1+VQRrnSio6cd7cm52rjTB3ZLn8cQ79qFru+sgxhVU4Guav6n
    +ItW/Y3UvZO1TF+WCM6kTCER2QURJhkwj7DqjycEjjaJzJTbkMmawjELg93BJmtN
    AgMBAAGjUzBRMB0GA1UdDgQWBBR3XnanQc3H+l2A6a4flKQhECGoBjAfBgNVHSME
    GDAWgBR3XnanQc3H+l2A6a4flKQhECGoBjAPBgNVHRMBAf8EBTADAQH/MA0GCSqG
    SIb3DQEBCwUAA4IBAQBkw3LVI8K5aX/ZvVe17qiC/E1SAd5XLpmCyABW58pPb9Ih
    kPh7sEwv8NoieYobvGzyiuHF+i3plApq+i1qKcqZb2PJ49lu+udCAGdJ6Z/m0ZRw
    ejUPrQNs2yhZ+WOP3z/7bUByfpGlcZp5PjlG2PSIdh8+OMKbFXkn+C8rUVQo6QcE
    9mD+4Mw3uueGMlfWE/jNscGomA1TXL1fiqvFxR348SXFdt38MB1qZiJJi9hMKxPm
    LGYVRPUM8FWuW6SJfXEpPhbYpfVAfcQ6Fyw9niFZIM5hUiRXF/Dsiwi9Oz44uxvu
    9DxfOLPbFvlIQcRXvGEcWqQ0jvq02h0HTBSYKZxH
    -----END CERTIFICATE-----
```
