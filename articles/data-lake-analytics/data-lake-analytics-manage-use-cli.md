---
title: "Správa Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se spravovat účty Data Lake Analytics, zdrojů dat, úlohy a uživatele pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: f90bada3572c0ed40b07d76ec02c1b499bbd1428
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a><span data-ttu-id="d3fbc-103">Správa Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure (CLI)</span><span class="sxs-lookup"><span data-stu-id="d3fbc-103">Manage Azure Data Lake Analytics using Azure Command-line Interface (CLI)</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="d3fbc-104">Zjistěte, jak pro správu účtů Azure Data Lake Analytics, zdroje dat, uživatelů a úloh pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-104">Learn how to manage Azure Data Lake Analytics accounts, data sources, users, and jobs using the Azure CLI.</span></span> <span data-ttu-id="d3fbc-105">Pokud chcete zobrazit témata týkající se řízení pomocí jiných nástrojů, klikněte na výběr karty výše.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-105">To see management topics using other tools, click the tab select above.</span></span>


<span data-ttu-id="d3fbc-106">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="d3fbc-106">**Prerequisites**</span></span>

<span data-ttu-id="d3fbc-107">Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-107">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="d3fbc-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-108">**An Azure subscription**.</span></span> <span data-ttu-id="d3fbc-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d3fbc-110">**Rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-110">**Azure CLI**.</span></span> <span data-ttu-id="d3fbc-111">Viz téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-111">See [Install and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="d3fbc-112">Pokud chcete absolvovat tuto ukázku, stáhněte a nainstalujte **předběžnou verzi** [nástrojů příkazového řádku Azure](https://github.com/MicrosoftBigData/AzureDataLake/releases).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-112">Download and install the **pre-release** [Azure CLI tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) in order to complete this demo.</span></span>
* <span data-ttu-id="d3fbc-113">**Ověřování**, a to pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-113">**Authentication**, using the following command:</span></span>
  
        azure login
    <span data-ttu-id="d3fbc-114">Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [Připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-114">For more information on authenticating using a work or school account, see [Connect to an Azure subscription from the Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="d3fbc-115">**Přepněte do režimu Azure Resource Manager**, a to pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-115">**Switch to the Azure Resource Manager mode**, using the following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="d3fbc-116">**Seznam příkazů Data Lake Store a Data Lake Analytics:**</span><span class="sxs-lookup"><span data-stu-id="d3fbc-116">**To list the Data Lake Store and Data Lake Analytics commands:**</span></span>

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a><span data-ttu-id="d3fbc-117">Správa účtů</span><span class="sxs-lookup"><span data-stu-id="d3fbc-117">Manage accounts</span></span>
<span data-ttu-id="d3fbc-118">Před spuštěním jakékoli úlohy Data Lake Analytics, musí mít účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-118">Before running any Data Lake Analytics jobs, you must have a Data Lake Analytics account.</span></span> <span data-ttu-id="d3fbc-119">Na rozdíl od Azure HDInsight nemáte platí pro účet Analytics když úlohu není spuštěn.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-119">Unlike Azure HDInsight, you don't pay for an Analytics account when it is not running a job.</span></span>  <span data-ttu-id="d3fbc-120">Platíte jenom čas, kdy je spuštěná úloha.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-120">You only pay for the time when it is running a job.</span></span>  <span data-ttu-id="d3fbc-121">Další informace najdete v tématu [přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-121">For more information, see [Azure Data Lake Analytics Overview](data-lake-analytics-overview.md).</span></span>  

### <a name="create-accounts"></a><span data-ttu-id="d3fbc-122">Vytvoření účtů</span><span class="sxs-lookup"><span data-stu-id="d3fbc-122">Create accounts</span></span>
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a><span data-ttu-id="d3fbc-123">Aktualizace účtů</span><span class="sxs-lookup"><span data-stu-id="d3fbc-123">Update accounts</span></span>
<span data-ttu-id="d3fbc-124">Příkaz aktualizuje vlastnosti existující účet Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d3fbc-124">The following command updates the properties of an existing Data Lake Analytics Account</span></span>

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a><span data-ttu-id="d3fbc-125">Výpis účtů</span><span class="sxs-lookup"><span data-stu-id="d3fbc-125">List accounts</span></span>
<span data-ttu-id="d3fbc-126">Účty seznamu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d3fbc-126">List Data Lake Analytics accounts</span></span> 

    azure datalake analytics account list

<span data-ttu-id="d3fbc-127">Seznam Data Lake Analytics účtů v rámci určité skupiny zdrojů</span><span class="sxs-lookup"><span data-stu-id="d3fbc-127">List Data Lake Analytics accounts within a specific resource group</span></span>

    azure datalake analytics account list -g "<Azure Resource Group Name>"

<span data-ttu-id="d3fbc-128">Podrobnosti o konkrétní účet Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d3fbc-128">Get details of a specific Data Lake Analytics account</span></span>

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a><span data-ttu-id="d3fbc-129">Odstranění účtů Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d3fbc-129">Delete Data Lake Analytics accounts</span></span>
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a><span data-ttu-id="d3fbc-130">Správa zdrojů dat účtu</span><span class="sxs-lookup"><span data-stu-id="d3fbc-130">Manage account data sources</span></span>
<span data-ttu-id="d3fbc-131">Data Lake Analytics teď podporuje následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-131">Data Lake Analytics currently supports the following data sources:</span></span>

* [<span data-ttu-id="d3fbc-132">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d3fbc-132">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="d3fbc-133">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d3fbc-133">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="d3fbc-134">Když vytvoříte účet Analytics, je třeba určit účet Azure Data Lake Storage výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-134">When you create an Analytics account, you must designate an Azure Data Lake Storage account to be the default storage account.</span></span> <span data-ttu-id="d3fbc-135">Výchozí účet úložiště ADL se používá k ukládání metadat a úlohy auditu v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-135">The default ADL storage account is used to store job metadata and job audit logs.</span></span> <span data-ttu-id="d3fbc-136">Po vytvoření účtu Analytics můžete přidat další účty Data Lake Storage a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-136">After you have created an Analytics account, you can add additional Data Lake Storage accounts and/or Azure Storage account.</span></span> 

### <a name="find-the-default-adl-storage-account"></a><span data-ttu-id="d3fbc-137">Najít výchozí účet úložiště ADL</span><span class="sxs-lookup"><span data-stu-id="d3fbc-137">Find the default ADL storage account</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

<span data-ttu-id="d3fbc-138">Hodnota je uvedená v části vlastnosti: datalakeStoreAccount:name.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-138">The value is listed under properties:datalakeStoreAccount:name.</span></span>

### <a name="add-additional-azure-blob-storage-accounts"></a><span data-ttu-id="d3fbc-139">Přidat další účty úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="d3fbc-139">Add additional Azure Blob storage accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> <span data-ttu-id="d3fbc-140">Jsou podporovány pouze objekt Blob úložiště krátké názvy.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-140">Only Blob storage short names are supported.</span></span>  <span data-ttu-id="d3fbc-141">Nepoužívejte plně kvalifikovaný název domény, například "myblob.blob.core.windows.net".</span><span class="sxs-lookup"><span data-stu-id="d3fbc-141">Don't use FQDN, for example "myblob.blob.core.windows.net".</span></span>
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a><span data-ttu-id="d3fbc-142">Přidat další účty Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d3fbc-142">Add additional Data Lake Store accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

<span data-ttu-id="d3fbc-143">[-d] je nepovinný přepínač, který označuje, zda Data Lake, který chcete přidat do výchozího účtu Data Lake.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-143">[-d] is an optional switch to indicate whether the Data Lake being added is the default Data Lake account.</span></span> 

### <a name="update-existing-data-source"></a><span data-ttu-id="d3fbc-144">Aktualizovat stávající zdroj dat</span><span class="sxs-lookup"><span data-stu-id="d3fbc-144">Update existing data source</span></span>
<span data-ttu-id="d3fbc-145">Existující účet Data Lake Store jako výchozí nastavení:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-145">To set an existing Data Lake Store account to be the default:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

<span data-ttu-id="d3fbc-146">Aktualizace existujícího klíče účtu úložiště objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-146">To update an existing Blob storage account key:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a><span data-ttu-id="d3fbc-147">Seznam zdrojů dat:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-147">List data sources:</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Data Lake Analytics seznamu zdroj dat](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a><span data-ttu-id="d3fbc-149">Odstraňte zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-149">Delete data sources:</span></span>
<span data-ttu-id="d3fbc-150">Odstranění účtu Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-150">To delete a Data Lake Store account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

<span data-ttu-id="d3fbc-151">Chcete odstranit účet úložiště objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-151">To delete a Blob storage account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a><span data-ttu-id="d3fbc-152">Správa úloh</span><span class="sxs-lookup"><span data-stu-id="d3fbc-152">Manage jobs</span></span>
<span data-ttu-id="d3fbc-153">Abyste mohli vytvořit úlohu, musí mít účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-153">You must have a Data Lake Analytics account before you can create a job.</span></span>  <span data-ttu-id="d3fbc-154">Další informace najdete v tématu [Správa Data Lake Analytics účty](#manage-accounts).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-154">For more information, see [Manage Data Lake Analytics accounts](#manage-accounts).</span></span>

### <a name="list-jobs"></a><span data-ttu-id="d3fbc-155">Seznam úloh</span><span class="sxs-lookup"><span data-stu-id="d3fbc-155">List jobs</span></span>
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Data Lake Analytics seznamu zdroj dat](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a><span data-ttu-id="d3fbc-157">Získání podrobností o úlohách</span><span class="sxs-lookup"><span data-stu-id="d3fbc-157">Get job details</span></span>
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a><span data-ttu-id="d3fbc-158">Odesílání úloh</span><span class="sxs-lookup"><span data-stu-id="d3fbc-158">Submit jobs</span></span>
> [!NOTE]
> <span data-ttu-id="d3fbc-159">Výchozí prioritu úlohy je 1000 a výchozí stupně paralelního zpracování v rámci úlohy je 1.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-159">The default priority of a job is 1000, and the default degree of parallelism for a job is 1.</span></span>
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a><span data-ttu-id="d3fbc-160">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="d3fbc-160">Cancel jobs</span></span>
<span data-ttu-id="d3fbc-161">Pomocí příkazu seznamu najít id úlohy a pak použijte Storno Chcete zrušit úlohu.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-161">Use the list command to find the job id, and then use cancel to cancel the job.</span></span>

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a><span data-ttu-id="d3fbc-162">Správa katalogu</span><span class="sxs-lookup"><span data-stu-id="d3fbc-162">Manage catalog</span></span>
<span data-ttu-id="d3fbc-163">Katalogu U-SQL se používá k struktury kódu a dat, může být sdílen skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-163">The U-SQL catalog is used to structure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="d3fbc-164">Katalogu umožňuje nejvyšší možný s daty v Azure Data Lake výkon.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-164">The catalog enables the highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="d3fbc-165">Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-165">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-catalog-items"></a><span data-ttu-id="d3fbc-166">Seznam položek katalogu</span><span class="sxs-lookup"><span data-stu-id="d3fbc-166">List catalog items</span></span>
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

<span data-ttu-id="d3fbc-167">Typy zahrnují databáze, schéma, sestavení, externí zdroj dat, tabulka, funkci vracející tabulku nebo statistiky tabulky.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-167">The types include database, schema, assembly, external data source, table, table valued function or table statistics.</span></span>

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a><span data-ttu-id="d3fbc-168">Použití skupin ARM</span><span class="sxs-lookup"><span data-stu-id="d3fbc-168">Use ARM groups</span></span>
<span data-ttu-id="d3fbc-169">Aplikace obvykle obsahují celou řadu součástí, například webovou aplikaci, databázi, databázový server, úložiště a služby třetích stran.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-169">Applications are typically made up of many components, for example a web app, database, database server, storage, and 3rd party services.</span></span> <span data-ttu-id="d3fbc-170">Azure Resource Manager (ARM) umožňuje pracovat s prostředky v aplikaci jako se skupinou, která se nazývá Skupina prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-170">Azure Resource Manager (ARM) enables you to work with the resources in your application as a group, referred to as an Azure Resource Group.</span></span> <span data-ttu-id="d3fbc-171">Všechny prostředky pro aplikaci můžete nasadit, aktualizovat, sledovat nebo odstranit v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-171">You can deploy, update, monitor or delete all of the resources for your application in a single, coordinated operation.</span></span> <span data-ttu-id="d3fbc-172">Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako je testovací, přípravné nebo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-172">You use a template for deployment and that template can work for different environments such as testing, staging and production.</span></span> <span data-ttu-id="d3fbc-173">Můžete zpřehlednit fakturaci ve své organizaci tím, že zobrazíte souhrnné náklady za celou skupinu.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-173">You can clarify billing for your organization by viewing the rolled-up costs for the entire group.</span></span> <span data-ttu-id="d3fbc-174">Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3fbc-174">For more information, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span> 

<span data-ttu-id="d3fbc-175">Služba Data Lake Analytics může zahrnovat následující součásti:</span><span class="sxs-lookup"><span data-stu-id="d3fbc-175">A Data Lake Analytics service can include the following components:</span></span>

* <span data-ttu-id="d3fbc-176">Účet Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d3fbc-176">Azure Data Lake Analytics account</span></span>
* <span data-ttu-id="d3fbc-177">Vyžaduje výchozí účet Azure Data Lake Storage</span><span class="sxs-lookup"><span data-stu-id="d3fbc-177">Required default Azure Data Lake Storage account</span></span>
* <span data-ttu-id="d3fbc-178">Účty úložiště další Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="d3fbc-178">Additional Azure Data Lake Storage accounts</span></span>
* <span data-ttu-id="d3fbc-179">Další účty Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d3fbc-179">Additional Azure Storage accounts</span></span>

<span data-ttu-id="d3fbc-180">Můžete vytvořit všechny tyto součásti v rámci jedné skupiny ARM, aby je bylo snazší spravovat.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-180">You can create all these components under one ARM group to make them easier to manage.</span></span>

![Účet Azure Data Lake Analytics a úložiště](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

<span data-ttu-id="d3fbc-182">Účet Data Lake Analytics a účty závislého úložiště musí být umístěné ve stejném centru dat Azure.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-182">A Data Lake Analytics account and the dependent storage accounts must be placed in the same Azure data center.</span></span>
<span data-ttu-id="d3fbc-183">Skupiny ARM ale může být umístěno v různých datových center.</span><span class="sxs-lookup"><span data-stu-id="d3fbc-183">The ARM group however can be located in a different data center.</span></span>  

## <a name="see-also"></a><span data-ttu-id="d3fbc-184">Viz také</span><span class="sxs-lookup"><span data-stu-id="d3fbc-184">See also</span></span>
* [<span data-ttu-id="d3fbc-185">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d3fbc-185">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="d3fbc-186">Začínáme se službou Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3fbc-186">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="d3fbc-187">Správa Azure Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3fbc-187">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="d3fbc-188">Sledování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3fbc-188">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

