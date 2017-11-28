---
title: "Sledování a správě Hadoop pomocí Ambari REST API – Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Ambari ke sledování a správě clusterů systému Hadoop v prostředí Azure HDInsight. V tomto dokumentu se dozvíte, jak pomocí Ambari REST API, která je součástí clusterů HDInsight."
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
ms.openlocfilehash: 7960d83bce22d4f671d61e9aaf55561bc24308f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a><span data-ttu-id="c4513-104">Správa clusterů HDInsight pomocí Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="c4513-104">Manage HDInsight clusters by using the Ambari REST API</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="c4513-105">Naučte se používat rozhraní Ambari REST API pro správu a sledování clusterů systému Hadoop v prostředí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4513-105">Learn how to use the Ambari REST API to manage and monitor Hadoop clusters in Azure HDInsight.</span></span>

<span data-ttu-id="c4513-106">Apache Ambari zjednodušuje správu a sledování clusteru Hadoop tím, že poskytuje snadno použít webového uživatelského rozhraní a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="c4513-106">Apache Ambari simplifies the management and monitoring of a Hadoop cluster by providing an easy to use web UI and REST API.</span></span> <span data-ttu-id="c4513-107">Ambari je obsažena v clusterech HDInsight, které používají operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="c4513-107">Ambari is included on HDInsight clusters that use the Linux operating system.</span></span> <span data-ttu-id="c4513-108">Ambari slouží ke sledování clusteru a udělat změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4513-108">You can use Ambari to monitor the cluster and make configuration changes.</span></span>

## <span data-ttu-id="c4513-109"><a id="whatis"></a>Co je Ambari</span><span class="sxs-lookup"><span data-stu-id="c4513-109"><a id="whatis"></a>What is Ambari</span></span>

