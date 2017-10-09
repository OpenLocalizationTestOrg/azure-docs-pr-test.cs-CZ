---
title: "aaaMonitor a spravovat Hadoop pomocí Ambari REST API – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Ambari toomonitor a správě clusterů systému Hadoop v prostředí Azure HDInsight. V tomto dokumentu se dozvíte, jak toouse hello Ambari REST API, která je součástí HDInsight clustery."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 1866a77c8e402231bccbcfba7174253aca41339b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-rest-api"></a>Správa clusterů HDInsight pomocí Ambari REST API hello

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Zjistěte, jak toouse hello toomanage Ambari REST API a monitorování clusterů systému Hadoop v prostředí Azure HDInsight.

Apache Ambari zjednodušuje hello Správa a sledování clusteru Hadoop tím, že poskytuje snadno toouse webového uživatelského rozhraní a REST API. Ambari je obsažena v clusterech HDInsight, které používají operační systém Linux hello. Můžete použít Ambari toomonitor hello clusteru a udělat změny konfigurace.

## <a id="whatis"></a>Co je Ambari

[Apache Ambari](http://ambari.apache.org) poskytuje webové uživatelské rozhraní, které může být použité tooprovision, správu a sledování clusterů systému Hadoop. Vývojářům můžete integrovat tyto funkce do svých aplikací s použitím hello [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari je dostupné ve výchozím nastavení s clustery HDInsight se systémem Linux.

## <a name="how-toouse-hello-ambari-rest-api"></a>Jak toouse hello Ambari REST API

> [!IMPORTANT]
> Hello informace a příklady v tomto dokumentu vyžadují clusteru služby HDInsight, který používá operační systém Linux. Další informace najdete v tématu [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

Hello příklady v tomto dokumentu jsou uvedené pro prostředí Bourne hello (bash) a prostředí PowerShell. Hello bash, který se příklady byly testovány s GNU bash 4.3.11, ale by měly spolupracovat s další součásti pro Unix. Příklady prostředí PowerShell Hello byly testovány s PowerShell 5.0, ale by měla fungovat s prostředí PowerShell 3.0 nebo vyšší.

Pokud používáte hello __Bourne prostředí__ (Bash), musíte mít nainstalované tyto položky hello:

* [cURL](http://curl.haxx.se/): cURL je nástroj, který lze použít toowork pomocí rozhraní REST API z příkazového řádku hello. V tomto dokumentu je použité toocommunicate s hello Ambari REST API.

Jestli používáte Bash nebo prostředí PowerShell, musí také mít [jq](https://stedolan.github.io/jq/) nainstalována. Jq je nástroj pro práci s dokumenty JSON. Používá se v **všechny** hello příklady Bash a **jeden** hello příklady prostředí PowerShell.

### <a name="base-uri-for-ambari-rest-api"></a>Základní identifikátor URI pro Ambari Rest API

Hello základní identifikátor URI pro hello Ambari REST API v HDInsight je https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, kde **CLUSTERNAME** je hello názvem vašeho clusteru.

> [!IMPORTANT]
> Při plně kvalifikovaný název clusteru hello v hello domény (FQDN) je součástí názvu hello identifikátor URI (CLUSTERNAME.azurehdinsight.net) nerozlišuje, jsou ostatní události v hello URI malá a velká písmena. Například pokud je název clusteru `MyCluster`, jsou platné identifikátory URI hello následující:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> Hello následující identifikátory URI vrátí chybovou zprávu, protože není hello hello druhého výskytu textu hello název opravte případu.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Authentication

Připojení tooAmbari v HDInsight vyžaduje protokol HTTPS. Název účtu správce hello použít (výchozí hodnota hello je **správce**) a heslo, které jste zadali při vytváření clusteru.

## <a name="examples-authentication-and-parsing-json"></a>Příklady: Ověřování a analýza JSON

Hello následující příklady ukazují, jak toomake požadavek GET hello základní Ambari REST API:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> Hello Bash příklady v tomto dokumentu provést hello následující předpoklady:
>
> * hello výchozí hodnota je Hello přihlašovací jméno pro hello cluster `admin`.
> * `$PASSWORD`obsahuje hello heslo pro hello příkaz přihlášení HDInsight. Tuto hodnotu můžete nastavit pomocí `PASSWORD='mypassword'`.
> * `$CLUSTERNAME`obsahuje název hello hello clusteru. Tuto hodnotu můžete nastavit pomocí`set CLUSTERNAME='clustername'`

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> Příklady prostředí PowerShell Hello v tomto dokumentu provést hello následující předpoklady:
>
> * `$creds`je objekt přihlašovacích údajů, který obsahuje hello správce přihlašovací jméno a heslo pro hello cluster. Tuto hodnotu můžete nastavit pomocí `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` a poskytnout pověření hello po zobrazení výzvy.
> * `$clusterName`je řetězec, který obsahuje název hello hello clusteru. Tuto hodnotu můžete nastavit pomocí `$clusterName="clustername"`.

Oba příklady vrátit dokument JSON, který začíná informace podobné toohello následující ukázka:

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a>Analýza dat JSON

Hello následující příklad používá `jq` tooparse hello dokumentu JSON odpovědi a zobrazit pouze hello `health_report` informace z výsledků hello.

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

Prostředí PowerShell 3.0 a vyšší poskytuje hello `ConvertFrom-Json` rutiny, která převádí hello dokumentu JSON na objekt, který je snazší toowork s z prostředí PowerShell. Hello následující příklad používá `ConvertFrom-Json` toodisplay pouze hello `health_report` informace z výsledků hello.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Při většina příklady v tomto dokumentu `ConvertFrom-Json` toodisplay elementy z dokumentu odpovědi hello hello [Ambari aktualizace konfigurace](#example-update-ambari-configuration) příklad používá jq. Jq se používá v této tooconstruct příklad novou šablonu z dokumentu odpovědi JSON hello.

Úplný referenční hello REST API, najdete v části [Ambari API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-hello-fqdn-of-cluster-nodes"></a>Příklad: Získání hello plně kvalifikovaný název domény uzlů clusteru

Při práci s HDInsight, může být nutné tooknow hello plně kvalifikovaný název domény (FQDN) uzlu clusteru. Můžete snadno načíst hello plně kvalifikovaný název domény pro hello různé uzly v clusteru hello pomocí hello následující příklady:

* **Všechny uzly**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **HEAD uzly**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Pracovní uzly**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Uzly zookeeper**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-hello-internal-ip-address-of-cluster-nodes"></a>Příklad: Získat hello interní IP adresu z uzlů clusteru

> [!IMPORTANT]
> Hello IP adresy vrácený hello příklady v této části nejsou přímo přístupné přes hello Internetu. Jsou dostupné v rámci hello virtuální síť Azure, která obsahuje clusteru HDInsight hello.
>
> Další informace o práci s HDInsight a virtuální sítě najdete v tématu [možnosti rozšíření HDInsight pomocí vlastních Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

toofind hello IP adresu, musíte znát hello interní plně kvalifikovaný název domény (FQDN) hello uzly clusteru. Jakmile máte hello plně kvalifikovaný název domény, pak můžete získat adresu IP hello hello hostitele. Hello následující příklady nejprve vyhledat Ambari hello plně kvalifikovaný název domény všech uzlů hello hostitele, a poté dotazu Ambari hello IP adresu každého hostitele.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-hello-default-storage"></a>Příklad: Získat hello výchozí úložiště

Při vytváření clusteru služby HDInsight, musíte použít účet úložiště Azure nebo Data Lake Store jako hello výchozí úložiště clusteru hello. Můžete použít Ambari tooretrieve tyto informace po vytvoření clusteru hello. Například pokud chcete, aby kontejner toohello tooread a zápis dat mimo HDInsight.

Hello následující příklady načtení hello výchozí úložiště konfigurace z clusteru hello:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> Tyto příklady vrátit hello první použitá konfigurace toohello server (`service_config_version=1`) obsahující tyto informace. Pokud je načíst hodnotu, která byla změněna po vytvoření clusteru, může potřebovat verze konfigurace hello toolist a načíst hello nejnovějšího.

Vrácená hodnota Hello je podobné tooone hello následující příklady:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`– Tato hodnota značí, že tento cluster hello používá účet úložiště Azure pro výchozí úložiště. Hello `ACCOUNTNAME` hodnota je hello název účtu úložiště hello. Hello `CONTAINER` část je název hello hello kontejneru objektů blob v účtu úložiště hello. kontejner Hello je hello kořenovém hello HDFS kompatibilní úložiště pro hello cluster.

* `adl://home`– Tato hodnota značí, že tento cluster hello používá pro výchozí úložiště Azure Data Lake Store.

    toofind hello název účtu Data Lake Store, použijte následující příklady hello:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    Hello návratová hodnota je podobný příliš`ACCOUNTNAME.azuredatalakestore.net`, kde `ACCOUNTNAME` je název hello hello účtu Data Lake Store.

    adresář hello toofind v Data Lake Store, který obsahuje hello úložiště pro cluster hello hello použijte následující příklady:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    Hello návratová hodnota je podobný příliš`/clusters/CLUSTERNAME/`. Tato hodnota je cestu v rámci hello účtu Data Lake Store. Tato cesta je hello kořenovém hello systém HDFS kompatibilní souborů pro hello cluster. 

> [!NOTE]
> Hello `Get-AzureRmHDInsightCluster` rutiny poskytované [prostředí Azure PowerShell](/powershell/azure/overview) také hello vrátí informace o úložiště pro hello cluster.


## <a name="example-get-configuration"></a>Příklad: Get konfigurace

1. Získáte hello konfigurace, které jsou k dispozici pro váš cluster.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Tento příklad vrátí dokumentu JSON obsahující aktuální konfiguraci hello (identifikovaný hello *značky* hodnotu) pro hello součásti nainstalovat na clusteru hello. Hello následující příklad je výňatek ze hello data vrácená z typu clusteru Spark.
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. Získáte hello konfiguraci pro součást hello, která vás zajímá. Následující příklad, nahraďte v hello `INITIAL` s hello Příznak Hodnota vrácená z hello předchozí požadavek.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Tento příklad vrátí dokumentu JSON, který obsahuje aktuální konfiguraci hello hello `core-site` součásti.

## <a name="example-update-configuration"></a>Příklad: Aktualizace konfigurace

1. Získejte aktuální konfiguraci hello, která Ambari ukládá jako hello "požadované konfigurace":

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Tento příklad vrátí dokumentu JSON obsahující aktuální konfiguraci hello (identifikovaný hello *značky* hodnotu) pro hello součásti nainstalovat na clusteru hello. Hello následující příklad je výňatek ze hello data vrácená z typu clusteru Spark.
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    Z tohoto seznamu, je třeba název hello toocopy hello součásti (například **spark\_thrift\_sparkconf** a hello **značky** hodnotu.

2. Načtení hello konfigurace pro součást hello a značky pomocí hello následující příkazy:
   
    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > Nahraďte **spark thrift-sparkconf** a **počáteční** pomocí součásti hello a značky, který chcete tooretrieve hello konfigurace pro.
   
    Jq je použité tooturn hello data načtená z HDInsight do nové šablony konfigurace. Konkrétně proveďte tyto příklady hello následující akce:
   
    * Vytvoří jedinečnou hodnotu obsahující hello řetězec "verze" a hello data, která je uložena v `newtag`.

    * Vytvoří dokument kořenové pro hello nové požadované konfigurace.

    * Získá hello obsah hello `.items[]` pole a přidává ji pod hello **desired_config** element.

    * Odstranění hello `href`, `version`, a `Config` prvky, jako tyto prvky nejsou potřebné toosubmit novou konfiguraci.

    * Přidá `tag` element s hodnotou `version#################`. číselnou část Hello je založena na hello aktuální datum. Každá konfigurace musí mít jedinečný kód.
     
    Nakonec hello budou uložena data toohello `newconfig.json` dokumentu. Struktura dokumentu Hello by měla vypadat podobně jako toohello následující ukázka:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Otevřete hello `newconfig.json` dokumentu a změnit nebo přidat hodnoty v hello `properties` objektu. Hello následující příklad změny hello hodnotu `"spark.yarn.am.memory"` z `"1g"` příliš`"3g"`. Přidává také `"spark.kryoserializer.buffer.max"` s hodnotou `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Jakmile dokončíte provedení změny, uložte soubor hello.

4. Použijte následující příkazy toosubmit hello aktualizovat konfiguraci tooAmbari hello.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Tyto příkazy odeslat obsah hello hello **newconfig.json** souborů toohello clusteru, jako jsou třeba konfigurace nové požadovaného hello. žádost o Hello vrátí dokumentu JSON. Hello **versionTag** element v tomto dokumentu by měl odpovídat verzi hello odeslání a hello **konfigurací** objekt obsahuje změny konfigurace hello požadujete.

### <a name="example-restart-a-service-component"></a>Příklad: Restartovat součást služby

Nyní když se podíváte na webovému uživatelskému rozhraní Ambari hello, hello službu Spark ukazuje, že ji vyžaduje toobe hello novou konfiguraci můžete projeví až po restartování. Pomocí následujících kroků toorestart hello služby hello.

1. Použijte následující tooenable režimu údržby pro hello službu Spark hello:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Tyto příkazy Odeslat server toohello dokumentu JSON, který zapne režimu údržby. Můžete ověřit, hello služby je nyní v režimu údržby pomocí hello následující požadavek:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    Hello vrácená hodnota je `ON`.

2. Pak pomocí hello následující tooturn vypnout hello služby:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    odpověď Hello je podobné toohello následující ukázka:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > Hello `href` hodnoty vrácené tento identifikátor URI používá hello interní IP adresu hello uzlu clusteru. toouse hello plně kvalifikovaný název domény clusteru hello z mimo hello clusteru, nahraďte část hello '10.0.0.18:8080'. 
    
    Následující příkazy Hello načíst stav hello hello žádosti:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Odpověď z `COMPLETED` označuje dokončení této žádosti hello.

3. Po dokončení předchozí požadavek hello, použijte následující služby hello toostart hello.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    Služba Hello teď používá hello novou konfiguraci.

4. Nakonec použijte hello následující tooturn vypnout režimu údržby.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Další kroky

Úplný referenční hello REST API, najdete v části [Ambari API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

