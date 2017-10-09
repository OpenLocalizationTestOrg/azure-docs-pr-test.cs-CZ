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
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-rest-api"></a><span data-ttu-id="fe060-104">Správa clusterů HDInsight pomocí Ambari REST API hello</span><span class="sxs-lookup"><span data-stu-id="fe060-104">Manage HDInsight clusters by using hello Ambari REST API</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="fe060-105">Zjistěte, jak toouse hello toomanage Ambari REST API a monitorování clusterů systému Hadoop v prostředí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fe060-105">Learn how toouse hello Ambari REST API toomanage and monitor Hadoop clusters in Azure HDInsight.</span></span>

<span data-ttu-id="fe060-106">Apache Ambari zjednodušuje hello Správa a sledování clusteru Hadoop tím, že poskytuje snadno toouse webového uživatelského rozhraní a REST API.</span><span class="sxs-lookup"><span data-stu-id="fe060-106">Apache Ambari simplifies hello management and monitoring of a Hadoop cluster by providing an easy toouse web UI and REST API.</span></span> <span data-ttu-id="fe060-107">Ambari je obsažena v clusterech HDInsight, které používají operační systém Linux hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-107">Ambari is included on HDInsight clusters that use hello Linux operating system.</span></span> <span data-ttu-id="fe060-108">Můžete použít Ambari toomonitor hello clusteru a udělat změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fe060-108">You can use Ambari toomonitor hello cluster and make configuration changes.</span></span>

## <span data-ttu-id="fe060-109"><a id="whatis"></a>Co je Ambari</span><span class="sxs-lookup"><span data-stu-id="fe060-109"><a id="whatis"></a>What is Ambari</span></span>

