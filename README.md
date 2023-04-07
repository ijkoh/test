
## Добавление/создание нового инстанса/домена в организации.


###### _Данные берутся из структуры организации, которая находится, путь: vars_smartpetrol/monitoring/ "grafana_organizations", и выполняются c помощью роли: monitoring/define_vars/main.yml_




#### 1. Под grafana_organizations:добавляем вашу новую организацию, под ней экспортер или endpoint.

#### 2. Указываем инстанс метрики. Если это endpoint, то нужно указать ещё domain и user.

#### 3. Password vault generation with command: `ansible-vault encrypt_string`



###### _Example(endpoint)_

``` yaml
 CargorunGeocoder:
    endpoint:
      - instance: geocoder-prod
        domain: metrics.prod.app.geo.canada.smprojects.ru
        user: pushgateway
        password: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            65383637616264663833646634626564346333323965316231346161356338383932366534653237
            3036653637393266333738306236656636383431306436650a383161623735633066396165623162
            64356532663635643931333233623861356265336530316636376537366438313263303538366661
            3632623530653330310a386266623535653633636339396238653762356663306565626338366239
            33303536666237346137373532386332313339323232313831666335353838306636643363656266
            6336363936653532663461623135653335386665623235343935
```
            
            
 

###### _Пример вывода структуры в консоли, после запуска роли `/roles/monitioring`:_
```yaml
  msg:
  - domain: cadvisor.ct.smprojects.ru
    exporter: cadvisor
    instance: cargorun-eam-prod-connecticut
    label: cadvisor-connecticut
    organization: CargorunEAM
    password: password
    user: cadvisor
```
---

## Описание переменных, вывод значений и с помощью чего они генерируются

###### _С помощью таска: `Transform dict to array flatten_grafana_organizations`,
получаем переменные и значения для:_
```yaml
инстанс:
 1. exporter 
 2. instance
 3. organization
````
```yaml
endpoint:
 1. exporter 
 2. instance
 3. organization
 4. domain 
 5. user
 6. label
 7. password
```
_Таск `Transform dict to array flatten_grafana_organizations` исключает генерацию домена в уже созданный заранее вручную домен в инстансе. Берёт те инстансы, которые указаны в структуре без инстанса и генерирует им, с помощью тасков: `Generate passwords for basicAuth` и `Lookup exporters password for prometheus scrape_configs`, такие переменные, как:_
```yaml
инстанс:
 4. domain 
 5. user
 6. label
 7. password
```
