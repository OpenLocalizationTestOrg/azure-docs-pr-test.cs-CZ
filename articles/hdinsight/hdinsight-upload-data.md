---
title: "Nahrání dat pro úlohy Hadoop v HDInsight | Microsoft Docs"
description: "Zjistěte, jak nahrát a přístup k datům pro úlohy Hadoop v HDInsight pomocí rozhraní příkazového řádku Azure, Azure Storage Explorer, prostředí Azure PowerShell, příkazový řádek Hadoop nebo Sqoop."
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
ms.openlocfilehash: 6867f96c8ea0e31ed0e682cef48e7aa5e3f65f86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="900e2-104">Nahrání dat úloh Hadoopu do služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="900e2-104">Upload data for Hadoop jobs in HDInsight</span></span>
<span data-ttu-id="900e2-105">Azure HDInsight nabízí plnohodnotné distribuované systému souborů Hadoop (HDFS) v porovnání s Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="900e2-105">Azure HDInsight provides a full-featured Hadoop distributed file system (HDFS) over Azure Blob storage.</span></span> <span data-ttu-id="900e2-106">Slouží jako rozšíření HDFS a poskytuje bezproblémové prostředí pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="900e2-106">It is designed as an HDFS extension to provide a seamless experience to customers.</span></span> <span data-ttu-id="900e2-107">Umožňuje úplnou sadu součástí ekosystému Hadoop pracovat přímo na data, která spravuje.</span><span class="sxs-lookup"><span data-stu-id="900e2-107">It enables the full set of components in the Hadoop ecosystem to operate directly on the data it manages.</span></span> <span data-ttu-id="900e2-108">Azure Blob storage a HDFS jsou systémy různých souborů, které jsou optimalizované pro úložiště dat a výpočty na tato data.</span><span class="sxs-lookup"><span data-stu-id="900e2-108">Azure Blob storage and HDFS are distinct file systems that are optimized for storage of data and computations on that data.</span></span> <span data-ttu-id="900e2-109">Informace o výhodách používání úložiště objektů Blob v Azure najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="900e2-109">For information about the benefits of using Azure Blob storage, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="900e2-110">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="900e2-110">**Prerequisites**</span></span>

<span data-ttu-id="900e2-111">Než začnete, vezměte na vědomí následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="900e2-111">Note the following requirement before you begin:</span></span>

