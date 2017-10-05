---
title: "Přidejte další úložiště Azure účty do HDInsight | Microsoft Docs"
description: "Naučte se přidávat další úložiště Azure účty do existujícího clusteru HDInsight."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 0853e8605e07c28867676e9c13b89263ade67c88
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a><span data-ttu-id="d8b96-103">Přidat další účty úložiště do HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8b96-103">Add additional storage accounts to HDInsight</span></span>

<span data-ttu-id="d8b96-104">Další informace o použití akce skriptu do HDInsight Pokud chcete přidat další úložiště Azure účty.</span><span class="sxs-lookup"><span data-stu-id="d8b96-104">Learn how to use script actions to add additional Azure storage accounts to HDInsight.</span></span> <span data-ttu-id="d8b96-105">Kroky v tomto dokumentu přidání účtu úložiště do existujícího clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="d8b96-105">The steps in this document add a storage account to an existing Linux-based HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8b96-106">Informace v tomto dokumentu je o přidání dalšího úložiště do clusteru po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="d8b96-106">The information in this document is about adding additional storage to a cluster after it has been created.</span></span> <span data-ttu-id="d8b96-107">Informace o přidání účty úložiště při vytváření clusteru najdete v tématu [nastavit clusterů v HDInsight Hadoop, Spark, Kafka a dalšími](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="d8b96-107">For information on adding storage accounts during cluster creation, see [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="how-it-works"></a><span data-ttu-id="d8b96-108">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="d8b96-108">How it works</span></span>

<span data-ttu-id="d8b96-109">Tento skript používá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d8b96-109">This script takes the following parameters:</span></span>

* <span data-ttu-id="d8b96-110">__Název účtu úložiště Azure__: název účtu úložiště pro přidání do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8b96-110">__Azure storage account name__: The name of the storage account to add to the HDInsight cluster.</span></span> <span data-ttu-id="d8b96-111">Po spuštění skriptu, HDInsight můžete číst a zapisovat data uložená v rámci tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-111">After running the script, HDInsight can read and write data stored in this storage account.</span></span>

* <span data-ttu-id="d8b96-112">__Klíč účtu úložiště Azure__: klíč, který uděluje přístup k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-112">__Azure storage account key__: A key that grants access to the storage account.</span></span>

* <span data-ttu-id="d8b96-113">__-p__ (volitelné):-li zadána, klíč není šifrován a je uložen v souboru core-site.xml jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="d8b96-113">__-p__ (optional): If specified, the key is not encrypted and is stored in the core-site.xml file as plain text.</span></span>

<span data-ttu-id="d8b96-114">Během zpracování na skript provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="d8b96-114">During processing, the script performs the following actions:</span></span>

* <span data-ttu-id="d8b96-115">Pokud účet úložiště už existuje v konfiguraci core-site.xml pro cluster, skript bude ukončen a budou provedeny žádné další akce.</span><span class="sxs-lookup"><span data-stu-id="d8b96-115">If the storage account already exists in the core-site.xml configuration for the cluster, the script exits and no further actions are performed.</span></span>

* <span data-ttu-id="d8b96-116">Ověří, že existuje účet úložiště a je přístupný pomocí klíče.</span><span class="sxs-lookup"><span data-stu-id="d8b96-116">Verifies that the storage account exists and can be accessed using the key.</span></span>

* <span data-ttu-id="d8b96-117">Klíč zašifruje pomocí přihlašovacích údajů clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8b96-117">Encrypts the key using the cluster credential.</span></span>

* <span data-ttu-id="d8b96-118">Účet úložiště se přidá do souboru core-site.xml.</span><span class="sxs-lookup"><span data-stu-id="d8b96-118">Adds the storage account to the core-site.xml file.</span></span>

* <span data-ttu-id="d8b96-119">Zastaví a restartuje službu Oozie, YARN, MapReduce2 a HDFS.</span><span class="sxs-lookup"><span data-stu-id="d8b96-119">Stops and restarts the Oozie, YARN, MapReduce2, and HDFS services.</span></span> <span data-ttu-id="d8b96-120">Zastavení a spuštění těchto služeb umožňuje, aby uživatelé používali nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-120">Stopping and starting these services allows them to use the new storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="d8b96-121">Použití účtu úložiště v jiném umístění než HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="d8b96-121">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span>

## <a name="the-script"></a><span data-ttu-id="d8b96-122">Skript</span><span class="sxs-lookup"><span data-stu-id="d8b96-122">The script</span></span>

<span data-ttu-id="d8b96-123">__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="d8b96-123">__Script location__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span></span>

<span data-ttu-id="d8b96-124">__Požadavky na__:</span><span class="sxs-lookup"><span data-stu-id="d8b96-124">__Requirements__:</span></span>

