---
title: 防火牆屬性
description: 請參閱範例，瞭解如何在Commerce應用程式設定檔案中設定防火牆屬性。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# 防火牆屬性

>[!IMPORTANT]
>
>僅限入門專案

對於入門專案，`firewall`屬性會將&#x200B;_輸出_&#x200B;防火牆新增至應用程式。 此防火牆對傳入的請求沒有影響。 它定義哪些`tcp`輸出要求可以&#x200B;_離開_ Adobe Commerce網站。 這稱為輸出篩選。 傳出防火牆會篩選可以匯出的內容 — 退出或逸出您的網站。 限制可以逸出的內容，為您的伺服器新增強大的安全性工具。

## 預設限制原則

防火牆提供兩種預設原則來控制輸出流量： `allow`和`deny`。 根據預設，`allow`原則&#x200B;_允許_&#x200B;所有輸出流量。 而且`deny`原則&#x200B;_預設會拒絕_&#x200B;所有輸出流量。 但當您新增規則時，預設原則會被覆寫，且防火牆會封鎖規則不允許的&#x200B;**所有**&#x200B;傳出流量。

對於入門計畫，預設原則設定為`allow`。 此設定可確保在您新增輸出篩選規則之前，所有目前的輸出流量保持未封鎖狀態。 預設原則可依要求設為`deny`。

**若要檢查您的預設原則**：

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

除非您要求您原則的`deny`，否則命令應該顯示您的原則設定為`allow`：

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**金鑰帶走**：新增輸出規則時，會封鎖所有輸出流量，但網域、IP位址或新增至規則的連線埠除外。 因此，在將完整的傳出清單新增至生產網站之前，請務必定義並測試該清單。

## 防火牆選項

`.magento.app.yaml`檔案中的下列設定範例顯示您可用來新增輸出篩選規則的所有`firewall`選項。

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## 輸出篩選規則

輸出防火牆設定是由規則所組成。 您可以視需要定義任意數量的規則。 規則的需求如下。

**每個規則：**

- 開頭必須是連字型大小(`-`)。 在同一行新增註解有助於以視覺化方式將一個規則與下一個規則區分開來。
- 必須至少定義下列其中一個選項： `domains`、`ips`或`ports`。
- 必須使用`tcp`通訊協定。 因為這是所有規則的預設通訊協定，所以您可以從規則中省略它。
- 可以在相同規則中定義`domains`或`ips`，但不能同時定義兩者。
- 可以包含`yaml`個註解(`#`)和分行符號，以組織允許的網域、IP位址和連線埠。

每個規則都使用下列屬性：

### `domains`

`domains`選項允許完整網域名稱(FQDN)的清單。

如果規則定義`domains`但不定義`ports`，防火牆會允許任何連線埠上的網域要求。

### `ips`

`ips`選項允許以CIDR標籤法列出IP位址。 您可以指定單一IP位址或IP位址範圍。

若要指定單一IP位址，請將`/32` CIDR首碼新增至您的IP位址結尾：

```
172.217.11.174/32  # google.com
```

若要指定IP位址的範圍，請使用[IP範圍到CIDR](https://ipaddressguide.com/cidr)計算器。

如果規則定義`ips`但不定義`ports`，防火牆會允許任何連線埠上的IP要求。

### `ports`

`ports`選項允許從1到65535的連線埠清單。 對於範例中的大部分規則，連線埠`80`和`443`都允許HTTP和HTTPS要求。 但對於New Relic，規則僅允許存取連線埠`443`上的網域和IP位址，如[網路流量](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents)的New Relic檔案所建議。

如果規則只定義`ports`，防火牆會允許存取定義之連線埠的所有網域和IP位址。

>[!NOTE]
>
>通訊埠`25` （傳送電子郵件的SMTP通訊埠）一律會遭到封鎖，沒有例外。

### `protocol`

如前所述，TCP是預設值，且僅允許用於規則的通訊協定。 不允許UDP及其連線埠。 因此，您可以從所有規則中省略`protocol`選項。 如果仍要包含它，則必須將值設為`tcp`，如範例的第一個規則所示。

## 尋找要允許的網域名稱

若要協助您識別要包含在輸出篩選規則中的網域，請使用下列命令來剖析伺服器的`dns.log`檔案，並顯示您的網站已記錄的所有DNS要求清單：

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

這個命令也會顯示已發出但遭到輸出篩選規則封鎖的DNS要求。 輸出不會顯示哪些網域遭到封鎖，只會顯示已提出的要求。 輸出不會顯示使用IP位址發出的要求。

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

網域與IP位址相反，通常在出口篩選方面更具體和安全。 例如，如果您為使用CDN的服務新增IP位址，則會允許CDN的IP位址，而該位址可供數百或數千個其他網域使用。 只要使用一個IP位址，您就可以允許對數千部其他伺服器的輸出存取。

## 測試輸出篩選規則

收集並設定您網站需求之網域和IP位址的存取規則後，就可以開始推播和測試。

若要測試輸出篩選規則：

1. 建立`curl`命令的Shell指令碼，以存取您規則中的網域和IP位址。 包含測試應封鎖之網域和IP位址存取權的命令。

1. 在您的`.magento.app.yaml`檔案中設定`post_deploy`連結以執行指令碼。

1. 將您的`firewall`設定和測試指令碼推送到`integration`分支。

1. 檢查您`curl`命令的`post_deploy`輸出。

1. 調整您的`firewall`規則、更新`curl`指令碼、認可、推播和重複。

### `curl`指令碼範例

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy`範例

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