<span data-ttu-id="fe060-110">[Apache Ambari](http://ambari.apache.org) poskytuje webové uživatelské rozhraní, které může být použité tooprovision, správu a sledování clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="fe060-110">[Apache Ambari](http://ambari.apache.org) provides web UI that can be used tooprovision, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="fe060-111">Vývojářům můžete integrovat tyto funkce do svých aplikací s použitím hello [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe060-111">Developers can integrate these capabilities into their applications by using hello [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="fe060-112">Ambari je dostupné ve výchozím nastavení s clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="fe060-112">Ambari is provided by default with Linux-based HDInsight clusters.</span></span>

## <a name="how-toouse-hello-ambari-rest-api"></a><span data-ttu-id="fe060-113">Jak toouse hello Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="fe060-113">How toouse hello Ambari REST API</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe060-114">Hello informace a příklady v tomto dokumentu vyžadují clusteru služby HDInsight, který používá operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="fe060-114">hello information and examples in this document require an HDInsight cluster that uses Linux operating system.</span></span> <span data-ttu-id="fe060-115">Další informace najdete v tématu [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fe060-115">For more information, see [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

<span data-ttu-id="fe060-116">Hello příklady v tomto dokumentu jsou uvedené pro prostředí Bourne hello (bash) a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe060-116">hello examples in this document are provided for both hello Bourne shell (bash) and PowerShell.</span></span> <span data-ttu-id="fe060-117">Hello bash, který se příklady byly testovány s GNU bash 4.3.11, ale by měly spolupracovat s další součásti pro Unix.</span><span class="sxs-lookup"><span data-stu-id="fe060-117">hello bash examples were tested with GNU bash 4.3.11, but should work with other Unix shells.</span></span> <span data-ttu-id="fe060-118">Příklady prostředí PowerShell Hello byly testovány s PowerShell 5.0, ale by měla fungovat s prostředí PowerShell 3.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="fe060-118">hello PowerShell examples were tested with PowerShell 5.0, but should work with PowerShell 3.0 or higher.</span></span>

<span data-ttu-id="fe060-119">Pokud používáte hello __Bourne prostředí__ (Bash), musíte mít nainstalované tyto položky hello:</span><span class="sxs-lookup"><span data-stu-id="fe060-119">If using hello __Bourne shell__ (Bash), you must have hello following installed:</span></span>

* <span data-ttu-id="fe060-120">[cURL](http://curl.haxx.se/): cURL je nástroj, který lze použít toowork pomocí rozhraní REST API z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-120">[cURL](http://curl.haxx.se/): cURL is a utility that can be used toowork with REST APIs from hello command line.</span></span> <span data-ttu-id="fe060-121">V tomto dokumentu je použité toocommunicate s hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="fe060-121">In this document, it is used toocommunicate with hello Ambari REST API.</span></span>

<span data-ttu-id="fe060-122">Jestli používáte Bash nebo prostředí PowerShell, musí také mít [jq](https://stedolan.github.io/jq/) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="fe060-122">Whether using Bash or PowerShell, you must also have [jq](https://stedolan.github.io/jq/) installed.</span></span> <span data-ttu-id="fe060-123">Jq je nástroj pro práci s dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="fe060-123">Jq is a utility for working with JSON documents.</span></span> <span data-ttu-id="fe060-124">Používá se v **všechny** hello příklady Bash a **jeden** hello příklady prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe060-124">It is used in **all** hello Bash examples, and **one** of hello PowerShell examples.</span></span>

### <a name="base-uri-for-ambari-rest-api"></a><span data-ttu-id="fe060-125">Základní identifikátor URI pro Ambari Rest API</span><span class="sxs-lookup"><span data-stu-id="fe060-125">Base URI for Ambari Rest API</span></span>

<span data-ttu-id="fe060-126">Hello základní identifikátor URI pro hello Ambari REST API v HDInsight je https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, kde **CLUSTERNAME** je hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-126">hello base URI for hello Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe060-127">Při plně kvalifikovaný název clusteru hello v hello domény (FQDN) je součástí názvu hello identifikátor URI (CLUSTERNAME.azurehdinsight.net) nerozlišuje, jsou ostatní události v hello URI malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="fe060-127">While hello cluster name in hello fully qualified domain name (FQDN) part of hello URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in hello URI are case-sensitive.</span></span> <span data-ttu-id="fe060-128">Například pokud je název clusteru `MyCluster`, jsou platné identifikátory URI hello následující:</span><span class="sxs-lookup"><span data-stu-id="fe060-128">For example, if your cluster is named `MyCluster`, hello following are valid URIs:</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> <span data-ttu-id="fe060-129">Hello následující identifikátory URI vrátí chybovou zprávu, protože není hello hello druhého výskytu textu hello název opravte případu.</span><span class="sxs-lookup"><span data-stu-id="fe060-129">hello following URIs return an error because hello second occurrence of hello name is not hello correct case.</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a><span data-ttu-id="fe060-130">Authentication</span><span class="sxs-lookup"><span data-stu-id="fe060-130">Authentication</span></span>

<span data-ttu-id="fe060-131">Připojení tooAmbari v HDInsight vyžaduje protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fe060-131">Connecting tooAmbari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="fe060-132">Název účtu správce hello použít (výchozí hodnota hello je **správce**) a heslo, které jste zadali při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-132">Use hello admin account name (hello default is **admin**) and password you provided during cluster creation.</span></span>

## <a name="examples-authentication-and-parsing-json"></a><span data-ttu-id="fe060-133">Příklady: Ověřování a analýza JSON</span><span class="sxs-lookup"><span data-stu-id="fe060-133">Examples: Authentication and parsing JSON</span></span>

<span data-ttu-id="fe060-134">Hello následující příklady ukazují, jak toomake požadavek GET hello základní Ambari REST API:</span><span class="sxs-lookup"><span data-stu-id="fe060-134">hello following examples demonstrate how toomake a GET request against hello base Ambari REST API:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> <span data-ttu-id="fe060-135">Hello Bash příklady v tomto dokumentu provést hello následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="fe060-135">hello Bash examples in this document make hello following assumptions:</span></span>
>
> * <span data-ttu-id="fe060-136">hello výchozí hodnota je Hello přihlašovací jméno pro hello cluster `admin`.</span><span class="sxs-lookup"><span data-stu-id="fe060-136">hello login name for hello cluster is hello default value of `admin`.</span></span>
> * <span data-ttu-id="fe060-137">`$PASSWORD`obsahuje hello heslo pro hello příkaz přihlášení HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fe060-137">`$PASSWORD` contains hello password for hello HDInsight login command.</span></span> <span data-ttu-id="fe060-138">Tuto hodnotu můžete nastavit pomocí `PASSWORD='mypassword'`.</span><span class="sxs-lookup"><span data-stu-id="fe060-138">You can set this value by using `PASSWORD='mypassword'`.</span></span>
> * <span data-ttu-id="fe060-139">`$CLUSTERNAME`obsahuje název hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-139">`$CLUSTERNAME` contains hello name of hello cluster.</span></span> <span data-ttu-id="fe060-140">Tuto hodnotu můžete nastavit pomocí`set CLUSTERNAME='clustername'`</span><span class="sxs-lookup"><span data-stu-id="fe060-140">You can set this value by using `set CLUSTERNAME='clustername'`</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> <span data-ttu-id="fe060-141">Příklady prostředí PowerShell Hello v tomto dokumentu provést hello následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="fe060-141">hello PowerShell examples in this document make hello following assumptions:</span></span>
>
> * <span data-ttu-id="fe060-142">`$creds`je objekt přihlašovacích údajů, který obsahuje hello správce přihlašovací jméno a heslo pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="fe060-142">`$creds` is a credential object that contains hello admin login and password for hello cluster.</span></span> <span data-ttu-id="fe060-143">Tuto hodnotu můžete nastavit pomocí `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` a poskytnout pověření hello po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="fe060-143">You can set this value by using `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` and providing hello credentials when prompted.</span></span>
> * <span data-ttu-id="fe060-144">`$clusterName`je řetězec, který obsahuje název hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-144">`$clusterName` is a string that contains hello name of hello cluster.</span></span> <span data-ttu-id="fe060-145">Tuto hodnotu můžete nastavit pomocí `$clusterName="clustername"`.</span><span class="sxs-lookup"><span data-stu-id="fe060-145">You can set this value by using `$clusterName="clustername"`.</span></span>

<span data-ttu-id="fe060-146">Oba příklady vrátit dokument JSON, který začíná informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fe060-146">Both examples return a JSON document that begins with information similar toohello following example:</span></span>

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

### <a name="parsing-json-data"></a><span data-ttu-id="fe060-147">Analýza dat JSON</span><span class="sxs-lookup"><span data-stu-id="fe060-147">Parsing JSON data</span></span>

<span data-ttu-id="fe060-148">Hello následující příklad používá `jq` tooparse hello dokumentu JSON odpovědi a zobrazit pouze hello `health_report` informace z výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-148">hello following example uses `jq` tooparse hello JSON response document and display only hello `health_report` information from hello results.</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

<span data-ttu-id="fe060-149">Prostředí PowerShell 3.0 a vyšší poskytuje hello `ConvertFrom-Json` rutiny, která převádí hello dokumentu JSON na objekt, který je snazší toowork s z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe060-149">PowerShell 3.0 and higher provides hello `ConvertFrom-Json` cmdlet, which converts hello JSON document into an object that is easier toowork with from PowerShell.</span></span> <span data-ttu-id="fe060-150">Hello následující příklad používá `ConvertFrom-Json` toodisplay pouze hello `health_report` informace z výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-150">hello following example uses `ConvertFrom-Json` toodisplay only hello `health_report` information from hello results.</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> <span data-ttu-id="fe060-151">Při většina příklady v tomto dokumentu `ConvertFrom-Json` toodisplay elementy z dokumentu odpovědi hello hello [Ambari aktualizace konfigurace](#example-update-ambari-configuration) příklad používá jq.</span><span class="sxs-lookup"><span data-stu-id="fe060-151">While most examples in this document use `ConvertFrom-Json` toodisplay elements from hello response document, hello [Update Ambari configuration](#example-update-ambari-configuration) example uses jq.</span></span> <span data-ttu-id="fe060-152">Jq se používá v této tooconstruct příklad novou šablonu z dokumentu odpovědi JSON hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-152">Jq is used in this example tooconstruct a new template from hello JSON response document.</span></span>

<span data-ttu-id="fe060-153">Úplný referenční hello REST API, najdete v části [Ambari API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe060-153">For a complete reference of hello REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

## <a name="example-get-hello-fqdn-of-cluster-nodes"></a><span data-ttu-id="fe060-154">Příklad: Získání hello plně kvalifikovaný název domény uzlů clusteru</span><span class="sxs-lookup"><span data-stu-id="fe060-154">Example: Get hello FQDN of cluster nodes</span></span>

<span data-ttu-id="fe060-155">Při práci s HDInsight, může být nutné tooknow hello plně kvalifikovaný název domény (FQDN) uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-155">When working with HDInsight, you may need tooknow hello fully qualified domain name (FQDN) of a cluster node.</span></span> <span data-ttu-id="fe060-156">Můžete snadno načíst hello plně kvalifikovaný název domény pro hello různé uzly v clusteru hello pomocí hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="fe060-156">You can easily retrieve hello FQDN for hello various nodes in hello cluster using hello following examples:</span></span>

* <span data-ttu-id="fe060-157">**Všechny uzly**</span><span class="sxs-lookup"><span data-stu-id="fe060-157">**All nodes**</span></span>

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

* <span data-ttu-id="fe060-158">**HEAD uzly**</span><span class="sxs-lookup"><span data-stu-id="fe060-158">**Head nodes**</span></span>

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

* <span data-ttu-id="fe060-159">**Pracovní uzly**</span><span class="sxs-lookup"><span data-stu-id="fe060-159">**Worker nodes**</span></span>

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

* <span data-ttu-id="fe060-160">**Uzly zookeeper**</span><span class="sxs-lookup"><span data-stu-id="fe060-160">**Zookeeper nodes**</span></span>

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

## <a name="example-get-hello-internal-ip-address-of-cluster-nodes"></a><span data-ttu-id="fe060-161">Příklad: Získat hello interní IP adresu z uzlů clusteru</span><span class="sxs-lookup"><span data-stu-id="fe060-161">Example: Get hello internal IP address of cluster nodes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe060-162">Hello IP adresy vrácený hello příklady v této části nejsou přímo přístupné přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="fe060-162">hello IP addresses returned by hello examples in this section are not directly accessible over hello internet.</span></span> <span data-ttu-id="fe060-163">Jsou dostupné v rámci hello virtuální síť Azure, která obsahuje clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-163">They are only accessible within hello Azure Virtual Network that contains hello HDInsight cluster.</span></span>
>
> <span data-ttu-id="fe060-164">Další informace o práci s HDInsight a virtuální sítě najdete v tématu [možnosti rozšíření HDInsight pomocí vlastních Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="fe060-164">For more information on working with HDInsight and virtual networks, see [Extend HDInsight capabilities by using a custom Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span></span>

<span data-ttu-id="fe060-165">toofind hello IP adresu, musíte znát hello interní plně kvalifikovaný název domény (FQDN) hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-165">toofind hello IP address, you must know hello internal fully qualified domain name (FQDN) of hello cluster nodes.</span></span> <span data-ttu-id="fe060-166">Jakmile máte hello plně kvalifikovaný název domény, pak můžete získat adresu IP hello hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="fe060-166">Once you have hello FQDN, you can then get hello IP address of hello host.</span></span> <span data-ttu-id="fe060-167">Hello následující příklady nejprve vyhledat Ambari hello plně kvalifikovaný název domény všech uzlů hello hostitele, a poté dotazu Ambari hello IP adresu každého hostitele.</span><span class="sxs-lookup"><span data-stu-id="fe060-167">hello following examples first query Ambari for hello FQDN of all hello host nodes, then query Ambari for hello IP address of each host.</span></span>

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

## <a name="example-get-hello-default-storage"></a><span data-ttu-id="fe060-168">Příklad: Získat hello výchozí úložiště</span><span class="sxs-lookup"><span data-stu-id="fe060-168">Example: Get hello default storage</span></span>

<span data-ttu-id="fe060-169">Při vytváření clusteru služby HDInsight, musíte použít účet úložiště Azure nebo Data Lake Store jako hello výchozí úložiště clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-169">When you create an HDInsight cluster, you must use an Azure Storage Account or Data Lake Store as hello default storage for hello cluster.</span></span> <span data-ttu-id="fe060-170">Můžete použít Ambari tooretrieve tyto informace po vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-170">You can use Ambari tooretrieve this information after hello cluster has been created.</span></span> <span data-ttu-id="fe060-171">Například pokud chcete, aby kontejner toohello tooread a zápis dat mimo HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fe060-171">For example, if you want tooread/write data toohello container outside HDInsight.</span></span>

<span data-ttu-id="fe060-172">Hello následující příklady načtení hello výchozí úložiště konfigurace z clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="fe060-172">hello following examples retrieve hello default storage configuration from hello cluster:</span></span>

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
> <span data-ttu-id="fe060-173">Tyto příklady vrátit hello první použitá konfigurace toohello server (`service_config_version=1`) obsahující tyto informace.</span><span class="sxs-lookup"><span data-stu-id="fe060-173">These examples return hello first configuration applied toohello server (`service_config_version=1`) which contains this information.</span></span> <span data-ttu-id="fe060-174">Pokud je načíst hodnotu, která byla změněna po vytvoření clusteru, může potřebovat verze konfigurace hello toolist a načíst hello nejnovějšího.</span><span class="sxs-lookup"><span data-stu-id="fe060-174">If you retrieve a value that has been modified after cluster creation, you may need toolist hello configuration versions and retrieve hello latest one.</span></span>

<span data-ttu-id="fe060-175">Vrácená hodnota Hello je podobné tooone hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="fe060-175">hello return value is similar tooone of hello following examples:</span></span>

* <span data-ttu-id="fe060-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`– Tato hodnota značí, že tento cluster hello používá účet úložiště Azure pro výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe060-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - This value indicates that hello cluster is using an Azure Storage account for default storage.</span></span> <span data-ttu-id="fe060-177">Hello `ACCOUNTNAME` hodnota je hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-177">hello `ACCOUNTNAME` value is hello name of hello storage account.</span></span> <span data-ttu-id="fe060-178">Hello `CONTAINER` část je název hello hello kontejneru objektů blob v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-178">hello `CONTAINER` portion is hello name of hello blob container in hello storage account.</span></span> <span data-ttu-id="fe060-179">kontejner Hello je hello kořenovém hello HDFS kompatibilní úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="fe060-179">hello container is hello root of hello HDFS compatible storage for hello cluster.</span></span>

* <span data-ttu-id="fe060-180">`adl://home`– Tato hodnota značí, že tento cluster hello používá pro výchozí úložiště Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fe060-180">`adl://home` - This value indicates that hello cluster is using an Azure Data Lake Store for default storage.</span></span>

    <span data-ttu-id="fe060-181">toofind hello název účtu Data Lake Store, použijte následující příklady hello:</span><span class="sxs-lookup"><span data-stu-id="fe060-181">toofind hello Data Lake Store account name, use hello following examples:</span></span>

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

    <span data-ttu-id="fe060-182">Hello návratová hodnota je podobný příliš`ACCOUNTNAME.azuredatalakestore.net`, kde `ACCOUNTNAME` je název hello hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fe060-182">hello return value is similar too`ACCOUNTNAME.azuredatalakestore.net`, where `ACCOUNTNAME` is hello name of hello Data Lake Store account.</span></span>

    <span data-ttu-id="fe060-183">adresář hello toofind v Data Lake Store, který obsahuje hello úložiště pro cluster hello hello použijte následující příklady:</span><span class="sxs-lookup"><span data-stu-id="fe060-183">toofind hello directory within Data Lake Store that contains hello storage for hello cluster, use hello following examples:</span></span>

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

    <span data-ttu-id="fe060-184">Hello návratová hodnota je podobný příliš`/clusters/CLUSTERNAME/`.</span><span class="sxs-lookup"><span data-stu-id="fe060-184">hello return value is similar too`/clusters/CLUSTERNAME/`.</span></span> <span data-ttu-id="fe060-185">Tato hodnota je cestu v rámci hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fe060-185">This value is a path within hello Data Lake Store account.</span></span> <span data-ttu-id="fe060-186">Tato cesta je hello kořenovém hello systém HDFS kompatibilní souborů pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="fe060-186">This path is hello root of hello HDFS compatible file system for hello cluster.</span></span> 

> [!NOTE]
> <span data-ttu-id="fe060-187">Hello `Get-AzureRmHDInsightCluster` rutiny poskytované [prostředí Azure PowerShell](/powershell/azure/overview) také hello vrátí informace o úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="fe060-187">hello `Get-AzureRmHDInsightCluster` cmdlet provided by [Azure PowerShell](/powershell/azure/overview) also returns hello storage information for hello cluster.</span></span>


## <a name="example-get-configuration"></a><span data-ttu-id="fe060-188">Příklad: Get konfigurace</span><span class="sxs-lookup"><span data-stu-id="fe060-188">Example: Get configuration</span></span>

1. <span data-ttu-id="fe060-189">Získáte hello konfigurace, které jsou k dispozici pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="fe060-189">Get hello configurations that are available for your cluster.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    <span data-ttu-id="fe060-190">Tento příklad vrátí dokumentu JSON obsahující aktuální konfiguraci hello (identifikovaný hello *značky* hodnotu) pro hello součásti nainstalovat na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-190">This example returns a JSON document containing hello current configuration (identified by hello *tag* value) for hello components installed on hello cluster.</span></span> <span data-ttu-id="fe060-191">Hello následující příklad je výňatek ze hello data vrácená z typu clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="fe060-191">hello following example is an excerpt from hello data returned from a Spark cluster type.</span></span>
   
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

2. <span data-ttu-id="fe060-192">Získáte hello konfiguraci pro součást hello, která vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="fe060-192">Get hello configuration for hello component that you are interested in.</span></span> <span data-ttu-id="fe060-193">Následující příklad, nahraďte v hello `INITIAL` s hello Příznak Hodnota vrácená z hello předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="fe060-193">In hello following example, replace `INITIAL` with hello tag value returned from hello previous request.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    <span data-ttu-id="fe060-194">Tento příklad vrátí dokumentu JSON, který obsahuje aktuální konfiguraci hello hello `core-site` součásti.</span><span class="sxs-lookup"><span data-stu-id="fe060-194">This example returns a JSON document containing hello current configuration for hello `core-site` component.</span></span>

## <a name="example-update-configuration"></a><span data-ttu-id="fe060-195">Příklad: Aktualizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="fe060-195">Example: Update configuration</span></span>

1. <span data-ttu-id="fe060-196">Získejte aktuální konfiguraci hello, která Ambari ukládá jako hello "požadované konfigurace":</span><span class="sxs-lookup"><span data-stu-id="fe060-196">Get hello current configuration, which Ambari stores as hello "desired configuration":</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    <span data-ttu-id="fe060-197">Tento příklad vrátí dokumentu JSON obsahující aktuální konfiguraci hello (identifikovaný hello *značky* hodnotu) pro hello součásti nainstalovat na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-197">This example returns a JSON document containing hello current configuration (identified by hello *tag* value) for hello components installed on hello cluster.</span></span> <span data-ttu-id="fe060-198">Hello následující příklad je výňatek ze hello data vrácená z typu clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="fe060-198">hello following example is an excerpt from hello data returned from a Spark cluster type.</span></span>
   
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
   
    <span data-ttu-id="fe060-199">Z tohoto seznamu, je třeba název hello toocopy hello součásti (například **spark\_thrift\_sparkconf** a hello **značky** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fe060-199">From this list, you need toocopy hello name of hello component (for example, **spark\_thrift\_sparkconf** and hello **tag** value.</span></span>

2. <span data-ttu-id="fe060-200">Načtení hello konfigurace pro součást hello a značky pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="fe060-200">Retrieve hello configuration for hello component and tag by using hello following commands:</span></span>
   
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
    > <span data-ttu-id="fe060-201">Nahraďte **spark thrift-sparkconf** a **počáteční** pomocí součásti hello a značky, který chcete tooretrieve hello konfigurace pro.</span><span class="sxs-lookup"><span data-stu-id="fe060-201">Replace **spark-thrift-sparkconf** and **INITIAL** with hello component and tag that you want tooretrieve hello configuration for.</span></span>
   
    <span data-ttu-id="fe060-202">Jq je použité tooturn hello data načtená z HDInsight do nové šablony konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fe060-202">Jq is used tooturn hello data retrieved from HDInsight into a new configuration template.</span></span> <span data-ttu-id="fe060-203">Konkrétně proveďte tyto příklady hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="fe060-203">Specifically, these examples perform hello following actions:</span></span>
   
    * <span data-ttu-id="fe060-204">Vytvoří jedinečnou hodnotu obsahující hello řetězec "verze" a hello data, která je uložena v `newtag`.</span><span class="sxs-lookup"><span data-stu-id="fe060-204">Creates a unique value containing hello string "version" and hello date, which is stored in `newtag`.</span></span>

    * <span data-ttu-id="fe060-205">Vytvoří dokument kořenové pro hello nové požadované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fe060-205">Creates a root document for hello new desired configuration.</span></span>

    * <span data-ttu-id="fe060-206">Získá hello obsah hello `.items[]` pole a přidává ji pod hello **desired_config** element.</span><span class="sxs-lookup"><span data-stu-id="fe060-206">Gets hello contents of hello `.items[]` array and adds it under hello **desired_config** element.</span></span>

    * <span data-ttu-id="fe060-207">Odstranění hello `href`, `version`, a `Config` prvky, jako tyto prvky nejsou potřebné toosubmit novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fe060-207">Deletes hello `href`, `version`, and `Config` elements, as these elements aren't needed toosubmit a new configuration.</span></span>

    * <span data-ttu-id="fe060-208">Přidá `tag` element s hodnotou `version#################`.</span><span class="sxs-lookup"><span data-stu-id="fe060-208">Adds a `tag` element with a value of `version#################`.</span></span> <span data-ttu-id="fe060-209">číselnou část Hello je založena na hello aktuální datum.</span><span class="sxs-lookup"><span data-stu-id="fe060-209">hello numeric portion is based on hello current date.</span></span> <span data-ttu-id="fe060-210">Každá konfigurace musí mít jedinečný kód.</span><span class="sxs-lookup"><span data-stu-id="fe060-210">Each configuration must have a unique tag.</span></span>
     
    <span data-ttu-id="fe060-211">Nakonec hello budou uložena data toohello `newconfig.json` dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fe060-211">Finally, hello data is saved toohello `newconfig.json` document.</span></span> <span data-ttu-id="fe060-212">Struktura dokumentu Hello by měla vypadat podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fe060-212">hello document structure should appear similar toohello following example:</span></span>
     
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

3. <span data-ttu-id="fe060-213">Otevřete hello `newconfig.json` dokumentu a změnit nebo přidat hodnoty v hello `properties` objektu.</span><span class="sxs-lookup"><span data-stu-id="fe060-213">Open hello `newconfig.json` document and modify/add values in hello `properties` object.</span></span> <span data-ttu-id="fe060-214">Hello následující příklad změny hello hodnotu `"spark.yarn.am.memory"` z `"1g"` příliš`"3g"`.</span><span class="sxs-lookup"><span data-stu-id="fe060-214">hello following example changes hello value of `"spark.yarn.am.memory"` from `"1g"` too`"3g"`.</span></span> <span data-ttu-id="fe060-215">Přidává také `"spark.kryoserializer.buffer.max"` s hodnotou `"256m"`.</span><span class="sxs-lookup"><span data-stu-id="fe060-215">It also adds `"spark.kryoserializer.buffer.max"` with a value of `"256m"`.</span></span>
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    <span data-ttu-id="fe060-216">Jakmile dokončíte provedení změny, uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-216">Save hello file once you are done making modifications.</span></span>

4. <span data-ttu-id="fe060-217">Použijte následující příkazy toosubmit hello aktualizovat konfiguraci tooAmbari hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-217">Use hello following commands toosubmit hello updated configuration tooAmbari.</span></span>
   
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
   
    <span data-ttu-id="fe060-218">Tyto příkazy odeslat obsah hello hello **newconfig.json** souborů toohello clusteru, jako jsou třeba konfigurace nové požadovaného hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-218">These commands submit hello contents of hello **newconfig.json** file toohello cluster as hello new desired configuration.</span></span> <span data-ttu-id="fe060-219">žádost o Hello vrátí dokumentu JSON.</span><span class="sxs-lookup"><span data-stu-id="fe060-219">hello request returns a JSON document.</span></span> <span data-ttu-id="fe060-220">Hello **versionTag** element v tomto dokumentu by měl odpovídat verzi hello odeslání a hello **konfigurací** objekt obsahuje změny konfigurace hello požadujete.</span><span class="sxs-lookup"><span data-stu-id="fe060-220">hello **versionTag** element in this document should match hello version you submitted, and hello **configs** object contains hello configuration changes you requested.</span></span>

### <a name="example-restart-a-service-component"></a><span data-ttu-id="fe060-221">Příklad: Restartovat součást služby</span><span class="sxs-lookup"><span data-stu-id="fe060-221">Example: Restart a service component</span></span>

<span data-ttu-id="fe060-222">Nyní když se podíváte na webovému uživatelskému rozhraní Ambari hello, hello službu Spark ukazuje, že ji vyžaduje toobe hello novou konfiguraci můžete projeví až po restartování.</span><span class="sxs-lookup"><span data-stu-id="fe060-222">At this point, if you look at hello Ambari web UI, hello Spark service indicates that it needs toobe restarted before hello new configuration can take effect.</span></span> <span data-ttu-id="fe060-223">Pomocí následujících kroků toorestart hello služby hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-223">Use hello following steps toorestart hello service.</span></span>

1. <span data-ttu-id="fe060-224">Použijte následující tooenable režimu údržby pro hello službu Spark hello:</span><span class="sxs-lookup"><span data-stu-id="fe060-224">Use hello following tooenable maintenance mode for hello Spark service:</span></span>

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
   
    <span data-ttu-id="fe060-225">Tyto příkazy Odeslat server toohello dokumentu JSON, který zapne režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="fe060-225">These commands send a JSON document toohello server that turns on maintenance mode.</span></span> <span data-ttu-id="fe060-226">Můžete ověřit, hello služby je nyní v režimu údržby pomocí hello následující požadavek:</span><span class="sxs-lookup"><span data-stu-id="fe060-226">You can verify that hello service is now in maintenance mode using hello following request:</span></span>
   
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
   
    <span data-ttu-id="fe060-227">Hello vrácená hodnota je `ON`.</span><span class="sxs-lookup"><span data-stu-id="fe060-227">hello return value is `ON`.</span></span>

2. <span data-ttu-id="fe060-228">Pak pomocí hello následující tooturn vypnout hello služby:</span><span class="sxs-lookup"><span data-stu-id="fe060-228">Next, use hello following tooturn off hello service:</span></span>

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
    
    <span data-ttu-id="fe060-229">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fe060-229">hello response is similar toohello following example:</span></span>
   
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
    > <span data-ttu-id="fe060-230">Hello `href` hodnoty vrácené tento identifikátor URI používá hello interní IP adresu hello uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="fe060-230">hello `href` value returned by this URI is using hello internal IP address of hello cluster node.</span></span> <span data-ttu-id="fe060-231">toouse hello plně kvalifikovaný název domény clusteru hello z mimo hello clusteru, nahraďte část hello '10.0.0.18:8080'.</span><span class="sxs-lookup"><span data-stu-id="fe060-231">toouse it from outside hello cluster, replace hello \`10.0.0.18:8080' portion with hello FQDN of hello cluster.</span></span> 
    
    <span data-ttu-id="fe060-232">Následující příkazy Hello načíst stav hello hello žádosti:</span><span class="sxs-lookup"><span data-stu-id="fe060-232">hello following commands retrieve hello status of hello request:</span></span>

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

    <span data-ttu-id="fe060-233">Odpověď z `COMPLETED` označuje dokončení této žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-233">A response of `COMPLETED` indicates that hello request has finished.</span></span>

3. <span data-ttu-id="fe060-234">Po dokončení předchozí požadavek hello, použijte následující služby hello toostart hello.</span><span class="sxs-lookup"><span data-stu-id="fe060-234">Once hello previous request completes, use hello following toostart hello service.</span></span>
   
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
    <span data-ttu-id="fe060-235">Služba Hello teď používá hello novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fe060-235">hello service is now using hello new configuration.</span></span>

4. <span data-ttu-id="fe060-236">Nakonec použijte hello následující tooturn vypnout režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="fe060-236">Finally, use hello following tooturn off maintenance mode.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="fe060-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe060-237">Next steps</span></span>

<span data-ttu-id="fe060-238">Úplný referenční hello REST API, najdete v části [Ambari API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe060-238">For a complete reference of hello REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