* <span data-ttu-id="d8b96-125">Skript se musí použít na __hlavní uzly__.</span><span class="sxs-lookup"><span data-stu-id="d8b96-125">The script must be applied on the __Head nodes__.</span></span>

## <a name="to-use-the-script"></a><span data-ttu-id="d8b96-126">Pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="d8b96-126">To use the script</span></span>

<span data-ttu-id="d8b96-127">Tento skript lze z portálu Azure, Azure PowerShell nebo Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="d8b96-127">This script can be used from the Azure portal, Azure PowerShell, or the Azure CLI 1.0.</span></span> <span data-ttu-id="d8b96-128">Další informace najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d8b96-128">For more information, see the [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8b96-129">Pokud používáte podle kroků uvedených v dokumentu přizpůsobení, použijte tyto informace použít tento skript:</span><span class="sxs-lookup"><span data-stu-id="d8b96-129">When using the steps provided in the customization document, use the following information to apply this script:</span></span>
>
> * <span data-ttu-id="d8b96-130">Nahradí všechny akce skriptu příklad URI identifikátor URI pro tento skript (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span><span class="sxs-lookup"><span data-stu-id="d8b96-130">Replace any example script action URI with the URI for this script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span></span>
> * <span data-ttu-id="d8b96-131">Žádné parametry příkladu nahraďte název účtu úložiště Azure a klíč účtu úložiště, který se má přidat do clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8b96-131">Replace any example parameters with the Azure storage account name and key of the storage account to be added to the cluster.</span></span> <span data-ttu-id="d8b96-132">Pokud používáte portál Azure, musí být tyto parametry oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="d8b96-132">If using the Azure portal, these parameters must be separated by a space.</span></span>
> * <span data-ttu-id="d8b96-133">Není potřeba označit tento skript jako __trvalé__, jak přímo aktualizuje Ambari konfiguraci pro daný cluster.</span><span class="sxs-lookup"><span data-stu-id="d8b96-133">You do not need to mark this script as __Persisted__, as it directly updates the Ambari configuration for the cluster.</span></span>

## <a name="known-issues"></a><span data-ttu-id="d8b96-134">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="d8b96-134">Known issues</span></span>

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a><span data-ttu-id="d8b96-135">Nezobrazuje se v portálu Azure nebo nástroje pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="d8b96-135">Storage accounts not displayed in Azure portal or tools</span></span>

<span data-ttu-id="d8b96-136">Při zobrazení clusteru HDInsight na portálu Azure, vyberete __účty úložiště__ položky v rámci __vlastnosti__ nezobrazí účty úložiště, které jsou přidány prostřednictvím této akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="d8b96-136">When viewing the HDInsight cluster in the Azure portal, selecting the __Storage Accounts__ entry under __Properties__ does not display storage accounts added through this script action.</span></span> <span data-ttu-id="d8b96-137">Azure PowerShell a rozhraní příkazového řádku Azure nezobrazují účtu další úložiště buď.</span><span class="sxs-lookup"><span data-stu-id="d8b96-137">Azure PowerShell and Azure CLI do not display the additional storage account either.</span></span>

<span data-ttu-id="d8b96-138">Informace o úložiště není zobrazit, protože skript pouze upraví konfiguraci core-site.xml pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d8b96-138">The storage information isn't displayed because the script only modifies the core-site.xml configuration for the cluster.</span></span> <span data-ttu-id="d8b96-139">Tyto informace se nepoužívá při načítání informace o clusteru pomocí Azure rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="d8b96-139">This information is not used when retrieving the cluster information using Azure management APIs.</span></span>

<span data-ttu-id="d8b96-140">Chcete-li zobrazit informace o účtu úložiště přidat do clusteru pomocí tohoto skriptu, použijte Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="d8b96-140">To view storage account information added to the cluster using this script, use the Ambari REST API.</span></span> <span data-ttu-id="d8b96-141">K načtení těchto informací pro váš cluster, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d8b96-141">Use the following commands to retrieve this information for your cluster:</span></span>

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> <span data-ttu-id="d8b96-142">Nastavit `$clusterName` na název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8b96-142">Set `$clusterName` to the name of the HDInsight cluster.</span></span> <span data-ttu-id="d8b96-143">Nastavit `$storageAccountName` k názvu účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-143">Set `$storageAccountName` to the name of the storage account.</span></span> <span data-ttu-id="d8b96-144">Po zobrazení výzvy zadejte přihlašovací clusteru (správce) a heslo.</span><span class="sxs-lookup"><span data-stu-id="d8b96-144">When prompted, enter the cluster login (admin) and password.</span></span>

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> <span data-ttu-id="d8b96-145">Nastavit `$PASSWORD` heslo k účtu přihlášení (správce) clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8b96-145">Set `$PASSWORD` to the cluster login (admin) account password.</span></span> <span data-ttu-id="d8b96-146">Nastavit `$CLUSTERNAME` na název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8b96-146">Set `$CLUSTERNAME` to the name of the HDInsight cluster.</span></span> <span data-ttu-id="d8b96-147">Nastavit `$STORAGEACCOUNTNAME` k názvu účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-147">Set `$STORAGEACCOUNTNAME` to the name of the storage account.</span></span>
>
> <span data-ttu-id="d8b96-148">Tento příklad používá [curl (http://curl.haxx.se/)](http://curl.haxx.se/) a [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) načíst a analyzovat JSON data.</span><span class="sxs-lookup"><span data-stu-id="d8b96-148">This example uses [curl (http://curl.haxx.se/)](http://curl.haxx.se/) and [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) to retrieve and parse JSON data.</span></span>

<span data-ttu-id="d8b96-149">Při použití tohoto příkazu, nahraďte __CLUSTERNAME__ s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8b96-149">When using this command, replace __CLUSTERNAME__ with the name of the HDInsight cluster.</span></span> <span data-ttu-id="d8b96-150">Nahraďte __heslo__ s heslo pro přihlášení HTTP pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d8b96-150">Replace __PASSWORD__ with the HTTP login password for the cluster.</span></span> <span data-ttu-id="d8b96-151">Nahraďte __STORAGEACCOUNT__ s názvem účtu úložiště přidat pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="d8b96-151">Replace __STORAGEACCOUNT__ with the name of the storage account added using script action.</span></span> <span data-ttu-id="d8b96-152">Informace o vrácená z tohoto příkazu se zobrazí podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d8b96-152">Information returned from this command appears similar to the following text:</span></span>

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

<span data-ttu-id="d8b96-153">Tento text je příkladem šifrovaný klíč, který se používá pro přístup k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-153">This text is an example of an encrypted key, which is used to access the storage account.</span></span>

### <a name="unable-to-access-storage-after-changing-key"></a><span data-ttu-id="d8b96-154">Nelze získat přístup k úložišti po změně klíč</span><span class="sxs-lookup"><span data-stu-id="d8b96-154">Unable to access storage after changing key</span></span>

<span data-ttu-id="d8b96-155">Pokud změníte klíč pro účet úložiště, HDInsight mít nadále přístup k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-155">If you change the key for a storage account, HDInsight can no longer access the storage account.</span></span> <span data-ttu-id="d8b96-156">HDInsight používá kopii klíče uložené v mezipaměti v core-site.xml pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d8b96-156">HDInsight uses a cached copy of key in the core-site.xml for the cluster.</span></span> <span data-ttu-id="d8b96-157">Tato kopie v mezipaměti musí být aktualizovány tak, aby odpovídaly nový klíč.</span><span class="sxs-lookup"><span data-stu-id="d8b96-157">This cached copy must be updated to match the new key.</span></span>

<span data-ttu-id="d8b96-158">Spuštění skriptu akci nemá __není__ aktualizovat klíč, protože skript zkontroluje, zda položka pro účet úložiště již existuje.</span><span class="sxs-lookup"><span data-stu-id="d8b96-158">Running the script action again does __not__ update the key, as the script checks to see if an entry for the storage account already exists.</span></span> <span data-ttu-id="d8b96-159">Pokud položka již existuje, nebyly provedeny žádné změny.</span><span class="sxs-lookup"><span data-stu-id="d8b96-159">If an entry already exists, it does not make any changes.</span></span>

<span data-ttu-id="d8b96-160">Chcete-li tento problém obejít, je třeba odebrat existující položku pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d8b96-160">To work around this problem, you must remove the existing entry for the storage account.</span></span> <span data-ttu-id="d8b96-161">Chcete-li odebrat existující položku pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d8b96-161">Use the following steps to remove the existing entry:</span></span>

1. <span data-ttu-id="d8b96-162">Ve webovém prohlížeči otevřete uživatelské rozhraní Ambari Web pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8b96-162">In a web browser, open the Ambari Web UI for your HDInsight cluster.</span></span> <span data-ttu-id="d8b96-163">Identifikátor URI je https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="d8b96-163">The URI is https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="d8b96-164">Nahraďte __CLUSTERNAME__ názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8b96-164">Replace __CLUSTERNAME__ with the name of your cluster.</span></span>

    <span data-ttu-id="d8b96-165">Po zobrazení výzvy zadejte HTTP přihlášení uživatele a heslo pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d8b96-165">When prompted, enter the HTTP login user and password for your cluster.</span></span>

2. <span data-ttu-id="d8b96-166">Ze seznamu služeb na levé straně stránky, vyberte __HDFS__.</span><span class="sxs-lookup"><span data-stu-id="d8b96-166">From the list of services on the left of the page, select __HDFS__.</span></span> <span data-ttu-id="d8b96-167">Vyberte __konfigurací__ ve středu stránky.</span><span class="sxs-lookup"><span data-stu-id="d8b96-167">Then select the __Configs__ tab in the center of the page.</span></span>

3. <span data-ttu-id="d8b96-168">V __filtru...__  pole, zadejte hodnotu __fs.azure.account__.</span><span class="sxs-lookup"><span data-stu-id="d8b96-168">In the __Filter...__ field, enter a value of __fs.azure.account__.</span></span> <span data-ttu-id="d8b96-169">Tento příkaz vrátí položky pro všechny účty dalšího úložiště, které jsou přidané do clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8b96-169">This returns entries for any additional storage accounts that have been added to the cluster.</span></span> <span data-ttu-id="d8b96-170">Existují dva typy položek; __keyprovider__ a __klíč__.</span><span class="sxs-lookup"><span data-stu-id="d8b96-170">There are two types of entries; __keyprovider__ and __key__.</span></span> <span data-ttu-id="d8b96-171">Oba obsahují název účtu úložiště jako součást název klíče.</span><span class="sxs-lookup"><span data-stu-id="d8b96-171">Both contain the name of the storage account as part of the key name.</span></span>

    <span data-ttu-id="d8b96-172">Níže jsou příklady položek pro účet úložiště s názvem __mystorage__:</span><span class="sxs-lookup"><span data-stu-id="d8b96-172">The following are example entries for a storage account named __mystorage__:</span></span>

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. <span data-ttu-id="d8b96-173">Po zjištění klíče pro účet úložiště, je třeba odebrat, použijte červený '-' ikony napravo položky ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="d8b96-173">After you have identified the keys for the storage account you need to remove, use the red '-' icon to the right of the entry to delete it.</span></span> <span data-ttu-id="d8b96-174">Potom pomocí __Uložit__ tlačítko uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="d8b96-174">Then use the __Save__ button to save your changes.</span></span>

5. <span data-ttu-id="d8b96-175">Po uložení změn, přidat účet úložiště a nová hodnota klíče do clusteru pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="d8b96-175">After changes have been saved, use the script action to add the storage account and new key value to the cluster.</span></span>

### <a name="poor-performance"></a><span data-ttu-id="d8b96-176">Snížený výkon</span><span class="sxs-lookup"><span data-stu-id="d8b96-176">Poor performance</span></span>

<span data-ttu-id="d8b96-177">Pokud účet úložiště v jiné oblasti než clusteru HDInsight se můžete setkat snížený výkon.</span><span class="sxs-lookup"><span data-stu-id="d8b96-177">If the storage account is in a different region than the HDInsight cluster, you may experience poor performance.</span></span> <span data-ttu-id="d8b96-178">Přístup k datům v jiné oblasti odešle síťový provoz, mimo místní datového centra Azure a napříč veřejného Internetu, které můžou představovat latence.</span><span class="sxs-lookup"><span data-stu-id="d8b96-178">Accessing data in a different region sends network traffic outside the regional Azure data center and across the public internet, which can introduce latency.</span></span>

> [!WARNING]
> <span data-ttu-id="d8b96-179">Použití účtu úložiště v jiné oblasti než HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="d8b96-179">Using a storage account in a different region than the HDInsight cluster is not supported.</span></span>

### <a name="additional-charges"></a><span data-ttu-id="d8b96-180">Další poplatky.</span><span class="sxs-lookup"><span data-stu-id="d8b96-180">Additional charges</span></span>

<span data-ttu-id="d8b96-181">Pokud účet úložiště je v jiné oblasti než clusteru HDInsight, můžete si povšimnout další nimi spojeným nákladům na vaši fakturaci Azure.</span><span class="sxs-lookup"><span data-stu-id="d8b96-181">If the storage account is in a different region than the HDInsight cluster, you may notice additional egress charges on your Azure billing.</span></span> <span data-ttu-id="d8b96-182">Poplatek za odchozí data se použije v případě, že data ponechá místního datového centra.</span><span class="sxs-lookup"><span data-stu-id="d8b96-182">An egress charge is applied when data leaves a regional data center.</span></span> <span data-ttu-id="d8b96-183">Tento poplatek za platí i v případě, že se provoz určený pro jiné datové centrum Azure v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="d8b96-183">This charge is applied even if the traffic is destined for another Azure data center in a different region.</span></span>

> [!WARNING]
> <span data-ttu-id="d8b96-184">Použití účtu úložiště v jiné oblasti než HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="d8b96-184">Using a storage account in a different region than the HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8b96-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8b96-185">Next steps</span></span>

<span data-ttu-id="d8b96-186">Jste se naučili jak přidat další účty úložiště do existujícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8b96-186">You have learned how to add additional storage accounts to an existing HDInsight cluster.</span></span> <span data-ttu-id="d8b96-187">Další informace o akcí skriptů naleznete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="d8b96-187">For more information on script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
