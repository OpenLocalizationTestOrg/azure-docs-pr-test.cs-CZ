---
title: "aaaUpload dat pro úlohy Hadoop v HDInsight | Microsoft Docs"
description: "Zjistěte, jak hello tooupload a přístup dat pro úlohy Hadoop v HDInsight pomocí rozhraní příkazového řádku Azure, Azure Storage Explorer, Azure PowerShell, hello Hadoop příkazového řádku nebo Sqoop."
keywords: "ETL hadoop, získávání dat do hadoop, hadoop načítání dat"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: jgao
ms.openlocfilehash: 15da602085d41c19789e34800f3d9e238d7d1de8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="4be2b-104">Nahrání dat úloh Hadoopu do služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="4be2b-104">Upload data for Hadoop jobs in HDInsight</span></span>
<span data-ttu-id="4be2b-105">Azure HDInsight nabízí plnohodnotné distribuované systému souborů Hadoop (HDFS) v porovnání s Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4be2b-105">Azure HDInsight provides a full-featured Hadoop distributed file system (HDFS) over Azure Blob storage.</span></span> <span data-ttu-id="4be2b-106">Je určený jako rozšíření tooprovide HDFS bezproblémové prostředí toocustomers.</span><span class="sxs-lookup"><span data-stu-id="4be2b-106">It is designed as an HDFS extension tooprovide a seamless experience toocustomers.</span></span> <span data-ttu-id="4be2b-107">Umožňuje hello úplnou sadu součástí toooperate ekosystém Hadoop hello přímo na hello dat, které spravuje.</span><span class="sxs-lookup"><span data-stu-id="4be2b-107">It enables hello full set of components in hello Hadoop ecosystem toooperate directly on hello data it manages.</span></span> <span data-ttu-id="4be2b-108">Azure Blob storage a HDFS jsou systémy různých souborů, které jsou optimalizované pro úložiště dat a výpočty na tato data.</span><span class="sxs-lookup"><span data-stu-id="4be2b-108">Azure Blob storage and HDFS are distinct file systems that are optimized for storage of data and computations on that data.</span></span> <span data-ttu-id="4be2b-109">Informace o hello výhody používání úložiště objektů Blob v Azure najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="4be2b-109">For information about hello benefits of using Azure Blob storage, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="4be2b-110">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="4be2b-110">**Prerequisites**</span></span>

<span data-ttu-id="4be2b-111">Vezměte na vědomí následující požadavky, než začnete hello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-111">Note hello following requirement before you begin:</span></span>

* <span data-ttu-id="4be2b-112">Cluster Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4be2b-112">An Azure HDInsight cluster.</span></span> <span data-ttu-id="4be2b-113">Pokyny najdete v tématu [Začínáme s Azure HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="4be2b-113">For instructions, see [Get started with Azure HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span>

## <a name="why-blob-storage"></a><span data-ttu-id="4be2b-114">Proč úložiště objektů blob?</span><span class="sxs-lookup"><span data-stu-id="4be2b-114">Why blob storage?</span></span>
<span data-ttu-id="4be2b-115">Azure HDInsight clustery jsou obvykle nasadit úloh MapReduce toorun a hello clustery jsou vyřazen po dokončení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="4be2b-115">Azure HDInsight clusters are typically deployed toorun MapReduce jobs, and hello clusters are dropped after these jobs complete.</span></span> <span data-ttu-id="4be2b-116">Zachovat hello data v clusterech HDFS hello po dokončení se výpočty by toostore nákladné způsob, jak tato data.</span><span class="sxs-lookup"><span data-stu-id="4be2b-116">Keeping hello data in hello HDFS clusters after computations are complete would be an expensive way toostore this data.</span></span> <span data-ttu-id="4be2b-117">Azure Blob storage je vysoce dostupný, vysoce škálovatelné a vysoce kapacitu, možnosti nízkonákladového a ke sdílení úložiště pro data, která je toobe zpracované pomocí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4be2b-117">Azure Blob storage is a highly available, highly scalable, high capacity, low cost, and shareable storage option for data that is toobe processed using HDInsight.</span></span> <span data-ttu-id="4be2b-118">Ukládání dat do objektu BLOB umožní hello clusterů HDInsight, které se používají pro výpočet toobe bezpečně vydaná bez ztráty dat.</span><span class="sxs-lookup"><span data-stu-id="4be2b-118">Storing data in a blob enables hello HDInsight clusters that are used for computation toobe safely released without losing data.</span></span>

### <a name="directories"></a><span data-ttu-id="4be2b-119">Adresáře</span><span class="sxs-lookup"><span data-stu-id="4be2b-119">Directories</span></span>
<span data-ttu-id="4be2b-120">Kontejnery Azure Blob storage ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů.</span><span class="sxs-lookup"><span data-stu-id="4be2b-120">Azure Blob storage containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="4be2b-121">Ale hello "/" znak lze použít v rámci toomake hello název klíče se zobrazí, jako kdyby je soubor uložit do do struktury adresářů.</span><span class="sxs-lookup"><span data-stu-id="4be2b-121">However hello "/" character can be used within hello key name toomake it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="4be2b-122">HDInsight uvidí tyto jako v případě, že jsou skutečné adresáře.</span><span class="sxs-lookup"><span data-stu-id="4be2b-122">HDInsight sees these as if they are actual directories.</span></span>