<span data-ttu-id="c4513-110">[Apache Ambari](http://ambari.apache.org) poskytuje webové uživatelské rozhraní, které lze použít ke zřízení, správě a sledování clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c4513-110">[Apache Ambari](http://ambari.apache.org) provides web UI that can be used to provision, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="c4513-111">Vývojářům můžete integrovat tyto funkce do svých aplikací pomocí [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="c4513-111">Developers can integrate these capabilities into their applications by using the [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="c4513-112">Ambari je dostupné ve výchozím nastavení s clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c4513-112">Ambari is provided by default with Linux-based HDInsight clusters.</span></span>

## <a name="how-to-use-the-ambari-rest-api"></a><span data-ttu-id="c4513-113">Jak používat Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="c4513-113">How to use the Ambari REST API</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4513-114">Informace a příklady v tomto dokumentu vyžadují clusteru služby HDInsight, který používá operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="c4513-114">The information and examples in this document require an HDInsight cluster that uses Linux operating system.</span></span> <span data-ttu-id="c4513-115">Další informace najdete v tématu [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c4513-115">For more information, see [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

<span data-ttu-id="c4513-116">Příklady v tomto dokumentu jsou uvedeny pro prostředí Bourne (bash) a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4513-116">The examples in this document are provided for both the Bourne shell (bash) and PowerShell.</span></span> <span data-ttu-id="c4513-117">Bash, který se příklady byly testovány s GNU bash 4.3.11, ale by měly spolupracovat s další součásti pro Unix.</span><span class="sxs-lookup"><span data-stu-id="c4513-117">The bash examples were tested with GNU bash 4.3.11, but should work with other Unix shells.</span></span> <span data-ttu-id="c4513-118">Příklady prostředí PowerShell byly testovány s PowerShell 5.0, ale by měla fungovat s prostředí PowerShell 3.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="c4513-118">The PowerShell examples were tested with PowerShell 5.0, but should work with PowerShell 3.0 or higher.</span></span>

<span data-ttu-id="c4513-119">Pokud se používá __Bourne prostředí__ (Bash), musíte mít nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="c4513-119">If using the __Bourne shell__ (Bash), you must have the following installed:</span></span>

* <span data-ttu-id="c4513-120">[cURL](http://curl.haxx.se/): cURL je nástroj, který slouží k práci s rozhraními API REST z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c4513-120">[cURL](http://curl.haxx.se/): cURL is a utility that can be used to work with REST APIs from the command line.</span></span> <span data-ttu-id="c4513-121">V tomto dokumentu se používá ke komunikaci s Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="c4513-121">In this document, it is used to communicate with the Ambari REST API.</span></span>

<span data-ttu-id="c4513-122">Jestli používáte Bash nebo prostředí PowerShell, musí také mít [jq](https://stedolan.github.io/jq/) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="c4513-122">Whether using Bash or PowerShell, you must also have [jq](https://stedolan.github.io/jq/) installed.</span></span> <span data-ttu-id="c4513-123">Jq je nástroj pro práci s dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="c4513-123">Jq is a utility for working with JSON documents.</span></span> <span data-ttu-id="c4513-124">Používá se v **všechny** příklady Bash a **jeden** příklady prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4513-124">It is used in **all** the Bash examples, and **one** of the PowerShell examples.</span></span>

### <a name="base-uri-for-ambari-rest-api"></a><span data-ttu-id="c4513-125">Základní identifikátor URI pro Ambari Rest API</span><span class="sxs-lookup"><span data-stu-id="c4513-125">Base URI for Ambari Rest API</span></span>

<span data-ttu-id="c4513-126">Základní identifikátor URI pro Ambari REST API v HDInsight je https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, kde **CLUSTERNAME** je název clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-126">The base URI for the Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4513-127">Sice velká a malá písmena název clusteru v části název (FQDN) plně kvalifikované domény identifikátor URI (CLUSTERNAME.azurehdinsight.net), jsou ostatní události v identifikátoru URI malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="c4513-127">While the cluster name in the fully qualified domain name (FQDN) part of the URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in the URI are case-sensitive.</span></span> <span data-ttu-id="c4513-128">Například pokud je název clusteru `MyCluster`, platné identifikátory URI jsou následující:</span><span class="sxs-lookup"><span data-stu-id="c4513-128">For example, if your cluster is named `MyCluster`, the following are valid URIs:</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> <span data-ttu-id="c4513-129">Následující identifikátory URI vrátí chybovou zprávu, protože název druhého výskytu není správnou velikost.</span><span class="sxs-lookup"><span data-stu-id="c4513-129">The following URIs return an error because the second occurrence of the name is not the correct case.</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a><span data-ttu-id="c4513-130">Authentication</span><span class="sxs-lookup"><span data-stu-id="c4513-130">Authentication</span></span>

<span data-ttu-id="c4513-131">Připojení k Ambari v HDInsight vyžaduje protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c4513-131">Connecting to Ambari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="c4513-132">Použijte název účtu správce (výchozí hodnota je **správce**) a heslo, které jste zadali při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-132">Use the admin account name (the default is **admin**) and password you provided during cluster creation.</span></span>

## <a name="examples-authentication-and-parsing-json"></a><span data-ttu-id="c4513-133">Příklady: Ověřování a analýza JSON</span><span class="sxs-lookup"><span data-stu-id="c4513-133">Examples: Authentication and parsing JSON</span></span>

<span data-ttu-id="c4513-134">Následující příklady ukazují, jak vytvořit požadavek GET na základní Ambari REST API:</span><span class="sxs-lookup"><span data-stu-id="c4513-134">The following examples demonstrate how to make a GET request against the base Ambari REST API:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> <span data-ttu-id="c4513-135">Příklady Bash v tomto dokumentu provést následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="c4513-135">The Bash examples in this document make the following assumptions:</span></span>
>
> * <span data-ttu-id="c4513-136">Výchozí hodnota je přihlašovací jméno pro cluster `admin`.</span><span class="sxs-lookup"><span data-stu-id="c4513-136">The login name for the cluster is the default value of `admin`.</span></span>
> * <span data-ttu-id="c4513-137">`$PASSWORD`obsahuje heslo pro přihlášení příkaz HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4513-137">`$PASSWORD` contains the password for the HDInsight login command.</span></span> <span data-ttu-id="c4513-138">Tuto hodnotu můžete nastavit pomocí `PASSWORD='mypassword'`.</span><span class="sxs-lookup"><span data-stu-id="c4513-138">You can set this value by using `PASSWORD='mypassword'`.</span></span>
> * <span data-ttu-id="c4513-139">`$CLUSTERNAME`obsahuje název clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-139">`$CLUSTERNAME` contains the name of the cluster.</span></span> <span data-ttu-id="c4513-140">Tuto hodnotu můžete nastavit pomocí`set CLUSTERNAME='clustername'`</span><span class="sxs-lookup"><span data-stu-id="c4513-140">You can set this value by using `set CLUSTERNAME='clustername'`</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> <span data-ttu-id="c4513-141">Příklady prostředí PowerShell v tomto dokumentu provést následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="c4513-141">The PowerShell examples in this document make the following assumptions:</span></span>
>
> * <span data-ttu-id="c4513-142">`$creds`je objekt přihlašovacích údajů, který obsahuje přihlašovací jméno správce a heslo pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c4513-142">`$creds` is a credential object that contains the admin login and password for the cluster.</span></span> <span data-ttu-id="c4513-143">Tuto hodnotu můžete nastavit pomocí `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` a poskytování pověření při zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="c4513-143">You can set this value by using `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` and providing the credentials when prompted.</span></span>
> * <span data-ttu-id="c4513-144">`$clusterName`je řetězec, který obsahuje název clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-144">`$clusterName` is a string that contains the name of the cluster.</span></span> <span data-ttu-id="c4513-145">Tuto hodnotu můžete nastavit pomocí `$clusterName="clustername"`.</span><span class="sxs-lookup"><span data-stu-id="c4513-145">You can set this value by using `$clusterName="clustername"`.</span></span>

<span data-ttu-id="c4513-146">Oba příklady vrátit dokument JSON, který začíná informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c4513-146">Both examples return a JSON document that begins with information similar to the following example:</span></span>

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

### <a name="parsing-json-data"></a><span data-ttu-id="c4513-147">Analýza dat JSON</span><span class="sxs-lookup"><span data-stu-id="c4513-147">Parsing JSON data</span></span>

<span data-ttu-id="c4513-148">Následující příklad používá `jq` analyzovat odpověď dokumentu JSON a zobrazuje pouze `health_report` informace z výsledků.</span><span class="sxs-lookup"><span data-stu-id="c4513-148">The following example uses `jq` to parse the JSON response document and display only the `health_report` information from the results.</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

<span data-ttu-id="c4513-149">Prostředí PowerShell 3.0 a vyšší poskytuje `ConvertFrom-Json` rutiny, která převádí dokumentu JSON na objekt, který je snazší s ním pracovat z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4513-149">PowerShell 3.0 and higher provides the `ConvertFrom-Json` cmdlet, which converts the JSON document into an object that is easier to work with from PowerShell.</span></span> <span data-ttu-id="c4513-150">Následující příklad používá `ConvertFrom-Json` lze zobrazit pouze `health_report` informace z výsledků.</span><span class="sxs-lookup"><span data-stu-id="c4513-150">The following example uses `ConvertFrom-Json` to display only the `health_report` information from the results.</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> <span data-ttu-id="c4513-151">Při většina příklady v tomto dokumentu `ConvertFrom-Json` zobrazíte elementy z dokumentu odpovědi [Ambari aktualizace konfigurace](#example-update-ambari-configuration) příklad používá jq.</span><span class="sxs-lookup"><span data-stu-id="c4513-151">While most examples in this document use `ConvertFrom-Json` to display elements from the response document, the [Update Ambari configuration](#example-update-ambari-configuration) example uses jq.</span></span> <span data-ttu-id="c4513-152">Jq se používá v tomto příkladu můžete vytvořit novou šablonu z dokumentu JSON odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4513-152">Jq is used in this example to construct a new template from the JSON response document.</span></span>

<span data-ttu-id="c4513-153">Úplný přehled rozhraní REST API, najdete v části [Ambari API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="c4513-153">For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

## <a name="example-get-the-fqdn-of-cluster-nodes"></a><span data-ttu-id="c4513-154">Příklad: Získat plně kvalifikovaný název domény uzlů clusteru</span><span class="sxs-lookup"><span data-stu-id="c4513-154">Example: Get the FQDN of cluster nodes</span></span>

<span data-ttu-id="c4513-155">Při práci s HDInsight, musíte znát název plně kvalifikované domény (FQDN) uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-155">When working with HDInsight, you may need to know the fully qualified domain name (FQDN) of a cluster node.</span></span> <span data-ttu-id="c4513-156">Můžete snadno získat plně kvalifikovaný název domény pro různé uzly v clusteru pomocí následující příklady:</span><span class="sxs-lookup"><span data-stu-id="c4513-156">You can easily retrieve the FQDN for the various nodes in the cluster using the following examples:</span></span>

* <span data-ttu-id="c4513-157">**Všechny uzly**</span><span class="sxs-lookup"><span data-stu-id="c4513-157">**All nodes**</span></span>

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

* <span data-ttu-id="c4513-158">**HEAD uzly**</span><span class="sxs-lookup"><span data-stu-id="c4513-158">**Head nodes**</span></span>

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

* <span data-ttu-id="c4513-159">**Pracovní uzly**</span><span class="sxs-lookup"><span data-stu-id="c4513-159">**Worker nodes**</span></span>

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

* <span data-ttu-id="c4513-160">**Uzly zookeeper**</span><span class="sxs-lookup"><span data-stu-id="c4513-160">**Zookeeper nodes**</span></span>

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

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a><span data-ttu-id="c4513-161">Příklad: Získáte interní IP adresu z uzlů clusteru</span><span class="sxs-lookup"><span data-stu-id="c4513-161">Example: Get the internal IP address of cluster nodes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4513-162">IP adresy vrácené v příkladech v této části nejsou přímo přístupné přes internet.</span><span class="sxs-lookup"><span data-stu-id="c4513-162">The IP addresses returned by the examples in this section are not directly accessible over the internet.</span></span> <span data-ttu-id="c4513-163">Jsou dostupné v rámci virtuální sítě Azure, která obsahuje clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4513-163">They are only accessible within the Azure Virtual Network that contains the HDInsight cluster.</span></span>
>
> <span data-ttu-id="c4513-164">Další informace o práci s HDInsight a virtuální sítě najdete v tématu [možnosti rozšíření HDInsight pomocí vlastních Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="c4513-164">For more information on working with HDInsight and virtual networks, see [Extend HDInsight capabilities by using a custom Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span></span>

<span data-ttu-id="c4513-165">Chcete-li najít IP adresu, musíte znát interní plně kvalifikovaný název domény (FQDN) uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-165">To find the IP address, you must know the internal fully qualified domain name (FQDN) of the cluster nodes.</span></span> <span data-ttu-id="c4513-166">Jakmile máte plně kvalifikovaný název domény, pak můžete získat IP adresu hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4513-166">Once you have the FQDN, you can then get the IP address of the host.</span></span> <span data-ttu-id="c4513-167">Následující příklady nejprve dotaz Ambari pro všechny uzly hostitelského plně kvalifikovaný název domény, a poté dotaz Ambari pro IP adresu každého hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4513-167">The following examples first query Ambari for the FQDN of all the host nodes, then query Ambari for the IP address of each host.</span></span>

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

## <a name="example-get-the-default-storage"></a><span data-ttu-id="c4513-168">Příklad: Získat výchozí úložiště</span><span class="sxs-lookup"><span data-stu-id="c4513-168">Example: Get the default storage</span></span>

<span data-ttu-id="c4513-169">Při vytváření clusteru služby HDInsight, musíte použít účet úložiště Azure nebo Data Lake Store jako výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c4513-169">When you create an HDInsight cluster, you must use an Azure Storage Account or Data Lake Store as the default storage for the cluster.</span></span> <span data-ttu-id="c4513-170">Ambari slouží k načtení těchto informací po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-170">You can use Ambari to retrieve this information after the cluster has been created.</span></span> <span data-ttu-id="c4513-171">Například pokud chcete pro čtení a zápis dat do kontejneru mimo HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4513-171">For example, if you want to read/write data to the container outside HDInsight.</span></span>

<span data-ttu-id="c4513-172">Následující příklady načíst výchozí konfigurace úložiště z clusteru:</span><span class="sxs-lookup"><span data-stu-id="c4513-172">The following examples retrieve the default storage configuration from the cluster:</span></span>

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
> <span data-ttu-id="c4513-173">Tyto příklady vrátí první konfigurace platí na serveru (`service_config_version=1`) obsahující tyto informace.</span><span class="sxs-lookup"><span data-stu-id="c4513-173">These examples return the first configuration applied to the server (`service_config_version=1`) which contains this information.</span></span> <span data-ttu-id="c4513-174">Pokud je načíst hodnotu, která byla změněna po vytvoření clusteru, musíte do seznamu verze konfigurace a načíst nejnovější.</span><span class="sxs-lookup"><span data-stu-id="c4513-174">If you retrieve a value that has been modified after cluster creation, you may need to list the configuration versions and retrieve the latest one.</span></span>

<span data-ttu-id="c4513-175">Vrácená hodnota je podobný jedné z následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="c4513-175">The return value is similar to one of the following examples:</span></span>

* <span data-ttu-id="c4513-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`– Tato hodnota určuje, zda cluster používá účet úložiště Azure pro výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4513-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - This value indicates that the cluster is using an Azure Storage account for default storage.</span></span> <span data-ttu-id="c4513-177">`ACCOUNTNAME` Hodnota je název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4513-177">The `ACCOUNTNAME` value is the name of the storage account.</span></span> <span data-ttu-id="c4513-178">`CONTAINER` Část je název kontejneru objektů blob v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4513-178">The `CONTAINER` portion is the name of the blob container in the storage account.</span></span> <span data-ttu-id="c4513-179">Kontejner je kořenem HDFS kompatibilní úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c4513-179">The container is the root of the HDFS compatible storage for the cluster.</span></span>

* <span data-ttu-id="c4513-180">`adl://home`– Tato hodnota určuje, jestli cluster používá Azure Data Lake Store pro výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4513-180">`adl://home` - This value indicates that the cluster is using an Azure Data Lake Store for default storage.</span></span>

    <span data-ttu-id="c4513-181">Pokud chcete najít název účtu Data Lake Store, použijte následující příklady:</span><span class="sxs-lookup"><span data-stu-id="c4513-181">To find the Data Lake Store account name, use the following examples:</span></span>

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

    <span data-ttu-id="c4513-182">Vrácená hodnota je podobná `ACCOUNTNAME.azuredatalakestore.net`, kde `ACCOUNTNAME` je název účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c4513-182">The return value is similar to `ACCOUNTNAME.azuredatalakestore.net`, where `ACCOUNTNAME` is the name of the Data Lake Store account.</span></span>

    <span data-ttu-id="c4513-183">Pokud chcete najít adresář v Data Lake Store, který obsahuje úložiště pro cluster, použijte následující příklady:</span><span class="sxs-lookup"><span data-stu-id="c4513-183">To find the directory within Data Lake Store that contains the storage for the cluster, use the following examples:</span></span>

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

    <span data-ttu-id="c4513-184">Vrácená hodnota je podobná `/clusters/CLUSTERNAME/`.</span><span class="sxs-lookup"><span data-stu-id="c4513-184">The return value is similar to `/clusters/CLUSTERNAME/`.</span></span> <span data-ttu-id="c4513-185">Tato hodnota je cestu v rámci účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c4513-185">This value is a path within the Data Lake Store account.</span></span> <span data-ttu-id="c4513-186">Tato cesta je kořenový adresář systému HDFS kompatibilní souborů pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c4513-186">This path is the root of the HDFS compatible file system for the cluster.</span></span> 

> [!NOTE]
> <span data-ttu-id="c4513-187">`Get-AzureRmHDInsightCluster` Rutiny poskytované [prostředí Azure PowerShell](/powershell/azure/overview) také vrátí informace o úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c4513-187">The `Get-AzureRmHDInsightCluster` cmdlet provided by [Azure PowerShell](/powershell/azure/overview) also returns the storage information for the cluster.</span></span>


## <a name="example-get-configuration"></a><span data-ttu-id="c4513-188">Příklad: Get konfigurace</span><span class="sxs-lookup"><span data-stu-id="c4513-188">Example: Get configuration</span></span>

1. <span data-ttu-id="c4513-189">Získání konfigurace, které jsou k dispozici pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="c4513-189">Get the configurations that are available for your cluster.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    <span data-ttu-id="c4513-190">Tento příklad vrátí dokumentu JSON, který obsahuje aktuální konfiguraci (identifikovaný *značky* hodnotu) pro součásti nainstalované v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-190">This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster.</span></span> <span data-ttu-id="c4513-191">Následující příklad je výňatek ze s daty vrácenými z typu clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="c4513-191">The following example is an excerpt from the data returned from a Spark cluster type.</span></span>
   
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

2. <span data-ttu-id="c4513-192">Získáte konfiguraci pro součást, která vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="c4513-192">Get the configuration for the component that you are interested in.</span></span> <span data-ttu-id="c4513-193">V následujícím příkladu nahraďte `INITIAL` s hodnota značky vrácená z předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="c4513-193">In the following example, replace `INITIAL` with the tag value returned from the previous request.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    <span data-ttu-id="c4513-194">Tento příklad vrátí dokumentu JSON, který obsahuje aktuální konfiguraci `core-site` součásti.</span><span class="sxs-lookup"><span data-stu-id="c4513-194">This example returns a JSON document containing the current configuration for the `core-site` component.</span></span>

## <a name="example-update-configuration"></a><span data-ttu-id="c4513-195">Příklad: Aktualizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="c4513-195">Example: Update configuration</span></span>

1. <span data-ttu-id="c4513-196">Získejte aktuální konfiguraci, která Ambari ukládá jako "požadovanou konfiguraci":</span><span class="sxs-lookup"><span data-stu-id="c4513-196">Get the current configuration, which Ambari stores as the "desired configuration":</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    <span data-ttu-id="c4513-197">Tento příklad vrátí dokumentu JSON, který obsahuje aktuální konfiguraci (identifikovaný *značky* hodnotu) pro součásti nainstalované v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-197">This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster.</span></span> <span data-ttu-id="c4513-198">Následující příklad je výňatek ze s daty vrácenými z typu clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="c4513-198">The following example is an excerpt from the data returned from a Spark cluster type.</span></span>
   
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
   
    <span data-ttu-id="c4513-199">Z tohoto seznamu, je nutné zkopírovat název součásti (například **spark\_thrift\_sparkconf** a **značky** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c4513-199">From this list, you need to copy the name of the component (for example, **spark\_thrift\_sparkconf** and the **tag** value.</span></span>

2. <span data-ttu-id="c4513-200">Načíst konfiguraci pro součást a značky pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="c4513-200">Retrieve the configuration for the component and tag by using the following commands:</span></span>
   
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
    > <span data-ttu-id="c4513-201">Nahraďte **spark thrift-sparkconf** a **počáteční** pomocí součásti a značky, který chcete načíst konfiguraci pro.</span><span class="sxs-lookup"><span data-stu-id="c4513-201">Replace **spark-thrift-sparkconf** and **INITIAL** with the component and tag that you want to retrieve the configuration for.</span></span>
   
    <span data-ttu-id="c4513-202">Jq slouží k zapnutí data načtená z HDInsight do nové šablony konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4513-202">Jq is used to turn the data retrieved from HDInsight into a new configuration template.</span></span> <span data-ttu-id="c4513-203">Tyto příklady konkrétně, proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="c4513-203">Specifically, these examples perform the following actions:</span></span>
   
    * <span data-ttu-id="c4513-204">Vytvoří jedinečnou hodnotu obsahující řetězec "verze" a data, která je uložena v `newtag`.</span><span class="sxs-lookup"><span data-stu-id="c4513-204">Creates a unique value containing the string "version" and the date, which is stored in `newtag`.</span></span>

    * <span data-ttu-id="c4513-205">Vytvoří dokument kořenové pro nové požadované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4513-205">Creates a root document for the new desired configuration.</span></span>

    * <span data-ttu-id="c4513-206">Získá obsah `.items[]` pole a přidá ho **desired_config** element.</span><span class="sxs-lookup"><span data-stu-id="c4513-206">Gets the contents of the `.items[]` array and adds it under the **desired_config** element.</span></span>

    * <span data-ttu-id="c4513-207">Odstraní `href`, `version`, a `Config` prvky, jako tyto prvky nejsou potřebné odeslat novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c4513-207">Deletes the `href`, `version`, and `Config` elements, as these elements aren't needed to submit a new configuration.</span></span>

    * <span data-ttu-id="c4513-208">Přidá `tag` element s hodnotou `version#################`.</span><span class="sxs-lookup"><span data-stu-id="c4513-208">Adds a `tag` element with a value of `version#################`.</span></span> <span data-ttu-id="c4513-209">Číselnou část je založena na aktuální datum.</span><span class="sxs-lookup"><span data-stu-id="c4513-209">The numeric portion is based on the current date.</span></span> <span data-ttu-id="c4513-210">Každá konfigurace musí mít jedinečný kód.</span><span class="sxs-lookup"><span data-stu-id="c4513-210">Each configuration must have a unique tag.</span></span>
     
    <span data-ttu-id="c4513-211">Nakonec k uložení dat `newconfig.json` dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c4513-211">Finally, the data is saved to the `newconfig.json` document.</span></span> <span data-ttu-id="c4513-212">Struktura dokumentu by měla vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c4513-212">The document structure should appear similar to the following example:</span></span>
     
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

3. <span data-ttu-id="c4513-213">Otevřete `newconfig.json` dokumentu a změnit nebo přidat hodnoty v `properties` objektu.</span><span class="sxs-lookup"><span data-stu-id="c4513-213">Open the `newconfig.json` document and modify/add values in the `properties` object.</span></span> <span data-ttu-id="c4513-214">Následující příklad změní hodnotu `"spark.yarn.am.memory"` z `"1g"` k `"3g"`.</span><span class="sxs-lookup"><span data-stu-id="c4513-214">The following example changes the value of `"spark.yarn.am.memory"` from `"1g"` to `"3g"`.</span></span> <span data-ttu-id="c4513-215">Přidává také `"spark.kryoserializer.buffer.max"` s hodnotou `"256m"`.</span><span class="sxs-lookup"><span data-stu-id="c4513-215">It also adds `"spark.kryoserializer.buffer.max"` with a value of `"256m"`.</span></span>
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    <span data-ttu-id="c4513-216">Jakmile dokončíte provedení změny, uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="c4513-216">Save the file once you are done making modifications.</span></span>

4. <span data-ttu-id="c4513-217">Použijte následující příkazy k odeslání do Ambari aktualizovanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c4513-217">Use the following commands to submit the updated configuration to Ambari.</span></span>
   
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
   
    <span data-ttu-id="c4513-218">Tyto příkazy odeslat obsah **newconfig.json** souboru do clusteru jako nový požadované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4513-218">These commands submit the contents of the **newconfig.json** file to the cluster as the new desired configuration.</span></span> <span data-ttu-id="c4513-219">Požadavek vrátí dokumentu JSON.</span><span class="sxs-lookup"><span data-stu-id="c4513-219">The request returns a JSON document.</span></span> <span data-ttu-id="c4513-220">**VersionTag** element v tomto dokumentu by měl shodovat s verzí, které jste odeslali, a **konfigurací** objekt obsahuje změny konfigurace, které jste požádali.</span><span class="sxs-lookup"><span data-stu-id="c4513-220">The **versionTag** element in this document should match the version you submitted, and the **configs** object contains the configuration changes you requested.</span></span>

### <a name="example-restart-a-service-component"></a><span data-ttu-id="c4513-221">Příklad: Restartovat součást služby</span><span class="sxs-lookup"><span data-stu-id="c4513-221">Example: Restart a service component</span></span>

<span data-ttu-id="c4513-222">Nyní když se podíváte na webovému uživatelskému rozhraní Ambari, službu Spark označuje, že je nutné restartovat předtím, než se projeví se nová konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4513-222">At this point, if you look at the Ambari web UI, the Spark service indicates that it needs to be restarted before the new configuration can take effect.</span></span> <span data-ttu-id="c4513-223">Restartujte službu pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="c4513-223">Use the following steps to restart the service.</span></span>

1. <span data-ttu-id="c4513-224">Pokud chcete povolit režim údržby pro službu Spark, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="c4513-224">Use the following to enable maintenance mode for the Spark service:</span></span>

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
   
    <span data-ttu-id="c4513-225">Tyto příkazy poslat dokument JSON na server, který zapne režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="c4513-225">These commands send a JSON document to the server that turns on maintenance mode.</span></span> <span data-ttu-id="c4513-226">Můžete ověřit, že služba je nyní v režimu údržby pomocí následující žádosti o:</span><span class="sxs-lookup"><span data-stu-id="c4513-226">You can verify that the service is now in maintenance mode using the following request:</span></span>
   
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
   
    <span data-ttu-id="c4513-227">Vrácená hodnota je `ON`.</span><span class="sxs-lookup"><span data-stu-id="c4513-227">The return value is `ON`.</span></span>

2. <span data-ttu-id="c4513-228">Chcete-li vypnout službu vedle, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="c4513-228">Next, use the following to turn off the service:</span></span>

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
    
    <span data-ttu-id="c4513-229">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c4513-229">The response is similar to the following example:</span></span>
   
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
    > <span data-ttu-id="c4513-230">`href` Hodnoty vrácené tento identifikátor URI používá interní IP adresu uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-230">The `href` value returned by this URI is using the internal IP address of the cluster node.</span></span> <span data-ttu-id="c4513-231">Pokud chcete použít z mimo cluster, nahraďte část '10.0.0.18:8080' plně kvalifikovaný název domény clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4513-231">To use it from outside the cluster, replace the \`10.0.0.18:8080' portion with the FQDN of the cluster.</span></span> 
    
    <span data-ttu-id="c4513-232">Následující příkazy načíst stav žádosti:</span><span class="sxs-lookup"><span data-stu-id="c4513-232">The following commands retrieve the status of the request:</span></span>

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

    <span data-ttu-id="c4513-233">Odpověď z `COMPLETED` označuje, že žádost byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="c4513-233">A response of `COMPLETED` indicates that the request has finished.</span></span>

3. <span data-ttu-id="c4513-234">Po dokončení předchozí požadavek, použijte následující spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="c4513-234">Once the previous request completes, use the following to start the service.</span></span>
   
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
    <span data-ttu-id="c4513-235">Služba teď používá nová konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4513-235">The service is now using the new configuration.</span></span>

4. <span data-ttu-id="c4513-236">Nakonec použijte následující vypnutí režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="c4513-236">Finally, use the following to turn off maintenance mode.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="c4513-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4513-237">Next steps</span></span>

<span data-ttu-id="c4513-238">Úplný přehled rozhraní REST API, najdete v části [Ambari API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="c4513-238">For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

