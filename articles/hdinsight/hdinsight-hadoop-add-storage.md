---
title: "Další úložiště Azure aaaAdd účty tooHDInsight | Microsoft Docs"
description: "Zjistěte, jak další úložiště Azure tooadd účty tooan stávajícího clusteru HDInsight."
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
ms.openlocfilehash: ce5acfa4b61bf7e83b1fb374d64a1eaa3182fbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-additional-storage-accounts-toohdinsight"></a><span data-ttu-id="1df6b-103">Přidejte další úložiště účtů tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="1df6b-103">Add additional storage accounts tooHDInsight</span></span>

<span data-ttu-id="1df6b-104">Zjistěte, jak toouse skript akce tooadd další úložiště Azure účty tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="1df6b-104">Learn how toouse script actions tooadd additional Azure storage accounts tooHDInsight.</span></span> <span data-ttu-id="1df6b-105">Hello kroky v tomto dokumentu přidejte účet tooan existující HDInsight se systémem Linux clusteru úložiště.</span><span class="sxs-lookup"><span data-stu-id="1df6b-105">hello steps in this document add a storage account tooan existing Linux-based HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1df6b-106">Hello informace v tomto dokumentu je o přidání dalšího úložiště tooa clusteru po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1df6b-106">hello information in this document is about adding additional storage tooa cluster after it has been created.</span></span> <span data-ttu-id="1df6b-107">Informace o přidání účty úložiště při vytváření clusteru najdete v tématu [nastavit clusterů v HDInsight Hadoop, Spark, Kafka a dalšími](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="1df6b-107">For information on adding storage accounts during cluster creation, see [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="how-it-works"></a><span data-ttu-id="1df6b-108">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="1df6b-108">How it works</span></span>

<span data-ttu-id="1df6b-109">Tento skript má hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="1df6b-109">This script takes hello following parameters:</span></span>

* <span data-ttu-id="1df6b-110">__Název účtu úložiště Azure__: název hello hello úložiště účet tooadd toohello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1df6b-110">__Azure storage account name__: hello name of hello storage account tooadd toohello HDInsight cluster.</span></span> <span data-ttu-id="1df6b-111">Po spuštění skriptu hello, HDInsight můžete číst a zapisovat data uložená v rámci tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1df6b-111">After running hello script, HDInsight can read and write data stored in this storage account.</span></span>

* <span data-ttu-id="1df6b-112">__Klíč účtu úložiště Azure__: klíč, který uděluje přístup k účtu úložiště toohello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-112">__Azure storage account key__: A key that grants access toohello storage account.</span></span>

* <span data-ttu-id="1df6b-113">__-p__ (volitelné):-li zadána, hello klíč není šifrován a je uložen v souboru core-site.xml hello jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="1df6b-113">__-p__ (optional): If specified, hello key is not encrypted and is stored in hello core-site.xml file as plain text.</span></span>

<span data-ttu-id="1df6b-114">Během zpracování hello skript provede hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="1df6b-114">During processing, hello script performs hello following actions:</span></span>

* <span data-ttu-id="1df6b-115">Pokud hello účet úložiště už existuje v konfiguraci core-site.xml hello hello clusteru, ukončí hello skriptu a byly provedeny žádné další akce.</span><span class="sxs-lookup"><span data-stu-id="1df6b-115">If hello storage account already exists in hello core-site.xml configuration for hello cluster, hello script exits and no further actions are performed.</span></span>

* <span data-ttu-id="1df6b-116">Ověřuje, že účet úložiště hello existuje a je přístupný pomocí klíče hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-116">Verifies that hello storage account exists and can be accessed using hello key.</span></span>

* <span data-ttu-id="1df6b-117">Zašifruje pomocí přihlašovacích údajů clusteru hello klíč hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-117">Encrypts hello key using hello cluster credential.</span></span>

* <span data-ttu-id="1df6b-118">Přidá soubor základní site.xml toohello účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-118">Adds hello storage account toohello core-site.xml file.</span></span>

* <span data-ttu-id="1df6b-119">Zastaví a restartuje hello Oozie, YARN, MapReduce2 a HDFS služby.</span><span class="sxs-lookup"><span data-stu-id="1df6b-119">Stops and restarts hello Oozie, YARN, MapReduce2, and HDFS services.</span></span> <span data-ttu-id="1df6b-120">Zastavení a spuštění těchto služeb jim umožňuje toouse hello nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1df6b-120">Stopping and starting these services allows them toouse hello new storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="1df6b-121">Použití účtu úložiště v jiném umístění než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="1df6b-121">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span>

## <a name="hello-script"></a><span data-ttu-id="1df6b-122">skript Hello</span><span class="sxs-lookup"><span data-stu-id="1df6b-122">hello script</span></span>

<span data-ttu-id="1df6b-123">__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="1df6b-123">__Script location__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span></span>

<span data-ttu-id="1df6b-124">__Požadavky na__:</span><span class="sxs-lookup"><span data-stu-id="1df6b-124">__Requirements__:</span></span>

* <span data-ttu-id="1df6b-125">skript Hello se musí použít na hello __hlavní uzly__.</span><span class="sxs-lookup"><span data-stu-id="1df6b-125">hello script must be applied on hello __Head nodes__.</span></span>

## <a name="toouse-hello-script"></a><span data-ttu-id="1df6b-126">toouse hello skriptu</span><span class="sxs-lookup"><span data-stu-id="1df6b-126">toouse hello script</span></span>

<span data-ttu-id="1df6b-127">Tento skript lze z hello portál Azure, Azure PowerShell nebo hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="1df6b-127">This script can be used from hello Azure portal, Azure PowerShell, or hello Azure CLI 1.0.</span></span> <span data-ttu-id="1df6b-128">Další informace najdete v tématu hello [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1df6b-128">For more information, see hello [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1df6b-129">Při použití hello kroků uvedených v dokumentu hello přizpůsobení, použijte následující informace tooapply hello tento skript:</span><span class="sxs-lookup"><span data-stu-id="1df6b-129">When using hello steps provided in hello customization document, use hello following information tooapply this script:</span></span>
>
> * <span data-ttu-id="1df6b-130">Nahradí všechny akce skriptu příklad URI hello identifikátor URI pro tento skript (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span><span class="sxs-lookup"><span data-stu-id="1df6b-130">Replace any example script action URI with hello URI for this script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span></span>
> * <span data-ttu-id="1df6b-131">Nahradí všechny parametry příklad hello název účtu úložiště Azure a klíč hello úložiště účet toobe přidané toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1df6b-131">Replace any example parameters with hello Azure storage account name and key of hello storage account toobe added toohello cluster.</span></span> <span data-ttu-id="1df6b-132">Pokud pomocí hello portálu Azure, musí být tyto parametry oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="1df6b-132">If using hello Azure portal, these parameters must be separated by a space.</span></span>
> * <span data-ttu-id="1df6b-133">Není nutné toomark tento skript jako __trvalé__, jak přímo aktualizuje hello Ambari konfiguraci pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="1df6b-133">You do not need toomark this script as __Persisted__, as it directly updates hello Ambari configuration for hello cluster.</span></span>

## <a name="known-issues"></a><span data-ttu-id="1df6b-134">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="1df6b-134">Known issues</span></span>

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a><span data-ttu-id="1df6b-135">Nezobrazuje se v portálu Azure nebo nástroje pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="1df6b-135">Storage accounts not displayed in Azure portal or tools</span></span>

<span data-ttu-id="1df6b-136">Při zobrazení hello HDInsight cluster v hello portálu Azure, výběr hello __účty úložiště__ položky v rámci __vlastnosti__ nezobrazí účty úložiště, které jsou přidány prostřednictvím této akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="1df6b-136">When viewing hello HDInsight cluster in hello Azure portal, selecting hello __Storage Accounts__ entry under __Properties__ does not display storage accounts added through this script action.</span></span> <span data-ttu-id="1df6b-137">Azure PowerShell a rozhraní příkazového řádku Azure nezobrazují účtu další úložiště hello buď.</span><span class="sxs-lookup"><span data-stu-id="1df6b-137">Azure PowerShell and Azure CLI do not display hello additional storage account either.</span></span>

<span data-ttu-id="1df6b-138">informace o Hello úložiště není zobrazit, protože skript hello pouze upraví konfiguraci core-site.xml hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1df6b-138">hello storage information isn't displayed because hello script only modifies hello core-site.xml configuration for hello cluster.</span></span> <span data-ttu-id="1df6b-139">Tyto informace se nepoužívá při načítání informací o hello clusteru pomocí Azure rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="1df6b-139">This information is not used when retrieving hello cluster information using Azure management APIs.</span></span>

<span data-ttu-id="1df6b-140">informace o účtu úložiště tooview přidat toohello clusteru pomocí tohoto skriptu, použijte hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="1df6b-140">tooview storage account information added toohello cluster using this script, use hello Ambari REST API.</span></span> <span data-ttu-id="1df6b-141">Použijte následující příkazy tooretrieve hello tyto informace pro váš cluster:</span><span class="sxs-lookup"><span data-stu-id="1df6b-141">Use hello following commands tooretrieve this information for your cluster:</span></span>

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter hello cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> <span data-ttu-id="1df6b-142">Nastavit `$clusterName` toohello název clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-142">Set `$clusterName` toohello name of hello HDInsight cluster.</span></span> <span data-ttu-id="1df6b-143">Nastavit `$storageAccountName` toohello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-143">Set `$storageAccountName` toohello name of hello storage account.</span></span> <span data-ttu-id="1df6b-144">Po zobrazení výzvy zadejte přihlašovací jméno clusteru hello (správce) a heslo.</span><span class="sxs-lookup"><span data-stu-id="1df6b-144">When prompted, enter hello cluster login (admin) and password.</span></span>

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> <span data-ttu-id="1df6b-145">Nastavit `$PASSWORD` heslo účtu přihlášení (správce) toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1df6b-145">Set `$PASSWORD` toohello cluster login (admin) account password.</span></span> <span data-ttu-id="1df6b-146">Nastavit `$CLUSTERNAME` toohello název clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-146">Set `$CLUSTERNAME` toohello name of hello HDInsight cluster.</span></span> <span data-ttu-id="1df6b-147">Nastavit `$STORAGEACCOUNTNAME` toohello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-147">Set `$STORAGEACCOUNTNAME` toohello name of hello storage account.</span></span>
>
> <span data-ttu-id="1df6b-148">Tento příklad používá [curl (http://curl.haxx.se/)](http://curl.haxx.se/) a [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve a analýzu dat JSON.</span><span class="sxs-lookup"><span data-stu-id="1df6b-148">This example uses [curl (http://curl.haxx.se/)](http://curl.haxx.se/) and [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve and parse JSON data.</span></span>

<span data-ttu-id="1df6b-149">Při použití tohoto příkazu, nahraďte __CLUSTERNAME__ s názvem hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1df6b-149">When using this command, replace __CLUSTERNAME__ with hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="1df6b-150">Nahraďte __heslo__ heslem hello HTTP přihlášení pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="1df6b-150">Replace __PASSWORD__ with hello HTTP login password for hello cluster.</span></span> <span data-ttu-id="1df6b-151">Nahraďte __STORAGEACCOUNT__ hello název účtu úložiště hello přidána pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="1df6b-151">Replace __STORAGEACCOUNT__ with hello name of hello storage account added using script action.</span></span> <span data-ttu-id="1df6b-152">Informace o vrácená z tohoto příkazu se zobrazí podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="1df6b-152">Information returned from this command appears similar toohello following text:</span></span>

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

<span data-ttu-id="1df6b-153">Tento text je příkladem šifrovaný klíč, který se používá tooaccess hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1df6b-153">This text is an example of an encrypted key, which is used tooaccess hello storage account.</span></span>

### <a name="unable-tooaccess-storage-after-changing-key"></a><span data-ttu-id="1df6b-154">Úložiště nelze tooaccess po změně klíč</span><span class="sxs-lookup"><span data-stu-id="1df6b-154">Unable tooaccess storage after changing key</span></span>

<span data-ttu-id="1df6b-155">Pokud změníte hello klíč pro účet úložiště, HDInsight mít nadále přístup k účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-155">If you change hello key for a storage account, HDInsight can no longer access hello storage account.</span></span> <span data-ttu-id="1df6b-156">HDInsight používá kopii klíče uložené v mezipaměti v hello core-site.xml pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="1df6b-156">HDInsight uses a cached copy of key in hello core-site.xml for hello cluster.</span></span> <span data-ttu-id="1df6b-157">Tato kopie v mezipaměti musí být aktualizované toomatch hello nový klíč.</span><span class="sxs-lookup"><span data-stu-id="1df6b-157">This cached copy must be updated toomatch hello new key.</span></span>

<span data-ttu-id="1df6b-158">Spuštění skriptu akci hello nemá __není__ aktualizovat hello klíč, protože hello skript kontroluje toosee, pokud již existuje položka pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-158">Running hello script action again does __not__ update hello key, as hello script checks toosee if an entry for hello storage account already exists.</span></span> <span data-ttu-id="1df6b-159">Pokud položka již existuje, nebyly provedeny žádné změny.</span><span class="sxs-lookup"><span data-stu-id="1df6b-159">If an entry already exists, it does not make any changes.</span></span>

<span data-ttu-id="1df6b-160">toowork tento problém vyřešit, je nutné odebrat hello existující položku pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1df6b-160">toowork around this problem, you must remove hello existing entry for hello storage account.</span></span> <span data-ttu-id="1df6b-161">Použijte následující postup tooremove hello existující položku hello:</span><span class="sxs-lookup"><span data-stu-id="1df6b-161">Use hello following steps tooremove hello existing entry:</span></span>

1. <span data-ttu-id="1df6b-162">Ve webovém prohlížeči otevřete hello webové uživatelské rozhraní Ambari pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1df6b-162">In a web browser, open hello Ambari Web UI for your HDInsight cluster.</span></span> <span data-ttu-id="1df6b-163">Hello identifikátor URI je https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1df6b-163">hello URI is https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="1df6b-164">Nahraďte __CLUSTERNAME__ s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="1df6b-164">Replace __CLUSTERNAME__ with hello name of your cluster.</span></span>

    <span data-ttu-id="1df6b-165">Po zobrazení výzvy zadejte hello HTTP přihlášení uživatele a heslo pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="1df6b-165">When prompted, enter hello HTTP login user and password for your cluster.</span></span>

2. <span data-ttu-id="1df6b-166">Hello seznamu služeb na levé straně hello hello stránky, vyberte __HDFS__.</span><span class="sxs-lookup"><span data-stu-id="1df6b-166">From hello list of services on hello left of hello page, select __HDFS__.</span></span> <span data-ttu-id="1df6b-167">Potom vyberte hello __konfigurací__ ve středu hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="1df6b-167">Then select hello __Configs__ tab in hello center of hello page.</span></span>

3. <span data-ttu-id="1df6b-168">V hello __filtru...__  pole, zadejte hodnotu __fs.azure.account__.</span><span class="sxs-lookup"><span data-stu-id="1df6b-168">In hello __Filter...__ field, enter a value of __fs.azure.account__.</span></span> <span data-ttu-id="1df6b-169">Tento příkaz vrátí položky pro všechny účty dalšího úložiště, které byly přidány toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1df6b-169">This returns entries for any additional storage accounts that have been added toohello cluster.</span></span> <span data-ttu-id="1df6b-170">Existují dva typy položek; __keyprovider__ a __klíč__.</span><span class="sxs-lookup"><span data-stu-id="1df6b-170">There are two types of entries; __keyprovider__ and __key__.</span></span> <span data-ttu-id="1df6b-171">Oba obsahují hello název účtu úložiště hello jako součást hello název klíče.</span><span class="sxs-lookup"><span data-stu-id="1df6b-171">Both contain hello name of hello storage account as part of hello key name.</span></span>

    <span data-ttu-id="1df6b-172">Hello se následující příklad položky pro účet úložiště s názvem __mystorage__:</span><span class="sxs-lookup"><span data-stu-id="1df6b-172">hello following are example entries for a storage account named __mystorage__:</span></span>

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. <span data-ttu-id="1df6b-173">Po zjištění hello klíče pro účet úložiště hello potřebujete tooremove, použijte hello red '-' toohello ikony napravo od hello položka toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="1df6b-173">After you have identified hello keys for hello storage account you need tooremove, use hello red '-' icon toohello right of hello entry toodelete it.</span></span> <span data-ttu-id="1df6b-174">Potom pomocí hello __Uložit__ tlačítko toosave změny.</span><span class="sxs-lookup"><span data-stu-id="1df6b-174">Then use hello __Save__ button toosave your changes.</span></span>

5. <span data-ttu-id="1df6b-175">Po uložení změn, použijte hello skript akce tooadd hello účet úložiště a nový cluster toohello hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="1df6b-175">After changes have been saved, use hello script action tooadd hello storage account and new key value toohello cluster.</span></span>

### <a name="poor-performance"></a><span data-ttu-id="1df6b-176">Snížený výkon</span><span class="sxs-lookup"><span data-stu-id="1df6b-176">Poor performance</span></span>

<span data-ttu-id="1df6b-177">Pokud účet úložiště hello v jiné oblasti než hello clusteru HDInsight se můžete setkat snížený výkon.</span><span class="sxs-lookup"><span data-stu-id="1df6b-177">If hello storage account is in a different region than hello HDInsight cluster, you may experience poor performance.</span></span> <span data-ttu-id="1df6b-178">Přístupu k datům v provozu sítě jiné oblasti zasílá, mimo hello místní datové centrum Azure a napříč hello veřejného Internetu, které můžou představovat latence.</span><span class="sxs-lookup"><span data-stu-id="1df6b-178">Accessing data in a different region sends network traffic outside hello regional Azure data center and across hello public internet, which can introduce latency.</span></span>

> [!WARNING]
> <span data-ttu-id="1df6b-179">Použití účtu úložiště v jiné oblasti než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="1df6b-179">Using a storage account in a different region than hello HDInsight cluster is not supported.</span></span>

### <a name="additional-charges"></a><span data-ttu-id="1df6b-180">Další poplatky.</span><span class="sxs-lookup"><span data-stu-id="1df6b-180">Additional charges</span></span>

<span data-ttu-id="1df6b-181">Pokud účet úložiště hello je v jiné oblasti než hello clusteru HDInsight, můžete si všimnout další nimi spojeným nákladům na vaši fakturaci Azure.</span><span class="sxs-lookup"><span data-stu-id="1df6b-181">If hello storage account is in a different region than hello HDInsight cluster, you may notice additional egress charges on your Azure billing.</span></span> <span data-ttu-id="1df6b-182">Poplatek za odchozí data se použije v případě, že data ponechá místního datového centra.</span><span class="sxs-lookup"><span data-stu-id="1df6b-182">An egress charge is applied when data leaves a regional data center.</span></span> <span data-ttu-id="1df6b-183">Tento poplatek za platí i v případě, že provoz hello je určené pro jiné datové centrum Azure v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="1df6b-183">This charge is applied even if hello traffic is destined for another Azure data center in a different region.</span></span>

> [!WARNING]
> <span data-ttu-id="1df6b-184">Použití účtu úložiště v jiné oblasti než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="1df6b-184">Using a storage account in a different region than hello HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1df6b-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1df6b-185">Next steps</span></span>

<span data-ttu-id="1df6b-186">Jste se naučili, jak účtů úložiště další tooadd tooan stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1df6b-186">You have learned how tooadd additional storage accounts tooan existing HDInsight cluster.</span></span> <span data-ttu-id="1df6b-187">Další informace o akcí skriptů naleznete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="1df6b-187">For more information on script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