<span data-ttu-id="4be2b-123">Klíč k objektu blob může být například *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="4be2b-123">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="4be2b-124">Žádný skutečný "vstupní" adresář existuje, ale z důvodu přítomnosti toohello hello "/" znak v názvu klíče hello má hello vzhled cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="4be2b-124">No actual "input" directory exists, but due toohello presence of hello "/" character in hello key name, it has hello appearance of a file path.</span></span>

<span data-ttu-id="4be2b-125">Z toho důvodu Pokud pomocí nástroje Průzkumník Azure může dojít k některé soubory 0 bajtů.</span><span class="sxs-lookup"><span data-stu-id="4be2b-125">Because of this, if you use Azure Explorer tools you may notice some 0 byte files.</span></span> <span data-ttu-id="4be2b-126">Tyto soubory mají dva účely:</span><span class="sxs-lookup"><span data-stu-id="4be2b-126">These files serve two purposes:</span></span>

* <span data-ttu-id="4be2b-127">Pokud jsou prázdné složky, označte hello existence hello složky.</span><span class="sxs-lookup"><span data-stu-id="4be2b-127">If there are empty folders, they mark of hello existence of hello folder.</span></span> <span data-ttu-id="4be2b-128">Azure Blob storage je dostatečně inteligentní tooknow, že pokud objekt blob názvem foo/panelu existuje, je k dispozici složku s názvem **foo**.</span><span class="sxs-lookup"><span data-stu-id="4be2b-128">Azure Blob storage is clever enough tooknow that if a blob called foo/bar exists, there is a folder called **foo**.</span></span> <span data-ttu-id="4be2b-129">Ale hello pouze toosignify způsob, jak volat k prázdné složce **foo** je tak, že tento soubor speciální 0 bajtů na místě.</span><span class="sxs-lookup"><span data-stu-id="4be2b-129">But hello only way toosignify an empty folder called **foo** is by having this special 0 byte file in place.</span></span>
* <span data-ttu-id="4be2b-130">Budou uloženy speciální metadata, která je potřeba pomocí hello Hadoop systému souborů, zejména oprávnění hello a vlastníků složek hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-130">They hold special metadata that is needed by hello Hadoop file system, notably hello permissions and owners for hello folders.</span></span>

## <a name="command-line-utilities"></a><span data-ttu-id="4be2b-131">Nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4be2b-131">Command-line utilities</span></span>
<span data-ttu-id="4be2b-132">Společnost Microsoft poskytuje následující nástroje toowork s úložištěm Azure Blob hello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-132">Microsoft provides hello following utilities toowork with Azure Blob storage:</span></span>

