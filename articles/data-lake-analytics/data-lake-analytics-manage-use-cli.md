---
title: "aaaManage Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage účtů Data Lake Analytics, datových zdrojů, úloh a uživatele pomocí rozhraní příkazového řádku Azure"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0af1f89080739b39f6980989b7694734cc135715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a><span data-ttu-id="8b84f-103">Správa Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure (CLI)</span><span class="sxs-lookup"><span data-stu-id="8b84f-103">Manage Azure Data Lake Analytics using Azure Command-line Interface (CLI)</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="8b84f-104">Zjistěte, jak hello toomanage účtů Azure Data Lake Analytics, zdroje dat, uživatelů a úloh pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8b84f-104">Learn how toomanage Azure Data Lake Analytics accounts, data sources, users, and jobs using hello Azure CLI.</span></span> <span data-ttu-id="8b84f-105">témata týkající se řízení toosee pomocí jiných nástrojů, klikněte na výběr karty hello výše.</span><span class="sxs-lookup"><span data-stu-id="8b84f-105">toosee management topics using other tools, click hello tab select above.</span></span>


<span data-ttu-id="8b84f-106">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="8b84f-106">**Prerequisites**</span></span>

<span data-ttu-id="8b84f-107">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="8b84f-107">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="8b84f-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="8b84f-108">**An Azure subscription**.</span></span> <span data-ttu-id="8b84f-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8b84f-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8b84f-110">**Rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="8b84f-110">**Azure CLI**.</span></span> <span data-ttu-id="8b84f-111">Viz téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8b84f-111">See [Install and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="8b84f-112">Stáhněte a nainstalujte hello **předběžné verze** [nástrojů příkazového řádku Azure](https://github.com/MicrosoftBigData/AzureDataLake/releases) v pořadí toocomplete tuto ukázku.</span><span class="sxs-lookup"><span data-stu-id="8b84f-112">Download and install hello **pre-release** [Azure CLI tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) in order toocomplete this demo.</span></span>
* <span data-ttu-id="8b84f-113">**Ověřování**pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b84f-113">**Authentication**, using hello following command:</span></span>
  
        azure login
    <span data-ttu-id="8b84f-114">Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="8b84f-114">For more information on authenticating using a work or school account, see [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="8b84f-115">**Přepínač režimu Azure Resource Manager toohello**pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b84f-115">**Switch toohello Azure Resource Manager mode**, using hello following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="8b84f-116">**Data Lake Store a Data Lake Analytics příkazy serveru toolist hello:**</span><span class="sxs-lookup"><span data-stu-id="8b84f-116">**toolist hello Data Lake Store and Data Lake Analytics commands:**</span></span>

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a><span data-ttu-id="8b84f-117">Správa účtů</span><span class="sxs-lookup"><span data-stu-id="8b84f-117">Manage accounts</span></span>
<span data-ttu-id="8b84f-118">Před spuštěním jakékoli úlohy Data Lake Analytics, musí mít účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="8b84f-118">Before running any Data Lake Analytics jobs, you must have a Data Lake Analytics account.</span></span> <span data-ttu-id="8b84f-119">Na rozdíl od Azure HDInsight nemáte platí pro účet Analytics když úlohu není spuštěn.</span><span class="sxs-lookup"><span data-stu-id="8b84f-119">Unlike Azure HDInsight, you don't pay for an Analytics account when it is not running a job.</span></span>  <span data-ttu-id="8b84f-120">Platíte jenom hello dobu, kdy je spuštěná úloha.</span><span class="sxs-lookup"><span data-stu-id="8b84f-120">You only pay for hello time when it is running a job.</span></span>  <span data-ttu-id="8b84f-121">Další informace najdete v tématu [přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8b84f-121">For more information, see [Azure Data Lake Analytics Overview](data-lake-analytics-overview.md).</span></span>  

### <a name="create-accounts"></a><span data-ttu-id="8b84f-122">Vytvoření účtů</span><span class="sxs-lookup"><span data-stu-id="8b84f-122">Create accounts</span></span>
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a><span data-ttu-id="8b84f-123">Aktualizace účtů</span><span class="sxs-lookup"><span data-stu-id="8b84f-123">Update accounts</span></span>
<span data-ttu-id="8b84f-124">Hello následující příkaz aktualizuje vlastnosti hello stávajícího účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8b84f-124">hello following command updates hello properties of an existing Data Lake Analytics Account</span></span>

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a><span data-ttu-id="8b84f-125">Výpis účtů</span><span class="sxs-lookup"><span data-stu-id="8b84f-125">List accounts</span></span>
<span data-ttu-id="8b84f-126">Účty seznamu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8b84f-126">List Data Lake Analytics accounts</span></span> 

    azure datalake analytics account list

<span data-ttu-id="8b84f-127">Seznam Data Lake Analytics účtů v rámci určité skupiny zdrojů</span><span class="sxs-lookup"><span data-stu-id="8b84f-127">List Data Lake Analytics accounts within a specific resource group</span></span>

    azure datalake analytics account list -g "<Azure Resource Group Name>"

<span data-ttu-id="8b84f-128">Podrobnosti o konkrétní účet Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8b84f-128">Get details of a specific Data Lake Analytics account</span></span>

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a><span data-ttu-id="8b84f-129">Odstranění účtů Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8b84f-129">Delete Data Lake Analytics accounts</span></span>
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a><span data-ttu-id="8b84f-130">Správa zdrojů dat účtu</span><span class="sxs-lookup"><span data-stu-id="8b84f-130">Manage account data sources</span></span>
<span data-ttu-id="8b84f-131">Data Lake Analytics teď podporuje hello následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="8b84f-131">Data Lake Analytics currently supports hello following data sources:</span></span>

* [<span data-ttu-id="8b84f-132">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="8b84f-132">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="8b84f-133">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8b84f-133">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="8b84f-134">Když vytvoříte účet Analytics, je třeba určit účet Azure Data Lake Storage účet toobe hello výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="8b84f-134">When you create an Analytics account, you must designate an Azure Data Lake Storage account toobe hello default storage account.</span></span> <span data-ttu-id="8b84f-135">Hello výchozí účet úložiště ADL slouží metadata toostore úlohy a úlohy protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="8b84f-135">hello default ADL storage account is used toostore job metadata and job audit logs.</span></span> <span data-ttu-id="8b84f-136">Po vytvoření účtu Analytics můžete přidat další účty Data Lake Storage a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8b84f-136">After you have created an Analytics account, you can add additional Data Lake Storage accounts and/or Azure Storage account.</span></span> 

### <a name="find-hello-default-adl-storage-account"></a><span data-ttu-id="8b84f-137">Najít hello výchozí účet úložiště ADL</span><span class="sxs-lookup"><span data-stu-id="8b84f-137">Find hello default ADL storage account</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

<span data-ttu-id="8b84f-138">Hodnota Hello je uveden v části vlastnosti: datalakeStoreAccount:name.</span><span class="sxs-lookup"><span data-stu-id="8b84f-138">hello value is listed under properties:datalakeStoreAccount:name.</span></span>

### <a name="add-additional-azure-blob-storage-accounts"></a><span data-ttu-id="8b84f-139">Přidat další účty úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="8b84f-139">Add additional Azure Blob storage accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> <span data-ttu-id="8b84f-140">Jsou podporovány pouze objekt Blob úložiště krátké názvy.</span><span class="sxs-lookup"><span data-stu-id="8b84f-140">Only Blob storage short names are supported.</span></span>  <span data-ttu-id="8b84f-141">Nepoužívejte plně kvalifikovaný název domény, například "myblob.blob.core.windows.net".</span><span class="sxs-lookup"><span data-stu-id="8b84f-141">Don't use FQDN, for example "myblob.blob.core.windows.net".</span></span>
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a><span data-ttu-id="8b84f-142">Přidat další účty Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="8b84f-142">Add additional Data Lake Store accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

<span data-ttu-id="8b84f-143">[-d] se tooindicate volitelný přepínač, jestli je hello Data Lake přidávané hello výchozí účet Data Lake.</span><span class="sxs-lookup"><span data-stu-id="8b84f-143">[-d] is an optional switch tooindicate whether hello Data Lake being added is hello default Data Lake account.</span></span> 

### <a name="update-existing-data-source"></a><span data-ttu-id="8b84f-144">Aktualizovat stávající zdroj dat</span><span class="sxs-lookup"><span data-stu-id="8b84f-144">Update existing data source</span></span>
<span data-ttu-id="8b84f-145">tooset existující Data Lake Store účtu toobe hello výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="8b84f-145">tooset an existing Data Lake Store account toobe hello default:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

<span data-ttu-id="8b84f-146">tooupdate existující klíč účtu úložiště objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="8b84f-146">tooupdate an existing Blob storage account key:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a><span data-ttu-id="8b84f-147">Seznam zdrojů dat:</span><span class="sxs-lookup"><span data-stu-id="8b84f-147">List data sources:</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Data Lake Analytics seznamu zdroj dat](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a><span data-ttu-id="8b84f-149">Odstraňte zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="8b84f-149">Delete data sources:</span></span>
<span data-ttu-id="8b84f-150">toodelete účtu Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="8b84f-150">toodelete a Data Lake Store account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

<span data-ttu-id="8b84f-151">toodelete účtu úložiště Blob:</span><span class="sxs-lookup"><span data-stu-id="8b84f-151">toodelete a Blob storage account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a><span data-ttu-id="8b84f-152">Správa úloh</span><span class="sxs-lookup"><span data-stu-id="8b84f-152">Manage jobs</span></span>
<span data-ttu-id="8b84f-153">Abyste mohli vytvořit úlohu, musí mít účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="8b84f-153">You must have a Data Lake Analytics account before you can create a job.</span></span>  <span data-ttu-id="8b84f-154">Další informace najdete v tématu [Správa Data Lake Analytics účty](#manage-accounts).</span><span class="sxs-lookup"><span data-stu-id="8b84f-154">For more information, see [Manage Data Lake Analytics accounts](#manage-accounts).</span></span>

### <a name="list-jobs"></a><span data-ttu-id="8b84f-155">Seznam úloh</span><span class="sxs-lookup"><span data-stu-id="8b84f-155">List jobs</span></span>
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Data Lake Analytics seznamu zdroj dat](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a><span data-ttu-id="8b84f-157">Získání podrobností o úlohách</span><span class="sxs-lookup"><span data-stu-id="8b84f-157">Get job details</span></span>
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a><span data-ttu-id="8b84f-158">Odesílání úloh</span><span class="sxs-lookup"><span data-stu-id="8b84f-158">Submit jobs</span></span>
> [!NOTE]
> <span data-ttu-id="8b84f-159">Hello výchozí prioritu úlohy je 1000 a hello výchozí stupně paralelního zpracování v rámci úlohy je 1.</span><span class="sxs-lookup"><span data-stu-id="8b84f-159">hello default priority of a job is 1000, and hello default degree of parallelism for a job is 1.</span></span>
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a><span data-ttu-id="8b84f-160">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="8b84f-160">Cancel jobs</span></span>
<span data-ttu-id="8b84f-161">Použijte hello seznamu příkaz toofind hello id úlohy a potom použijte úlohu hello toocancel Storno.</span><span class="sxs-lookup"><span data-stu-id="8b84f-161">Use hello list command toofind hello job id, and then use cancel toocancel hello job.</span></span>

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a><span data-ttu-id="8b84f-162">Správa katalogu</span><span class="sxs-lookup"><span data-stu-id="8b84f-162">Manage catalog</span></span>
<span data-ttu-id="8b84f-163">katalog Hello U-SQL je použité toostructure dat a kódu, takže může být sdílen skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="8b84f-163">hello U-SQL catalog is used toostructure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="8b84f-164">katalog Hello umožňuje hello nejvyšší možný výkon s daty v Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="8b84f-164">hello catalog enables hello highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="8b84f-165">Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="8b84f-165">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-catalog-items"></a><span data-ttu-id="8b84f-166">Seznam položek katalogu</span><span class="sxs-lookup"><span data-stu-id="8b84f-166">List catalog items</span></span>
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

<span data-ttu-id="8b84f-167">typy Hello obsahují databáze, schéma, sestavení, externí zdroj dat, tabulka, funkci vracející tabulku nebo statistiky tabulky.</span><span class="sxs-lookup"><span data-stu-id="8b84f-167">hello types include database, schema, assembly, external data source, table, table valued function or table statistics.</span></span>

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a><span data-ttu-id="8b84f-168">Použití skupin ARM</span><span class="sxs-lookup"><span data-stu-id="8b84f-168">Use ARM groups</span></span>
<span data-ttu-id="8b84f-169">Aplikace obvykle obsahují celou řadu součástí, například webovou aplikaci, databázi, databázový server, úložiště a služby třetích stran.</span><span class="sxs-lookup"><span data-stu-id="8b84f-169">Applications are typically made up of many components, for example a web app, database, database server, storage, and 3rd party services.</span></span> <span data-ttu-id="8b84f-170">Azure Resource Manager (ARM) umožňuje toowork s hello prostředky v aplikaci jako skupina, která označují tooas skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="8b84f-170">Azure Resource Manager (ARM) enables you toowork with hello resources in your application as a group, referred tooas an Azure Resource Group.</span></span> <span data-ttu-id="8b84f-171">Můžete nasadit, aktualizovat, sledovat nebo odstranit všechny prostředky hello pro vaši aplikaci v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="8b84f-171">You can deploy, update, monitor or delete all of hello resources for your application in a single, coordinated operation.</span></span> <span data-ttu-id="8b84f-172">Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako je testovací, přípravné nebo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b84f-172">You use a template for deployment and that template can work for different environments such as testing, staging and production.</span></span> <span data-ttu-id="8b84f-173">Můžete zpřehlednit fakturaci pro vaši organizaci zobrazením hello zahrnuté náklady pro celou skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="8b84f-173">You can clarify billing for your organization by viewing hello rolled-up costs for hello entire group.</span></span> <span data-ttu-id="8b84f-174">Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8b84f-174">For more information, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span> 

<span data-ttu-id="8b84f-175">Služba Data Lake Analytics může zahrnovat hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="8b84f-175">A Data Lake Analytics service can include hello following components:</span></span>

* <span data-ttu-id="8b84f-176">Účet Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8b84f-176">Azure Data Lake Analytics account</span></span>
* <span data-ttu-id="8b84f-177">Vyžaduje výchozí účet Azure Data Lake Storage</span><span class="sxs-lookup"><span data-stu-id="8b84f-177">Required default Azure Data Lake Storage account</span></span>
* <span data-ttu-id="8b84f-178">Účty úložiště další Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="8b84f-178">Additional Azure Data Lake Storage accounts</span></span>
* <span data-ttu-id="8b84f-179">Další účty Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8b84f-179">Additional Azure Storage accounts</span></span>

<span data-ttu-id="8b84f-180">Můžete vytvořit všechny tyto součásti v rámci jedné skupiny toomake ARM je snazší toomanage.</span><span class="sxs-lookup"><span data-stu-id="8b84f-180">You can create all these components under one ARM group toomake them easier toomanage.</span></span>

![Účet Azure Data Lake Analytics a úložiště](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

<span data-ttu-id="8b84f-182">Účet Data Lake Analytics a hello závislého úložiště účty musí být umístěny v hello stejné datové centrum Azure.</span><span class="sxs-lookup"><span data-stu-id="8b84f-182">A Data Lake Analytics account and hello dependent storage accounts must be placed in hello same Azure data center.</span></span>
<span data-ttu-id="8b84f-183">skupiny ARM Hello ale může být umístěno v různých datových center.</span><span class="sxs-lookup"><span data-stu-id="8b84f-183">hello ARM group however can be located in a different data center.</span></span>  

## <a name="see-also"></a><span data-ttu-id="8b84f-184">Viz také</span><span class="sxs-lookup"><span data-stu-id="8b84f-184">See also</span></span>
* [<span data-ttu-id="8b84f-185">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="8b84f-185">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="8b84f-186">Začínáme se službou Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8b84f-186">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="8b84f-187">Správa Azure Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8b84f-187">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="8b84f-188">Sledování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8b84f-188">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