* <span data-ttu-id="900e2-112">Cluster Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="900e2-112">An Azure HDInsight cluster.</span></span> <span data-ttu-id="900e2-113">Pokyny najdete v tématu [Začínáme s Azure HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="900e2-113">For instructions, see [Get started with Azure HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span>

## <a name="why-blob-storage"></a><span data-ttu-id="900e2-114">Proč úložiště objektů blob?</span><span class="sxs-lookup"><span data-stu-id="900e2-114">Why blob storage?</span></span>
<span data-ttu-id="900e2-115">Azure HDInsight clustery jsou obvykle nasazují ke spuštění úloh MapReduce, a clustery jsou vyřazen po dokončení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="900e2-115">Azure HDInsight clusters are typically deployed to run MapReduce jobs, and the clusters are dropped after these jobs complete.</span></span> <span data-ttu-id="900e2-116">Zachovat data v HDFS clustery po dokončení se výpočty by nákladné způsob uložení dat této.</span><span class="sxs-lookup"><span data-stu-id="900e2-116">Keeping the data in the HDFS clusters after computations are complete would be an expensive way to store this data.</span></span> <span data-ttu-id="900e2-117">Azure Blob storage je vysoce dostupný, vysoce škálovatelné a vysoce kapacitu, možnosti nízkonákladového a ke sdílení úložiště pro data, která se má zpracovat pomocí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="900e2-117">Azure Blob storage is a highly available, highly scalable, high capacity, low cost, and shareable storage option for data that is to be processed using HDInsight.</span></span> <span data-ttu-id="900e2-118">Ukládání dat do objektu BLOB umožní clusterů HDInsight, které jsou používány pro výpočty bezpečně vydané bez ztráty dat.</span><span class="sxs-lookup"><span data-stu-id="900e2-118">Storing data in a blob enables the HDInsight clusters that are used for computation to be safely released without losing data.</span></span>

### <a name="directories"></a><span data-ttu-id="900e2-119">Adresáře</span><span class="sxs-lookup"><span data-stu-id="900e2-119">Directories</span></span>
<span data-ttu-id="900e2-120">Kontejnery Azure Blob storage ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů.</span><span class="sxs-lookup"><span data-stu-id="900e2-120">Azure Blob storage containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="900e2-121">Znak "/" můžete použít název klíče, ale jevit jako soubor jsou uložená v do struktury adresářů.</span><span class="sxs-lookup"><span data-stu-id="900e2-121">However the "/" character can be used within the key name to make it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="900e2-122">HDInsight uvidí tyto jako v případě, že jsou skutečné adresáře.</span><span class="sxs-lookup"><span data-stu-id="900e2-122">HDInsight sees these as if they are actual directories.</span></span>

<span data-ttu-id="900e2-123">Klíč k objektu blob může být například *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="900e2-123">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="900e2-124">Žádný skutečný "vstupní" adresář existuje, ale z důvodu přítomnosti znak "/" v názvu klíče, připomíná zobrazení cesty k souboru.</span><span class="sxs-lookup"><span data-stu-id="900e2-124">No actual "input" directory exists, but due to the presence of the "/" character in the key name, it has the appearance of a file path.</span></span>

<span data-ttu-id="900e2-125">Z toho důvodu Pokud pomocí nástroje Průzkumník Azure může dojít k některé soubory 0 bajtů.</span><span class="sxs-lookup"><span data-stu-id="900e2-125">Because of this, if you use Azure Explorer tools you may notice some 0 byte files.</span></span> <span data-ttu-id="900e2-126">Tyto soubory mají dva účely:</span><span class="sxs-lookup"><span data-stu-id="900e2-126">These files serve two purposes:</span></span>

* <span data-ttu-id="900e2-127">Pokud jsou prázdné složky, označte existence složky.</span><span class="sxs-lookup"><span data-stu-id="900e2-127">If there are empty folders, they mark of the existence of the folder.</span></span> <span data-ttu-id="900e2-128">Azure Blob storage je dostatečně inteligentní vědět, pokud existuje objekt blob názvem foo/panelu, se složku s názvem **foo**.</span><span class="sxs-lookup"><span data-stu-id="900e2-128">Azure Blob storage is clever enough to know that if a blob called foo/bar exists, there is a folder called **foo**.</span></span> <span data-ttu-id="900e2-129">Jediný způsob, jak označily k prázdné složce názvem, ale **foo** je tak, že tento soubor speciální 0 bajtů na místě.</span><span class="sxs-lookup"><span data-stu-id="900e2-129">But the only way to signify an empty folder called **foo** is by having this special 0 byte file in place.</span></span>
* <span data-ttu-id="900e2-130">Drží speciální metadata, která je potřeba systému souborů Hadoop, zejména oprávnění a vlastníků složek.</span><span class="sxs-lookup"><span data-stu-id="900e2-130">They hold special metadata that is needed by the Hadoop file system, notably the permissions and owners for the folders.</span></span>

## <a name="command-line-utilities"></a><span data-ttu-id="900e2-131">Nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="900e2-131">Command-line utilities</span></span>
<span data-ttu-id="900e2-132">Společnost Microsoft poskytuje následující nástroje pro práci s Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="900e2-132">Microsoft provides the following utilities to work with Azure Blob storage:</span></span>

| <span data-ttu-id="900e2-133">Nástroj</span><span class="sxs-lookup"><span data-stu-id="900e2-133">Tool</span></span> | <span data-ttu-id="900e2-134">Linux</span><span class="sxs-lookup"><span data-stu-id="900e2-134">Linux</span></span> | <span data-ttu-id="900e2-135">OS X</span><span class="sxs-lookup"><span data-stu-id="900e2-135">OS X</span></span> | <span data-ttu-id="900e2-136">Windows</span><span class="sxs-lookup"><span data-stu-id="900e2-136">Windows</span></span> |
| --- |:---:|:---:|:---:|
| <span data-ttu-id="900e2-137">[Rozhraní příkazového řádku Azure][azurecli]</span><span class="sxs-lookup"><span data-stu-id="900e2-137">[Azure Command-Line Interface][azurecli]</span></span> |<span data-ttu-id="900e2-138">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-138">✔</span></span> |<span data-ttu-id="900e2-139">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-139">✔</span></span> |<span data-ttu-id="900e2-140">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-140">✔</span></span> |
| <span data-ttu-id="900e2-141">[Prostředí Azure PowerShell][azure-powershell]</span><span class="sxs-lookup"><span data-stu-id="900e2-141">[Azure PowerShell][azure-powershell]</span></span> | | |<span data-ttu-id="900e2-142">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-142">✔</span></span> |
| <span data-ttu-id="900e2-143">[AzCopy][azure-azcopy]</span><span class="sxs-lookup"><span data-stu-id="900e2-143">[AzCopy][azure-azcopy]</span></span> | | |<span data-ttu-id="900e2-144">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-144">✔</span></span> |
| [<span data-ttu-id="900e2-145">Příkaz Hadoop</span><span class="sxs-lookup"><span data-stu-id="900e2-145">Hadoop command</span></span>](#commandline) |<span data-ttu-id="900e2-146">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-146">✔</span></span> |<span data-ttu-id="900e2-147">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-147">✔</span></span> |<span data-ttu-id="900e2-148">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-148">✔</span></span> |

> [!NOTE]
> <span data-ttu-id="900e2-149">Při rozhraní příkazového řádku Azure, Azure PowerShell a AzCopy můžete všechny možné použít mimo Azure, Hadoop příkaz je dostupná pouze na clusteru HDInsight a umožňuje pouze načítání dat z místního systému souborů do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-149">While the Azure CLI, Azure PowerShell, and AzCopy can all be used from outside Azure, the Hadoop command is only available on the HDInsight cluster and only allows loading data from the local file system into Azure Blob storage.</span></span>
>
>

### <span data-ttu-id="900e2-150"><a id="xplatcli"></a>Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="900e2-150"><a id="xplatcli"></a>Azure CLI</span></span>
<span data-ttu-id="900e2-151">Rozhraní příkazového řádku Azure je napříč platformami nástroj, který vám umožní spravovat služby Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-151">The Azure CLI is a cross-platform tool that allows you to manage Azure services.</span></span> <span data-ttu-id="900e2-152">Odeslání dat do úložiště objektů Blob v Azure pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="900e2-152">Use the following steps to upload data to Azure Blob storage:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="900e2-153">[Instalace a konfigurace rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="900e2-153">[Install and configure the Azure CLI for Mac, Linux and Windows](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="900e2-154">Otevřete příkazový řádek, bash nebo jiné prostředí a k ověření vašeho předplatného Azure použijte následující postupy.</span><span class="sxs-lookup"><span data-stu-id="900e2-154">Open a command prompt, bash, or other shell, and use the following to authenticate to your Azure subscription.</span></span>

        azure login

    <span data-ttu-id="900e2-155">Po zobrazení výzvy zadejte uživatelské jméno a heslo pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="900e2-155">When prompted, enter the user name and password for your subscription.</span></span>
3. <span data-ttu-id="900e2-156">Zadejte následující příkaz k zobrazení seznamu účtů úložiště pro vaše předplatné:</span><span class="sxs-lookup"><span data-stu-id="900e2-156">Enter the following command to list the storage accounts for your subscription:</span></span>

        azure storage account list
4. <span data-ttu-id="900e2-157">Vyberte účet úložiště, který obsahuje objekt blob, které chcete pracovat, potom načíst klíč pro tento účet použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="900e2-157">Select the storage account that contains the blob you want to work with, then use the following command to retrieve the key for this account:</span></span>

        azure storage account keys list <storage-account-name>

    <span data-ttu-id="900e2-158">To by měla vrátit **primární** a **sekundární** klíče.</span><span class="sxs-lookup"><span data-stu-id="900e2-158">This should return **Primary** and **Secondary** keys.</span></span> <span data-ttu-id="900e2-159">Kopírování **primární** hodnotu klíče, protože se použije v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="900e2-159">Copy the **Primary** key value because it will be used in the next steps.</span></span>
5. <span data-ttu-id="900e2-160">Použijte následující příkaz k načtení seznamu kontejnery objektů blob v rámci účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="900e2-160">Use the following command to retrieve a list of blob containers within the storage account:</span></span>

        azure storage container list -a <storage-account-name> -k <primary-key>
6. <span data-ttu-id="900e2-161">Pomocí následujících příkazů nahrávání a stahování souborů na objekt blob:</span><span class="sxs-lookup"><span data-stu-id="900e2-161">Use the following commands to upload and download files to the blob:</span></span>

   * <span data-ttu-id="900e2-162">Pro nahrání souboru:</span><span class="sxs-lookup"><span data-stu-id="900e2-162">To upload a file:</span></span>

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * <span data-ttu-id="900e2-163">Ke stažení souboru:</span><span class="sxs-lookup"><span data-stu-id="900e2-163">To download a file:</span></span>

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> <span data-ttu-id="900e2-164">Pokud budete pracovat vždy pomocí stejného účtu úložiště, můžete nastavit následující proměnné prostředí místo zadání účtu a klíče pro každý příkaz:</span><span class="sxs-lookup"><span data-stu-id="900e2-164">If you will always be working with the same storage account, you can set the following environment variables instead of specifying the account and key for every command:</span></span>
>
> * <span data-ttu-id="900e2-165">**AZURE\_úložiště\_účet**: název účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="900e2-165">**AZURE\_STORAGE\_ACCOUNT**: The storage account name</span></span>
> * <span data-ttu-id="900e2-166">**AZURE\_úložiště\_přístup\_klíč**: klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="900e2-166">**AZURE\_STORAGE\_ACCESS\_KEY**: The storage account key</span></span>
>
>

### <span data-ttu-id="900e2-167"><a id="powershell"></a>Prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="900e2-167"><a id="powershell"></a>Azure PowerShell</span></span>
<span data-ttu-id="900e2-168">Prostředí Azure PowerShell je skriptovací prostředí, které můžete řídit a automatizovat nasazení a správy vašich zatížení v Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-168">Azure PowerShell is a scripting environment that you can use to control and automate the deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="900e2-169">Informace o konfiguraci pracovní stanice ke spuštění prostředí Azure PowerShell najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="900e2-169">For information about configuring your workstation to run Azure PowerShell, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="900e2-170">**Nahrát do úložiště objektů Blob Azure do místního souboru**</span><span class="sxs-lookup"><span data-stu-id="900e2-170">**To upload a local file to Azure Blob storage**</span></span>

1. <span data-ttu-id="900e2-171">Otevřete konzolu prostředí Azure PowerShell podle pokynů v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="900e2-171">Open the Azure PowerShell console as instructed in [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="900e2-172">Nastavte hodnoty prvních pět proměnných v následující skript:</span><span class="sxs-lookup"><span data-stu-id="900e2-172">Set the values of the first five variables in the following script:</span></span>

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. <span data-ttu-id="900e2-173">Vložte skript do konzoly Azure PowerShell spouštět se zkopírovat soubor.</span><span class="sxs-lookup"><span data-stu-id="900e2-173">Paste the script into the Azure PowerShell console to run it to copy the file.</span></span>

<span data-ttu-id="900e2-174">Například skripty prostředí PowerShell, které jsou vytvořeny pro práci s HDInsight, najdete v části [nástroje HDInsight](https://github.com/blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="900e2-174">For example PowerShell scripts created to work with HDInsight, see [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span></span>

### <span data-ttu-id="900e2-175"><a id="azcopy"></a>AzCopy</span><span class="sxs-lookup"><span data-stu-id="900e2-175"><a id="azcopy"></a>AzCopy</span></span>
<span data-ttu-id="900e2-176">AzCopy je nástroj příkazového řádku, který je navržený pro zjednodušit přenosu dat do a z účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-176">AzCopy is a command-line tool that is designed to simplify the task of transferring data into and out of an Azure Storage account.</span></span> <span data-ttu-id="900e2-177">Můžete používat jako samostatný nástroj nebo začlenit tento nástroj v existující aplikaci.</span><span class="sxs-lookup"><span data-stu-id="900e2-177">You can use it as a standalone tool or incorporate this tool in an existing application.</span></span> <span data-ttu-id="900e2-178">[Stáhněte si nástroj AzCopy][azure-azcopy-download].</span><span class="sxs-lookup"><span data-stu-id="900e2-178">[Download AzCopy][azure-azcopy-download].</span></span>

<span data-ttu-id="900e2-179">AzCopy syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="900e2-179">The AzCopy syntax is:</span></span>

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

<span data-ttu-id="900e2-180">Další informace najdete v tématu [AzCopy - nahrávání nebo stahování souborů pro objekty BLOB Azure][azure-azcopy].</span><span class="sxs-lookup"><span data-stu-id="900e2-180">For more information, see [AzCopy - Uploading/Downloading files for Azure Blobs][azure-azcopy].</span></span>

### <span data-ttu-id="900e2-181"><a id="commandline"></a>Hadoop příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="900e2-181"><a id="commandline"></a>Hadoop command line</span></span>
<span data-ttu-id="900e2-182">Hadoop příkazového řádku využijete pouze pro ukládání dat do úložiště objektů blob, když se data nachází již hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="900e2-182">The Hadoop command line is only useful for storing data into blob storage when the data is already present on the cluster head node.</span></span>

<span data-ttu-id="900e2-183">Chcete-li použít příkaz Hadoop, musíte nejdřív připojit k headnode pomocí jedné z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="900e2-183">In order to use the Hadoop command, you must first connect to the headnode using one of the following methods:</span></span>

* <span data-ttu-id="900e2-184">**HDInsight se systémem Windows**: [připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="900e2-184">**Windows-based HDInsight**: [Connect using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>
* <span data-ttu-id="900e2-185">**HDInsight se systémem Linux**: připojení pomocí protokolu SSH ([příkazu SSH](hdinsight-hadoop-linux-use-ssh-unix.md) nebo [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span><span class="sxs-lookup"><span data-stu-id="900e2-185">**Linux-based HDInsight**: Connect using SSH ([the SSH command](hdinsight-hadoop-linux-use-ssh-unix.md) or [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span></span>

<span data-ttu-id="900e2-186">Po připojení můžete nahrát soubor do úložiště syntaxi.</span><span class="sxs-lookup"><span data-stu-id="900e2-186">Once connected, you can use the following syntax to upload a file to storage.</span></span>

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

<span data-ttu-id="900e2-187">Například `hadoop fs -copyFromLocal data.txt /example/data/data.txt`.</span><span class="sxs-lookup"><span data-stu-id="900e2-187">For example, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span></span>

<span data-ttu-id="900e2-188">Protože výchozí systém souborů pro HDInsight v úložišti objektů Azure Blob, /example/data.txt ve skutečnosti je v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-188">Because the default file system for HDInsight is in Azure Blob storage, /example/data.txt is actually in Azure Blob storage.</span></span> <span data-ttu-id="900e2-189">Můžete se také podívat na soubor jako:</span><span class="sxs-lookup"><span data-stu-id="900e2-189">You can also refer to the file as:</span></span>

    wasb:///example/data/data.txt

<span data-ttu-id="900e2-190">nebo</span><span class="sxs-lookup"><span data-stu-id="900e2-190">or</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

<span data-ttu-id="900e2-191">Seznam dalších Hadoop příkazy svou práci se soubory, najdete v části [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span><span class="sxs-lookup"><span data-stu-id="900e2-191">For a list of other Hadoop commands that work with files, see [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span></span>

> [!WARNING]
> <span data-ttu-id="900e2-192">Na clustery HBase blok výchozí velikost použitou při zápisu dat je 256KB.</span><span class="sxs-lookup"><span data-stu-id="900e2-192">On HBase clusters, the default block size used when writing data is 256KB.</span></span> <span data-ttu-id="900e2-193">Když to funguje bez problémů při používání rozhraní API HBase nebo rozhraní REST API, pomocí `hadoop` nebo `hdfs dfs` příkazy k zápisu dat je větší než ~ 12 GB výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="900e2-193">While this works fine when using HBase APIs or REST APIs, using the `hadoop` or `hdfs dfs` commands to write data larger than ~12GB results in an error.</span></span> <span data-ttu-id="900e2-194">Najdete v článku [pro zápis na objekt blob úložiště výjimka](#storageexception) části níže Další informace.</span><span class="sxs-lookup"><span data-stu-id="900e2-194">See the [storage exception for write on blob](#storageexception) section below for more information.</span></span>
>
>

## <a name="graphical-clients"></a><span data-ttu-id="900e2-195">Grafické klientů</span><span class="sxs-lookup"><span data-stu-id="900e2-195">Graphical clients</span></span>
<span data-ttu-id="900e2-196">Existují také několik aplikací, které poskytují grafické rozhraní pro práci s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="900e2-196">There are also several applications that provide a graphical interface for working with Azure Storage.</span></span> <span data-ttu-id="900e2-197">Následuje seznam několik z těchto aplikací:</span><span class="sxs-lookup"><span data-stu-id="900e2-197">The following is a list of a few of these applications:</span></span>

| <span data-ttu-id="900e2-198">Klient</span><span class="sxs-lookup"><span data-stu-id="900e2-198">Client</span></span> | <span data-ttu-id="900e2-199">Linux</span><span class="sxs-lookup"><span data-stu-id="900e2-199">Linux</span></span> | <span data-ttu-id="900e2-200">OS X</span><span class="sxs-lookup"><span data-stu-id="900e2-200">OS X</span></span> | <span data-ttu-id="900e2-201">Windows</span><span class="sxs-lookup"><span data-stu-id="900e2-201">Windows</span></span> |
| --- |:---:|:---:|:---:|
| [<span data-ttu-id="900e2-202">Microsoft Visual Studio Tools pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="900e2-202">Microsoft Visual Studio Tools for HDInsight</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |<span data-ttu-id="900e2-203">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-203">✔</span></span> |<span data-ttu-id="900e2-204">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-204">✔</span></span> |<span data-ttu-id="900e2-205">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-205">✔</span></span> |
| [<span data-ttu-id="900e2-206">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="900e2-206">Azure Storage Explorer</span></span>](http://storageexplorer.com/) |<span data-ttu-id="900e2-207">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-207">✔</span></span> |<span data-ttu-id="900e2-208">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-208">✔</span></span> |<span data-ttu-id="900e2-209">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-209">✔</span></span> |
| [<span data-ttu-id="900e2-210">Cloudové úložiště Studio 2</span><span class="sxs-lookup"><span data-stu-id="900e2-210">Cloud Storage Studio 2</span></span>](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |<span data-ttu-id="900e2-211">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-211">✔</span></span> |
| [<span data-ttu-id="900e2-212">CloudXplorer</span><span class="sxs-lookup"><span data-stu-id="900e2-212">CloudXplorer</span></span>](http://clumsyleaf.com/products/cloudxplorer) | | |<span data-ttu-id="900e2-213">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-213">✔</span></span> |
| [<span data-ttu-id="900e2-214">Průzkumník Azure</span><span class="sxs-lookup"><span data-stu-id="900e2-214">Azure Explorer</span></span>](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |<span data-ttu-id="900e2-215">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-215">✔</span></span> |
| [<span data-ttu-id="900e2-216">Cyberduck</span><span class="sxs-lookup"><span data-stu-id="900e2-216">Cyberduck</span></span>](https://cyberduck.io/) | |<span data-ttu-id="900e2-217">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-217">✔</span></span> |<span data-ttu-id="900e2-218">✔</span><span class="sxs-lookup"><span data-stu-id="900e2-218">✔</span></span> |

### <a name="visual-studio-tools-for-hdinsight"></a><span data-ttu-id="900e2-219">Visual Studio Tools pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="900e2-219">Visual Studio Tools for HDInsight</span></span>
<span data-ttu-id="900e2-220">Další informace najdete v tématu [procházejte propojené prostředky](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span><span class="sxs-lookup"><span data-stu-id="900e2-220">For more information, see [Navigate the linked resources](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span></span>

### <span data-ttu-id="900e2-221"><a id="storageexplorer"></a>Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="900e2-221"><a id="storageexplorer"></a>Azure Storage Explorer</span></span>
<span data-ttu-id="900e2-222">*Azure Storage Explorer* je užitečným nástrojem pro kontrolu a změna data do objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="900e2-222">*Azure Storage Explorer* is a useful tool for inspecting and altering the data in blobs.</span></span> <span data-ttu-id="900e2-223">Je bezplatný nástroj, který si můžete stáhnout z [http://storageexplorer.com/](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="900e2-223">It is a free, open source tool that can be downloaded from [http://storageexplorer.com/](http://storageexplorer.com/).</span></span> <span data-ttu-id="900e2-224">Zdrojový kód je k dispozici také tento odkaz.</span><span class="sxs-lookup"><span data-stu-id="900e2-224">The source code is available from this link as well.</span></span>

<span data-ttu-id="900e2-225">Před použitím nástroje, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-225">Before using the tool, you must know your Azure storage account name and account key.</span></span> <span data-ttu-id="900e2-226">Pokyny o načtení těchto informací najdete v tématu "postupy: zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště" části [vytvořit, spravovat nebo odstranit účet úložiště][azure-create-storage-account].</span><span class="sxs-lookup"><span data-stu-id="900e2-226">For instructions about getting this information, see the "How to: View, copy and regenerate storage access keys" section of [Create, manage, or delete a storage account][azure-create-storage-account].</span></span>

1. <span data-ttu-id="900e2-227">Spuštění Průzkumníka úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-227">Run Azure Storage Explorer.</span></span> <span data-ttu-id="900e2-228">Pokud je to poprvé spustíte Průzkumníka úložiště, zobrazí se výzva pro **název účtu _Storage** a **klíč účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="900e2-228">If this is the first time you have run the Storage Explorer, you will be prompted for the **_Storage account name** and **Storage account key**.</span></span> <span data-ttu-id="900e2-229">Pokud spustíte ji před použít **přidat** tlačítko přidáte název nového účtu úložiště a klíč.</span><span class="sxs-lookup"><span data-stu-id="900e2-229">If you have run it before, use the **Add** button to add a new storage account name and key.</span></span>

    <span data-ttu-id="900e2-230">Zadejte název a klíč pro účet úložiště používá váš cluster HDInsight a pak vyberte **Uložit & OTEVŘETE**.</span><span class="sxs-lookup"><span data-stu-id="900e2-230">Enter the name and key for the storage account used by your HDInsight cluster and then select **SAVE & OPEN**.</span></span>

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. <span data-ttu-id="900e2-232">V seznamu nalevo od rozhraní kontejnery klikněte na název kontejneru, který je přidružený k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="900e2-232">In the list of containers to the left of the interface, click the name of the container that is associated with your HDInsight cluster.</span></span> <span data-ttu-id="900e2-233">Ve výchozím nastavení to je název clusteru HDInsight, ale může lišit, pokud jste zadali při vytváření clusteru určitý název.</span><span class="sxs-lookup"><span data-stu-id="900e2-233">By default, this is the name of the HDInsight cluster, but may be different if you entered a specific name when creating the cluster.</span></span>
3. <span data-ttu-id="900e2-234">Z panelu nástrojů vyberte ikonu nahrávání.</span><span class="sxs-lookup"><span data-stu-id="900e2-234">From the tool bar, select the upload icon.</span></span>

    ![Panel nástrojů se zvýrazněnou ikonou nahrávání](./media/hdinsight-upload-data/toolbar.png)
4. <span data-ttu-id="900e2-236">Zadejte soubor, a pak klikněte na **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="900e2-236">Specify a file to upload, and then click **Open**.</span></span> <span data-ttu-id="900e2-237">Po zobrazení výzvy vyberte **nahrát** pro nahrání souboru do kořenového adresáře kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="900e2-237">When prompted, select **Upload** to upload the file to the root of the storage container.</span></span> <span data-ttu-id="900e2-238">Pokud chcete nahrát soubor do určité cesty, zadejte cestu v **cílové** pole a pak vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="900e2-238">If you want to upload the file to a specific path, enter the path in the **Destination** field and then select **Upload**.</span></span>

    ![Dialogové okno nahrání souboru](./media/hdinsight-upload-data/fileupload.png)

    <span data-ttu-id="900e2-240">Po dokončení nahrávání souboru můžete z úloh v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="900e2-240">Once the file has finished uploading, you can use it from jobs on the HDInsight cluster.</span></span>

## <a name="mount-azure-blob-storage-as-local-drive"></a><span data-ttu-id="900e2-241">Připojit jako místní disk úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="900e2-241">Mount Azure Blob Storage as Local Drive</span></span>
<span data-ttu-id="900e2-242">V tématu [úložiště objektů Blob Azure připojit jako místní disk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span><span class="sxs-lookup"><span data-stu-id="900e2-242">See [Mount Azure Blob Storage as Local Drive](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span></span>

## <a name="services"></a><span data-ttu-id="900e2-243">Služby</span><span class="sxs-lookup"><span data-stu-id="900e2-243">Services</span></span>
### <a name="azure-data-factory"></a><span data-ttu-id="900e2-244">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="900e2-244">Azure Data Factory</span></span>
<span data-ttu-id="900e2-245">Služba Azure Data Factory je plně spravovaná služba pro sestavování služby pro úložiště, zpracování dat a dat přesun dat do efektivní, škálovatelného a spolehlivého datových kanálů produkční.</span><span class="sxs-lookup"><span data-stu-id="900e2-245">The Azure Data Factory service is a fully managed service for composing data storage, data processing, and data movement services into streamlined, scalable, and reliable data production pipelines.</span></span>

<span data-ttu-id="900e2-246">Azure Data Factory slouží pro přesun dat do úložiště objektů Blob v Azure nebo k vytvoření datových kanálů, které přímo používat HDInsight funkce jako je například Hive a Pig.</span><span class="sxs-lookup"><span data-stu-id="900e2-246">Azure Data Factory can be used to move data into Azure Blob storage, or to create data pipelines that directly use HDInsight features such as Hive and Pig.</span></span>

<span data-ttu-id="900e2-247">Další informace najdete v tématu [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="900e2-247">For more information, see the [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/).</span></span>

### <span data-ttu-id="900e2-248"><a id="sqoop"></a>Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="900e2-248"><a id="sqoop"></a>Apache Sqoop</span></span>
<span data-ttu-id="900e2-249">Sqoop je nástroj sloužící k přenosu dat mezi Hadoop a relačními databázemi.</span><span class="sxs-lookup"><span data-stu-id="900e2-249">Sqoop is a tool designed to transfer data between Hadoop and relational databases.</span></span> <span data-ttu-id="900e2-250">Můžete ho pro import dat ze systému správy relačních databází (RDBMS), například SQL Server, MySQL a Oracle do systému souborů Hadoop distributed (HDFS), transformovat data v Hadoop pomocí MapReduce nebo Hive a pak exportovat data zpět do relační.</span><span class="sxs-lookup"><span data-stu-id="900e2-250">You can use it to import data from a relational database management system (RDBMS), such as SQL Server, MySQL, or Oracle into the Hadoop distributed file system (HDFS), transform the data in Hadoop with MapReduce or Hive, and then export the data back into an RDBMS.</span></span>

<span data-ttu-id="900e2-251">Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="900e2-251">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

## <a name="development-sdks"></a><span data-ttu-id="900e2-252">Vývoj sady SDK</span><span class="sxs-lookup"><span data-stu-id="900e2-252">Development SDKs</span></span>
<span data-ttu-id="900e2-253">Azure Blob storage můžete také získat přístup pomocí sady Azure SDK z těchto programovacích jazycích:</span><span class="sxs-lookup"><span data-stu-id="900e2-253">Azure Blob storage can also be accessed using an Azure SDK from the following programming languages:</span></span>

* <span data-ttu-id="900e2-254">.NET</span><span class="sxs-lookup"><span data-stu-id="900e2-254">.NET</span></span>
* <span data-ttu-id="900e2-255">Java</span><span class="sxs-lookup"><span data-stu-id="900e2-255">Java</span></span>
* <span data-ttu-id="900e2-256">Node.js</span><span class="sxs-lookup"><span data-stu-id="900e2-256">Node.js</span></span>
* <span data-ttu-id="900e2-257">PHP</span><span class="sxs-lookup"><span data-stu-id="900e2-257">PHP</span></span>
* <span data-ttu-id="900e2-258">Python</span><span class="sxs-lookup"><span data-stu-id="900e2-258">Python</span></span>
* <span data-ttu-id="900e2-259">Ruby</span><span class="sxs-lookup"><span data-stu-id="900e2-259">Ruby</span></span>

<span data-ttu-id="900e2-260">Další informace o instalaci sady Azure SDK najdete v tématu [stáhne Azure](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="900e2-260">For more information on installing the Azure SDKs, see [Azure downloads](https://azure.microsoft.com/downloads/)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="900e2-261">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="900e2-261">Troubleshooting</span></span>
### <span data-ttu-id="900e2-262"><a id="storageexception"></a>Výjimka úložiště pro zápis u objektu blob</span><span class="sxs-lookup"><span data-stu-id="900e2-262"><a id="storageexception"></a>Storage exception for write on blob</span></span>
<span data-ttu-id="900e2-263">**Příznaky**: při použití `hadoop` nebo `hdfs dfs` příkazy k zápisu souborů, které jsou ~ 12 GB nebo větší na HBase cluster, může dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="900e2-263">**Symptoms**: When using the `hadoop` or `hdfs dfs` commands to write files that are ~12GB or larger on an HBase cluster, you may encounter the following error:</span></span>

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
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

<span data-ttu-id="900e2-264">**Příčina**: HBase v HDInsight clustery výchozí velikost bloku 256 KB při zápisu do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="900e2-264">**Cause**: HBase on HDInsight clusters default to a block size of 256KB when writing to Azure storage.</span></span> <span data-ttu-id="900e2-265">Když tento postup funguje pro rozhraní API HBase nebo rozhraní REST API, výsledkem bude k chybě při použití `hadoop` nebo `hdfs dfs` nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="900e2-265">While this works for HBase APIs or REST APIs, it will result in an error when using the `hadoop` or `hdfs dfs` command-line utilities.</span></span>

<span data-ttu-id="900e2-266">**Řešení**: použití `fs.azure.write.request.size` k určení větší velikost bloku.</span><span class="sxs-lookup"><span data-stu-id="900e2-266">**Resolution**: Use `fs.azure.write.request.size` to specify a larger block size.</span></span> <span data-ttu-id="900e2-267">To provedete na základě za použití pomocí `-D` parametr.</span><span class="sxs-lookup"><span data-stu-id="900e2-267">You can do this on a per-use basis by using the `-D` parameter.</span></span> <span data-ttu-id="900e2-268">Následuje příklad použití tohoto parametru se `hadoop` příkaz:</span><span class="sxs-lookup"><span data-stu-id="900e2-268">The following is an example using this parameter with the `hadoop` command:</span></span>

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

<span data-ttu-id="900e2-269">Můžete taky zvýšit hodnotu `fs.azure.write.request.size` globálně pomocí Ambari.</span><span class="sxs-lookup"><span data-stu-id="900e2-269">You can also increase the value of `fs.azure.write.request.size` globally by using Ambari.</span></span> <span data-ttu-id="900e2-270">Následující postup slouží ke změně hodnoty v uživatelském rozhraní Ambari Web:</span><span class="sxs-lookup"><span data-stu-id="900e2-270">The following steps can be used to change the value in the Ambari Web UI:</span></span>

1. <span data-ttu-id="900e2-271">V prohlížeči přejděte na webové uživatelské rozhraní Ambari pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="900e2-271">In your browser, go to the Ambari Web UI for your cluster.</span></span> <span data-ttu-id="900e2-272">Toto je https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název clusteru.</span><span class="sxs-lookup"><span data-stu-id="900e2-272">This is https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your cluster.</span></span>

    <span data-ttu-id="900e2-273">Po zobrazení výzvy zadejte jméno správce a heslo pro cluster.</span><span class="sxs-lookup"><span data-stu-id="900e2-273">When prompted, enter the admin name and password for the cluster.</span></span>
2. <span data-ttu-id="900e2-274">Na levé straně obrazovky vyberte **HDFS**a pak vyberte **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="900e2-274">From the left side of the screen, select **HDFS**, and then select the **Configs** tab.</span></span>
3. <span data-ttu-id="900e2-275">V **filtru...**  zadejte `fs.azure.write.request.size`.</span><span class="sxs-lookup"><span data-stu-id="900e2-275">In the **Filter...** field, enter `fs.azure.write.request.size`.</span></span> <span data-ttu-id="900e2-276">Tato akce zobrazí pole a aktuální hodnotu uprostřed stránky.</span><span class="sxs-lookup"><span data-stu-id="900e2-276">This will display the field and current value in the middle of the page.</span></span>
4. <span data-ttu-id="900e2-277">Změňte hodnotu z 262144 (256KB) na novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="900e2-277">Change the value from 262144 (256KB) to the new value.</span></span> <span data-ttu-id="900e2-278">Například 4194304 (4MB).</span><span class="sxs-lookup"><span data-stu-id="900e2-278">For example, 4194304 (4MB).</span></span>

![Obrázek změna prostřednictvím webové uživatelské rozhraní Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

<span data-ttu-id="900e2-280">Další informace o používání Ambari najdete v tématu [Správa clusterů HDInsight pomocí webového uživatelského rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="900e2-280">For more information on using Ambari, see [Manage HDInsight clusters using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="900e2-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="900e2-281">Next steps</span></span>
<span data-ttu-id="900e2-282">Teď, když chápete, jak získat data do HDInsight, přečtěte si zjistěte, jak provádět analýzy v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="900e2-282">Now that you understand how to get data into HDInsight, read the following articles to learn how to perform analysis:</span></span>

* <span data-ttu-id="900e2-283">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="900e2-283">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="900e2-284">[Odesílání úloh Hadoop prostřednictvím kódu programu][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="900e2-284">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="900e2-285">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="900e2-285">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="900e2-286">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="900e2-286">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>

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