| <span data-ttu-id="4be2b-133">Nástroj</span><span class="sxs-lookup"><span data-stu-id="4be2b-133">Tool</span></span> | <span data-ttu-id="4be2b-134">Linux</span><span class="sxs-lookup"><span data-stu-id="4be2b-134">Linux</span></span> | <span data-ttu-id="4be2b-135">OS X</span><span class="sxs-lookup"><span data-stu-id="4be2b-135">OS X</span></span> | <span data-ttu-id="4be2b-136">Windows</span><span class="sxs-lookup"><span data-stu-id="4be2b-136">Windows</span></span> |
| --- |:---:|:---:|:---:|
| <span data-ttu-id="4be2b-137">[Rozhraní příkazového řádku Azure][azurecli]</span><span class="sxs-lookup"><span data-stu-id="4be2b-137">[Azure Command-Line Interface][azurecli]</span></span> |<span data-ttu-id="4be2b-138">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-138">✔</span></span> |<span data-ttu-id="4be2b-139">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-139">✔</span></span> |<span data-ttu-id="4be2b-140">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-140">✔</span></span> |
| <span data-ttu-id="4be2b-141">[Prostředí Azure PowerShell][azure-powershell]</span><span class="sxs-lookup"><span data-stu-id="4be2b-141">[Azure PowerShell][azure-powershell]</span></span> | | |<span data-ttu-id="4be2b-142">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-142">✔</span></span> |
| <span data-ttu-id="4be2b-143">[AzCopy][azure-azcopy]</span><span class="sxs-lookup"><span data-stu-id="4be2b-143">[AzCopy][azure-azcopy]</span></span> | | |<span data-ttu-id="4be2b-144">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-144">✔</span></span> |
| [<span data-ttu-id="4be2b-145">Příkaz Hadoop</span><span class="sxs-lookup"><span data-stu-id="4be2b-145">Hadoop command</span></span>](#commandline) |<span data-ttu-id="4be2b-146">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-146">✔</span></span> |<span data-ttu-id="4be2b-147">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-147">✔</span></span> |<span data-ttu-id="4be2b-148">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-148">✔</span></span> |

> [!NOTE]
> <span data-ttu-id="4be2b-149">Při hello rozhraní příkazového řádku Azure, Azure PowerShell a AzCopy můžete všechny možné použít mimo Azure, hello Hadoop příkaz je dostupná pouze na clusteru HDInsight hello a umožňuje pouze načítání dat z hello místního systému souborů do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-149">While hello Azure CLI, Azure PowerShell, and AzCopy can all be used from outside Azure, hello Hadoop command is only available on hello HDInsight cluster and only allows loading data from hello local file system into Azure Blob storage.</span></span>
>
>

### <span data-ttu-id="4be2b-150"><a id="xplatcli"></a>Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4be2b-150"><a id="xplatcli"></a>Azure CLI</span></span>
<span data-ttu-id="4be2b-151">Hello rozhraní příkazového řádku Azure je napříč platformami nástroj, který vám umožní toomanage Azure services.</span><span class="sxs-lookup"><span data-stu-id="4be2b-151">hello Azure CLI is a cross-platform tool that allows you toomanage Azure services.</span></span> <span data-ttu-id="4be2b-152">Použijte následující kroky tooupload data tooAzure Blob storage hello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-152">Use hello following steps tooupload data tooAzure Blob storage:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="4be2b-153">[Instalace a konfigurace hello příkazového řádku Azure CLI pro Mac, Linux a Windows](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4be2b-153">[Install and configure hello Azure CLI for Mac, Linux and Windows](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="4be2b-154">Otevřete příkazový řádek, bash nebo jiné prostředí a použít hello následující tooauthenticate tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-154">Open a command prompt, bash, or other shell, and use hello following tooauthenticate tooyour Azure subscription.</span></span>

        azure login

    <span data-ttu-id="4be2b-155">Po zobrazení výzvy zadejte hello uživatelské jméno a heslo pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="4be2b-155">When prompted, enter hello user name and password for your subscription.</span></span>
3. <span data-ttu-id="4be2b-156">Zadejte následující příkaz toolist hello účty úložiště pro vaše předplatné hello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-156">Enter hello following command toolist hello storage accounts for your subscription:</span></span>

        azure storage account list
4. <span data-ttu-id="4be2b-157">Vyberte účet úložiště hello, která obsahuje objekt blob hello, které chcete toowork s, potom použijte následující příkaz tooretrieve hello klíč pro tento účet hello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-157">Select hello storage account that contains hello blob you want toowork with, then use hello following command tooretrieve hello key for this account:</span></span>

        azure storage account keys list <storage-account-name>

    <span data-ttu-id="4be2b-158">To by měla vrátit **primární** a **sekundární** klíče.</span><span class="sxs-lookup"><span data-stu-id="4be2b-158">This should return **Primary** and **Secondary** keys.</span></span> <span data-ttu-id="4be2b-159">Kopírování hello **primární** hodnotu klíče, protože se použije v dalších krocích hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-159">Copy hello **Primary** key value because it will be used in hello next steps.</span></span>
5. <span data-ttu-id="4be2b-160">Použijte následující příkaz tooretrieve seznam kontejnery objektů blob v rámci účtu úložiště hello hello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-160">Use hello following command tooretrieve a list of blob containers within hello storage account:</span></span>

        azure storage container list -a <storage-account-name> -k <primary-key>
6. <span data-ttu-id="4be2b-161">Použijte následující příkazy tooupload hello a stáhnout soubory toohello blob:</span><span class="sxs-lookup"><span data-stu-id="4be2b-161">Use hello following commands tooupload and download files toohello blob:</span></span>

   * <span data-ttu-id="4be2b-162">tooupload souboru:</span><span class="sxs-lookup"><span data-stu-id="4be2b-162">tooupload a file:</span></span>

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * <span data-ttu-id="4be2b-163">toodownload souboru:</span><span class="sxs-lookup"><span data-stu-id="4be2b-163">toodownload a file:</span></span>

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> <span data-ttu-id="4be2b-164">Pokud bude vždy pracovat s hello stejný účet úložiště, můžete nastavit hello následující proměnné prostředí místo zadávání hello účtu a klíče pro každý příkaz:</span><span class="sxs-lookup"><span data-stu-id="4be2b-164">If you will always be working with hello same storage account, you can set hello following environment variables instead of specifying hello account and key for every command:</span></span>
>
> * <span data-ttu-id="4be2b-165">**AZURE\_úložiště\_účet**: název účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="4be2b-165">**AZURE\_STORAGE\_ACCOUNT**: hello storage account name</span></span>
> * <span data-ttu-id="4be2b-166">**AZURE\_úložiště\_přístup\_klíč**: hello klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4be2b-166">**AZURE\_STORAGE\_ACCESS\_KEY**: hello storage account key</span></span>
>
>

### <span data-ttu-id="4be2b-167"><a id="powershell"></a>Prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4be2b-167"><a id="powershell"></a>Azure PowerShell</span></span>
<span data-ttu-id="4be2b-168">Prostředí Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správy vašich zatížení v Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-168">Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="4be2b-169">Informace o konfiguraci vašeho pracovní stanice toorun prostředí Azure PowerShell najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4be2b-169">For information about configuring your workstation toorun Azure PowerShell, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="4be2b-170">**tooupload místního souboru tooAzure úložiště objektů Blob**</span><span class="sxs-lookup"><span data-stu-id="4be2b-170">**tooupload a local file tooAzure Blob storage**</span></span>

1. <span data-ttu-id="4be2b-171">Konzoly Azure PowerShell otevřené hello podle pokynů v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4be2b-171">Open hello Azure PowerShell console as instructed in [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="4be2b-172">Nastavení hodnoty hello hello prvních pět proměnných v hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="4be2b-172">Set hello values of hello first five variables in hello following script:</span></span>

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get hello storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create hello storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy hello file from local workstation toohello Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. <span data-ttu-id="4be2b-173">Hello vložení do toorun konzoly Azure PowerShell hello ho toocopy hello soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="4be2b-173">Paste hello script into hello Azure PowerShell console toorun it toocopy hello file.</span></span>

<span data-ttu-id="4be2b-174">Například toowork vytvořit skripty prostředí PowerShell s HDInsight, najdete v části [nástroje HDInsight](https://github.com/blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="4be2b-174">For example PowerShell scripts created toowork with HDInsight, see [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span></span>

### <span data-ttu-id="4be2b-175"><a id="azcopy"></a>AzCopy</span><span class="sxs-lookup"><span data-stu-id="4be2b-175"><a id="azcopy"></a>AzCopy</span></span>
<span data-ttu-id="4be2b-176">AzCopy je nástroj příkazového řádku, který je určený toosimplify hello úlohy přenosu dat do a z účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-176">AzCopy is a command-line tool that is designed toosimplify hello task of transferring data into and out of an Azure Storage account.</span></span> <span data-ttu-id="4be2b-177">Můžete používat jako samostatný nástroj nebo začlenit tento nástroj v existující aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4be2b-177">You can use it as a standalone tool or incorporate this tool in an existing application.</span></span> <span data-ttu-id="4be2b-178">[Stáhněte si nástroj AzCopy][azure-azcopy-download].</span><span class="sxs-lookup"><span data-stu-id="4be2b-178">[Download AzCopy][azure-azcopy-download].</span></span>

<span data-ttu-id="4be2b-179">Hello AzCopy syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="4be2b-179">hello AzCopy syntax is:</span></span>

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

<span data-ttu-id="4be2b-180">Další informace najdete v tématu [AzCopy - nahrávání nebo stahování souborů pro objekty BLOB Azure][azure-azcopy].</span><span class="sxs-lookup"><span data-stu-id="4be2b-180">For more information, see [AzCopy - Uploading/Downloading files for Azure Blobs][azure-azcopy].</span></span>

### <span data-ttu-id="4be2b-181"><a id="commandline"></a>Hadoop příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4be2b-181"><a id="commandline"></a>Hadoop command line</span></span>
<span data-ttu-id="4be2b-182">Hello Hadoop příkazového řádku využijete pouze pro ukládání dat do úložiště objektů blob při hello dat již existuje hello hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="4be2b-182">hello Hadoop command line is only useful for storing data into blob storage when hello data is already present on hello cluster head node.</span></span>

<span data-ttu-id="4be2b-183">V pořadí toouse hello příkaz Hadoop nejprve je nutné připojit pomocí některého z následujících metod hello headnode toohello:</span><span class="sxs-lookup"><span data-stu-id="4be2b-183">In order toouse hello Hadoop command, you must first connect toohello headnode using one of hello following methods:</span></span>

* <span data-ttu-id="4be2b-184">**HDInsight se systémem Windows**: [připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="4be2b-184">**Windows-based HDInsight**: [Connect using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>
* <span data-ttu-id="4be2b-185">**HDInsight se systémem Linux**: připojení pomocí protokolu SSH ([hello příkazu SSH](hdinsight-hadoop-linux-use-ssh-unix.md) nebo [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span><span class="sxs-lookup"><span data-stu-id="4be2b-185">**Linux-based HDInsight**: Connect using SSH ([hello SSH command](hdinsight-hadoop-linux-use-ssh-unix.md) or [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span></span>

<span data-ttu-id="4be2b-186">Po připojení můžete použít následující syntaxi tooupload toostorage souboru hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-186">Once connected, you can use hello following syntax tooupload a file toostorage.</span></span>

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

<span data-ttu-id="4be2b-187">Například `hadoop fs -copyFromLocal data.txt /example/data/data.txt`.</span><span class="sxs-lookup"><span data-stu-id="4be2b-187">For example, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span></span>

<span data-ttu-id="4be2b-188">Protože hello výchozí systém souborů pro HDInsight v úložišti objektů Azure Blob, /example/data.txt ve skutečnosti je v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-188">Because hello default file system for HDInsight is in Azure Blob storage, /example/data.txt is actually in Azure Blob storage.</span></span> <span data-ttu-id="4be2b-189">Můžete se také podívat toohello souboru jako:</span><span class="sxs-lookup"><span data-stu-id="4be2b-189">You can also refer toohello file as:</span></span>

    wasb:///example/data/data.txt

<span data-ttu-id="4be2b-190">nebo</span><span class="sxs-lookup"><span data-stu-id="4be2b-190">or</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

<span data-ttu-id="4be2b-191">Seznam dalších Hadoop příkazy svou práci se soubory, najdete v části [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span><span class="sxs-lookup"><span data-stu-id="4be2b-191">For a list of other Hadoop commands that work with files, see [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span></span>

> [!WARNING]
> <span data-ttu-id="4be2b-192">Na clustery HBase použít výchozí velikost bloku hello při zápisu dat je 256KB.</span><span class="sxs-lookup"><span data-stu-id="4be2b-192">On HBase clusters, hello default block size used when writing data is 256KB.</span></span> <span data-ttu-id="4be2b-193">Když to funguje bez problémů při používání rozhraní API HBase nebo rozhraní REST API, pomocí hello `hadoop` nebo `hdfs dfs` větší než ~ 12 GB dat toowrite příkazy, které jsou výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="4be2b-193">While this works fine when using HBase APIs or REST APIs, using hello `hadoop` or `hdfs dfs` commands toowrite data larger than ~12GB results in an error.</span></span> <span data-ttu-id="4be2b-194">V tématu hello [pro zápis na objekt blob úložiště výjimka](#storageexception) části níže Další informace.</span><span class="sxs-lookup"><span data-stu-id="4be2b-194">See hello [storage exception for write on blob](#storageexception) section below for more information.</span></span>
>
>

## <a name="graphical-clients"></a><span data-ttu-id="4be2b-195">Grafické klientů</span><span class="sxs-lookup"><span data-stu-id="4be2b-195">Graphical clients</span></span>
<span data-ttu-id="4be2b-196">Existují také několik aplikací, které poskytují grafické rozhraní pro práci s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4be2b-196">There are also several applications that provide a graphical interface for working with Azure Storage.</span></span> <span data-ttu-id="4be2b-197">Hello následuje seznam několik z těchto aplikací:</span><span class="sxs-lookup"><span data-stu-id="4be2b-197">hello following is a list of a few of these applications:</span></span>

| <span data-ttu-id="4be2b-198">Klient</span><span class="sxs-lookup"><span data-stu-id="4be2b-198">Client</span></span> | <span data-ttu-id="4be2b-199">Linux</span><span class="sxs-lookup"><span data-stu-id="4be2b-199">Linux</span></span> | <span data-ttu-id="4be2b-200">OS X</span><span class="sxs-lookup"><span data-stu-id="4be2b-200">OS X</span></span> | <span data-ttu-id="4be2b-201">Windows</span><span class="sxs-lookup"><span data-stu-id="4be2b-201">Windows</span></span> |
| --- |:---:|:---:|:---:|
| [<span data-ttu-id="4be2b-202">Microsoft Visual Studio Tools pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="4be2b-202">Microsoft Visual Studio Tools for HDInsight</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |<span data-ttu-id="4be2b-203">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-203">✔</span></span> |<span data-ttu-id="4be2b-204">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-204">✔</span></span> |<span data-ttu-id="4be2b-205">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-205">✔</span></span> |
| [<span data-ttu-id="4be2b-206">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="4be2b-206">Azure Storage Explorer</span></span>](http://storageexplorer.com/) |<span data-ttu-id="4be2b-207">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-207">✔</span></span> |<span data-ttu-id="4be2b-208">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-208">✔</span></span> |<span data-ttu-id="4be2b-209">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-209">✔</span></span> |
| [<span data-ttu-id="4be2b-210">Cloudové úložiště Studio 2</span><span class="sxs-lookup"><span data-stu-id="4be2b-210">Cloud Storage Studio 2</span></span>](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |<span data-ttu-id="4be2b-211">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-211">✔</span></span> |
| [<span data-ttu-id="4be2b-212">CloudXplorer</span><span class="sxs-lookup"><span data-stu-id="4be2b-212">CloudXplorer</span></span>](http://clumsyleaf.com/products/cloudxplorer) | | |<span data-ttu-id="4be2b-213">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-213">✔</span></span> |
| [<span data-ttu-id="4be2b-214">Průzkumník Azure</span><span class="sxs-lookup"><span data-stu-id="4be2b-214">Azure Explorer</span></span>](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |<span data-ttu-id="4be2b-215">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-215">✔</span></span> |
| [<span data-ttu-id="4be2b-216">Cyberduck</span><span class="sxs-lookup"><span data-stu-id="4be2b-216">Cyberduck</span></span>](https://cyberduck.io/) | |<span data-ttu-id="4be2b-217">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-217">✔</span></span> |<span data-ttu-id="4be2b-218">✔</span><span class="sxs-lookup"><span data-stu-id="4be2b-218">✔</span></span> |

### <a name="visual-studio-tools-for-hdinsight"></a><span data-ttu-id="4be2b-219">Visual Studio Tools pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="4be2b-219">Visual Studio Tools for HDInsight</span></span>
<span data-ttu-id="4be2b-220">Další informace najdete v tématu [přejděte hello propojené prostředky](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span><span class="sxs-lookup"><span data-stu-id="4be2b-220">For more information, see [Navigate hello linked resources](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span></span>

### <span data-ttu-id="4be2b-221"><a id="storageexplorer"></a>Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="4be2b-221"><a id="storageexplorer"></a>Azure Storage Explorer</span></span>
<span data-ttu-id="4be2b-222">*Azure Storage Explorer* je užitečným nástrojem pro kontrolu a změna hello data do objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="4be2b-222">*Azure Storage Explorer* is a useful tool for inspecting and altering hello data in blobs.</span></span> <span data-ttu-id="4be2b-223">Je bezplatný nástroj, který si můžete stáhnout z [http://storageexplorer.com/](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="4be2b-223">It is a free, open source tool that can be downloaded from [http://storageexplorer.com/](http://storageexplorer.com/).</span></span> <span data-ttu-id="4be2b-224">Hello zdrojový kód je k dispozici také tento odkaz.</span><span class="sxs-lookup"><span data-stu-id="4be2b-224">hello source code is available from this link as well.</span></span>

<span data-ttu-id="4be2b-225">Před použitím nástroje hello, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-225">Before using hello tool, you must know your Azure storage account name and account key.</span></span> <span data-ttu-id="4be2b-226">Pokyny k načtení těchto informací naleznete v tématu hello "postupy: zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště" části [vytvořit, spravovat nebo odstranit účet úložiště][azure-create-storage-account].</span><span class="sxs-lookup"><span data-stu-id="4be2b-226">For instructions about getting this information, see hello "How to: View, copy and regenerate storage access keys" section of [Create, manage, or delete a storage account][azure-create-storage-account].</span></span>

1. <span data-ttu-id="4be2b-227">Spuštění Průzkumníka úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4be2b-227">Run Azure Storage Explorer.</span></span> <span data-ttu-id="4be2b-228">Pokud je tato hello poprvé spustíte hello Storage Explorer, zobrazí se výzva pro hello **název účtu _Storage** a **klíč účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4be2b-228">If this is hello first time you have run hello Storage Explorer, you will be prompted for hello **_Storage account name** and **Storage account key**.</span></span> <span data-ttu-id="4be2b-229">Pokud spustíte ji před použijte hello **přidat** tlačítko tooadd název nového účtu úložiště a klíč.</span><span class="sxs-lookup"><span data-stu-id="4be2b-229">If you have run it before, use hello **Add** button tooadd a new storage account name and key.</span></span>

    <span data-ttu-id="4be2b-230">Zadejte hello názvů a klíče pro účet úložiště hello používá váš cluster HDInsight a pak vyberte **Uložit & OTEVŘETE**.</span><span class="sxs-lookup"><span data-stu-id="4be2b-230">Enter hello name and key for hello storage account used by your HDInsight cluster and then select **SAVE & OPEN**.</span></span>

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. <span data-ttu-id="4be2b-232">V seznamu hello kontejnery toohello nalevo od rozhraní hello klikněte na název hello hello kontejneru, který je přidružený k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4be2b-232">In hello list of containers toohello left of hello interface, click hello name of hello container that is associated with your HDInsight cluster.</span></span> <span data-ttu-id="4be2b-233">Ve výchozím nastavení to je název hello hello clusteru HDInsight, ale může lišit, pokud jste zadali při vytváření clusteru hello určitý název.</span><span class="sxs-lookup"><span data-stu-id="4be2b-233">By default, this is hello name of hello HDInsight cluster, but may be different if you entered a specific name when creating hello cluster.</span></span>
3. <span data-ttu-id="4be2b-234">Panel nástrojů hello vyberte ikonu nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-234">From hello tool bar, select hello upload icon.</span></span>

    ![Panel nástrojů se zvýrazněnou ikonou nahrávání](./media/hdinsight-upload-data/toolbar.png)
4. <span data-ttu-id="4be2b-236">Zadejte soubor tooupload a pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="4be2b-236">Specify a file tooupload, and then click **Open**.</span></span> <span data-ttu-id="4be2b-237">Po zobrazení výzvy vyberte **nahrát** tooupload hello souboru toohello kořenovém kontejneru úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-237">When prompted, select **Upload** tooupload hello file toohello root of hello storage container.</span></span> <span data-ttu-id="4be2b-238">Pokud chcete, aby tooupload hello tooa konkrétní cesta, zadejte cestu hello v hello **cílové** pole a pak vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="4be2b-238">If you want tooupload hello file tooa specific path, enter hello path in hello **Destination** field and then select **Upload**.</span></span>

    ![Dialogové okno nahrání souboru](./media/hdinsight-upload-data/fileupload.png)

    <span data-ttu-id="4be2b-240">Po dokončení nahrávání souboru hello můžete z úloh na clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-240">Once hello file has finished uploading, you can use it from jobs on hello HDInsight cluster.</span></span>

## <a name="mount-azure-blob-storage-as-local-drive"></a><span data-ttu-id="4be2b-241">Připojit jako místní disk úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="4be2b-241">Mount Azure Blob Storage as Local Drive</span></span>
<span data-ttu-id="4be2b-242">V tématu [úložiště objektů Blob Azure připojit jako místní disk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span><span class="sxs-lookup"><span data-stu-id="4be2b-242">See [Mount Azure Blob Storage as Local Drive](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span></span>

## <a name="services"></a><span data-ttu-id="4be2b-243">Služby</span><span class="sxs-lookup"><span data-stu-id="4be2b-243">Services</span></span>
### <a name="azure-data-factory"></a><span data-ttu-id="4be2b-244">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4be2b-244">Azure Data Factory</span></span>
<span data-ttu-id="4be2b-245">Hello služby Azure Data Factory je plně spravovaná služba pro sestavování služby pro úložiště, zpracování dat a dat přesun dat do efektivní, škálovatelného a spolehlivého datových kanálů produkční.</span><span class="sxs-lookup"><span data-stu-id="4be2b-245">hello Azure Data Factory service is a fully managed service for composing data storage, data processing, and data movement services into streamlined, scalable, and reliable data production pipelines.</span></span>

<span data-ttu-id="4be2b-246">Azure Data Factory lze použít toomove data do úložiště objektů Blob v Azure nebo toocreate datových kanálů, které přímo pomocí HDInsight funkcí, jako například Hive a vepřových.</span><span class="sxs-lookup"><span data-stu-id="4be2b-246">Azure Data Factory can be used toomove data into Azure Blob storage, or toocreate data pipelines that directly use HDInsight features such as Hive and Pig.</span></span>

<span data-ttu-id="4be2b-247">Další informace najdete v tématu hello [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="4be2b-247">For more information, see hello [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/).</span></span>

### <span data-ttu-id="4be2b-248"><a id="sqoop"></a>Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="4be2b-248"><a id="sqoop"></a>Apache Sqoop</span></span>
<span data-ttu-id="4be2b-249">Sqoop je dat tootransfer nástroj navržený mezi Hadoop a relačními databázemi.</span><span class="sxs-lookup"><span data-stu-id="4be2b-249">Sqoop is a tool designed tootransfer data between Hadoop and relational databases.</span></span> <span data-ttu-id="4be2b-250">Můžete ji použít tooimport data ze systému správy relačních databází (RDBMS), jako je SQL Server, MySQL a Oracle do systému souborů Hadoop distributed hello (HDFS), transformovat hello data v Hadoop pomocí MapReduce nebo Hive a poté exportujte hello data zpět do RELAČNÍ.</span><span class="sxs-lookup"><span data-stu-id="4be2b-250">You can use it tooimport data from a relational database management system (RDBMS), such as SQL Server, MySQL, or Oracle into hello Hadoop distributed file system (HDFS), transform hello data in Hadoop with MapReduce or Hive, and then export hello data back into an RDBMS.</span></span>

<span data-ttu-id="4be2b-251">Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="4be2b-251">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

## <a name="development-sdks"></a><span data-ttu-id="4be2b-252">Vývoj sady SDK</span><span class="sxs-lookup"><span data-stu-id="4be2b-252">Development SDKs</span></span>
<span data-ttu-id="4be2b-253">Azure Blob storage můžete také získat přístup pomocí sady Azure SDK z hello následující programovací jazyky:</span><span class="sxs-lookup"><span data-stu-id="4be2b-253">Azure Blob storage can also be accessed using an Azure SDK from hello following programming languages:</span></span>

* <span data-ttu-id="4be2b-254">.NET</span><span class="sxs-lookup"><span data-stu-id="4be2b-254">.NET</span></span>
* <span data-ttu-id="4be2b-255">Java</span><span class="sxs-lookup"><span data-stu-id="4be2b-255">Java</span></span>
* <span data-ttu-id="4be2b-256">Node.js</span><span class="sxs-lookup"><span data-stu-id="4be2b-256">Node.js</span></span>
* <span data-ttu-id="4be2b-257">PHP</span><span class="sxs-lookup"><span data-stu-id="4be2b-257">PHP</span></span>
* <span data-ttu-id="4be2b-258">Python</span><span class="sxs-lookup"><span data-stu-id="4be2b-258">Python</span></span>
* <span data-ttu-id="4be2b-259">Ruby</span><span class="sxs-lookup"><span data-stu-id="4be2b-259">Ruby</span></span>

<span data-ttu-id="4be2b-260">Další informace o instalaci hello sadami SDK služby Azure najdete v tématu [stáhne Azure](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="4be2b-260">For more information on installing hello Azure SDKs, see [Azure downloads](https://azure.microsoft.com/downloads/)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4be2b-261">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4be2b-261">Troubleshooting</span></span>
### <span data-ttu-id="4be2b-262"><a id="storageexception"></a>Výjimka úložiště pro zápis u objektu blob</span><span class="sxs-lookup"><span data-stu-id="4be2b-262"><a id="storageexception"></a>Storage exception for write on blob</span></span>
<span data-ttu-id="4be2b-263">**Příznaky**: při použití hello `hadoop` nebo `hdfs dfs` příkazy toowrite soubory, které jsou ~ 12 GB nebo větší v clusteru služby HBase můžete setkat s hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="4be2b-263">**Symptoms**: When using hello `hadoop` or `hdfs dfs` commands toowrite files that are ~12GB or larger on an HBase cluster, you may encounter hello following error:</span></span>

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: hello request body is too large and exceeds hello maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

<span data-ttu-id="4be2b-264">**Příčina**: HBase v HDInsight clustery výchozí velikost bloku tooa 256 kB při zápisu tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="4be2b-264">**Cause**: HBase on HDInsight clusters default tooa block size of 256KB when writing tooAzure storage.</span></span> <span data-ttu-id="4be2b-265">Když tento postup funguje pro rozhraní API HBase nebo rozhraní REST API, výsledkem bude k chybě při použití hello `hadoop` nebo `hdfs dfs` nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4be2b-265">While this works for HBase APIs or REST APIs, it will result in an error when using hello `hadoop` or `hdfs dfs` command-line utilities.</span></span>

<span data-ttu-id="4be2b-266">**Řešení**: použití `fs.azure.write.request.size` toospecify větší velikost bloku.</span><span class="sxs-lookup"><span data-stu-id="4be2b-266">**Resolution**: Use `fs.azure.write.request.size` toospecify a larger block size.</span></span> <span data-ttu-id="4be2b-267">To provedete na základě za použití pomocí hello `-D` parametr.</span><span class="sxs-lookup"><span data-stu-id="4be2b-267">You can do this on a per-use basis by using hello `-D` parameter.</span></span> <span data-ttu-id="4be2b-268">Hello následuje příklad použití tohoto parametru s hello `hadoop` příkaz:</span><span class="sxs-lookup"><span data-stu-id="4be2b-268">hello following is an example using this parameter with hello `hadoop` command:</span></span>

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

<span data-ttu-id="4be2b-269">Můžete taky zvýšit hodnotu hello `fs.azure.write.request.size` globálně pomocí Ambari.</span><span class="sxs-lookup"><span data-stu-id="4be2b-269">You can also increase hello value of `fs.azure.write.request.size` globally by using Ambari.</span></span> <span data-ttu-id="4be2b-270">Hello následující postup může být použitá hodnota hello toochange v hello webové uživatelské rozhraní Ambari:</span><span class="sxs-lookup"><span data-stu-id="4be2b-270">hello following steps can be used toochange hello value in hello Ambari Web UI:</span></span>

1. <span data-ttu-id="4be2b-271">V prohlížeči přejděte toohello webové uživatelské rozhraní Ambari pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="4be2b-271">In your browser, go toohello Ambari Web UI for your cluster.</span></span> <span data-ttu-id="4be2b-272">Toto je https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="4be2b-272">This is https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your cluster.</span></span>

    <span data-ttu-id="4be2b-273">Po zobrazení výzvy zadejte hello správce jméno a heslo pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="4be2b-273">When prompted, enter hello admin name and password for hello cluster.</span></span>
2. <span data-ttu-id="4be2b-274">Hello levé straně obrazovky hello, vyberte **HDFS**a potom vyberte hello **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="4be2b-274">From hello left side of hello screen, select **HDFS**, and then select hello **Configs** tab.</span></span>
3. <span data-ttu-id="4be2b-275">V hello **filtru...**  zadejte `fs.azure.write.request.size`.</span><span class="sxs-lookup"><span data-stu-id="4be2b-275">In hello **Filter...** field, enter `fs.azure.write.request.size`.</span></span> <span data-ttu-id="4be2b-276">Tato akce zobrazí pole hello a aktuální hodnota uprostřed hello stránku hello.</span><span class="sxs-lookup"><span data-stu-id="4be2b-276">This will display hello field and current value in hello middle of hello page.</span></span>
4. <span data-ttu-id="4be2b-277">Změňte hodnotu hello z 262144 (256KB) toohello novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4be2b-277">Change hello value from 262144 (256KB) toohello new value.</span></span> <span data-ttu-id="4be2b-278">Například 4194304 (4MB).</span><span class="sxs-lookup"><span data-stu-id="4be2b-278">For example, 4194304 (4MB).</span></span>

![Obrázek změna hello prostřednictvím webové uživatelské rozhraní Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

<span data-ttu-id="4be2b-280">Další informace o používání Ambari najdete v tématu [clusterů HDInsight spravovat pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="4be2b-280">For more information on using Ambari, see [Manage HDInsight clusters using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4be2b-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4be2b-281">Next steps</span></span>
<span data-ttu-id="4be2b-282">Teď, když znáte jak číst tooget data do HDInsight, hello následující články toolearn jak tooperform analýzy:</span><span class="sxs-lookup"><span data-stu-id="4be2b-282">Now that you understand how tooget data into HDInsight, read hello following articles toolearn how tooperform analysis:</span></span>

* <span data-ttu-id="4be2b-283">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="4be2b-283">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="4be2b-284">[Odesílání úloh Hadoop prostřednictvím kódu programu][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="4be2b-284">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="4be2b-285">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="4be2b-285">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="4be2b-286">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="4be2b-286">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
