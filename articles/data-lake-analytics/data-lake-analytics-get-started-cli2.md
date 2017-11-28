---
title: "aaaGet začít s Azure Data Lake Analytics pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak toouse hello 2.0 rozhraní příkazového řádku Azure toocreate účet Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy hello. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: c4e91c0d3526e4932c2948c0a326d4cedc985791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a><span data-ttu-id="019d9-103">Začínáme s Azure Data Lake Analytics s využitím rozhraní Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="019d9-103">Get started with Azure Data Lake Analytics using Azure CLI 2.0</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="019d9-104">V tomto kurzu vyvinete úlohu, která načte soubor hodnot oddělených tabulátory (TSV) a převede ho na soubor hodnot oddělených čárkami (CSV).</span><span class="sxs-lookup"><span data-stu-id="019d9-104">In this tutorial, you develop a job that reads a tab separated values (TSV) file and converts it into a comma-separated values (CSV) file.</span></span> <span data-ttu-id="019d9-105">toogo prostřednictvím hello stejný kurz pomocí jiné podporované nástroje použijte hello rozevíracího seznamu nahoře hello této části.</span><span class="sxs-lookup"><span data-stu-id="019d9-105">toogo through hello same tutorial using other supported tools, use hello dropdown list on hello top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="019d9-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="019d9-106">Prerequisites</span></span>
<span data-ttu-id="019d9-107">Než začnete tento kurz, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="019d9-107">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="019d9-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="019d9-108">**An Azure subscription**.</span></span> <span data-ttu-id="019d9-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="019d9-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="019d9-110">**Azure CLI 2.0**.</span><span class="sxs-lookup"><span data-stu-id="019d9-110">**Azure CLI 2.0**.</span></span> <span data-ttu-id="019d9-111">Viz téma [Instalace a konfigurace rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="019d9-111">See [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="019d9-112">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="019d9-112">Log in tooAzure</span></span>

<span data-ttu-id="019d9-113">toolog v tooyour předplatné Azure:</span><span class="sxs-lookup"><span data-stu-id="019d9-113">toolog in tooyour Azure subscription:</span></span>

```
azurecli
az login
```

<span data-ttu-id="019d9-114">Jsou tooa toobrowse požadovanou adresu URL a zadání ověřovacího kódu.</span><span class="sxs-lookup"><span data-stu-id="019d9-114">You are requested toobrowse tooa URL, and enter an authentication code.</span></span>  <span data-ttu-id="019d9-115">A pak postupujte podle pokynů tooenter hello přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="019d9-115">And then follow hello instructions tooenter your credentials.</span></span>

<span data-ttu-id="019d9-116">Po přihlášení hello přihlášení příkaz vypíše vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="019d9-116">Once you have logged in, hello login command lists your subscriptions.</span></span>

<span data-ttu-id="019d9-117">toouse konkrétní předplatné:</span><span class="sxs-lookup"><span data-stu-id="019d9-117">toouse a specific subscription:</span></span>

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="019d9-118">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="019d9-118">Create Data Lake Analytics account</span></span>
<span data-ttu-id="019d9-119">Je nutné, abyste před spuštěním jakékoli úlohy měli účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="019d9-119">You need a Data Lake Analytics account before you can run any jobs.</span></span> <span data-ttu-id="019d9-120">toocreate účtu Data Lake Analytics, je nutné zadat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="019d9-120">toocreate a Data Lake Analytics account, you must specify hello following items:</span></span>

* <span data-ttu-id="019d9-121">**Skupina prostředků Azure**.</span><span class="sxs-lookup"><span data-stu-id="019d9-121">**Azure Resource Group**.</span></span> <span data-ttu-id="019d9-122">Účet Data Lake Analytics se musí vytvořit v rámci Skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="019d9-122">A Data Lake Analytics account must be created within an Azure Resource group.</span></span> <span data-ttu-id="019d9-123">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) vám umožní toowork s hello prostředky v aplikaci jako se skupinou.</span><span class="sxs-lookup"><span data-stu-id="019d9-123">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) enables you toowork with hello resources in your application as a group.</span></span> <span data-ttu-id="019d9-124">Můžete nasadit, aktualizovat nebo odstranit všechny prostředky hello pro vaši aplikaci v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="019d9-124">You can deploy, update, or delete all of hello resources for your application in a single, coordinated operation.</span></span>  

<span data-ttu-id="019d9-125">toolist hello existující skupiny prostředků v rámci svého předplatného:</span><span class="sxs-lookup"><span data-stu-id="019d9-125">toolist hello existing resource groups under your subscription:</span></span>

```
az group list
```

<span data-ttu-id="019d9-126">toocreate novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="019d9-126">toocreate a new resource group:</span></span>

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* <span data-ttu-id="019d9-127">**Název účtu Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="019d9-127">**Data Lake Analytics account name**.</span></span> <span data-ttu-id="019d9-128">Každý účtu Data Lake Analytics má název.</span><span class="sxs-lookup"><span data-stu-id="019d9-128">Each Data Lake Analytics account has a name.</span></span>
* <span data-ttu-id="019d9-129">**Umístění**.</span><span class="sxs-lookup"><span data-stu-id="019d9-129">**Location**.</span></span> <span data-ttu-id="019d9-130">Použijte jeden z hello datových center Azure podporující Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="019d9-130">Use one of hello Azure data centers that supports Data Lake Analytics.</span></span>
* <span data-ttu-id="019d9-131">**Výchozí účet Data Lake Store:** Každý účet Data Lake Analytics má výchozí účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="019d9-131">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span>

<span data-ttu-id="019d9-132">toolist hello existující účet Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="019d9-132">toolist hello existing Data Lake Store account:</span></span>

```
az dls account list
```

<span data-ttu-id="019d9-133">toocreate nový účet Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="019d9-133">toocreate a new Data Lake Store account:</span></span>

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

<span data-ttu-id="019d9-134">Použijte následující syntaxi toocreate účtu Data Lake Analytics hello:</span><span class="sxs-lookup"><span data-stu-id="019d9-134">Use hello following syntax toocreate a Data Lake Analytics account:</span></span>

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

<span data-ttu-id="019d9-135">Po vytvoření účtu, můžete použít následující příkazy toolist hello účty hello a zobrazit podrobnosti o účtu:</span><span class="sxs-lookup"><span data-stu-id="019d9-135">After creating an account, you can use hello following commands toolist hello accounts and show account details:</span></span>

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-toodata-lake-store"></a><span data-ttu-id="019d9-136">Nahrání dat tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="019d9-136">Upload data tooData Lake Store</span></span>
<span data-ttu-id="019d9-137">V tomto kurzu zpracujete několik protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="019d9-137">In this tutorial, you process some search logs.</span></span>  <span data-ttu-id="019d9-138">Protokol hledání Hello mohou být uloženy v Data Lake store nebo úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="019d9-138">hello search log can be stored in either Data Lake store or Azure Blob storage.</span></span>

<span data-ttu-id="019d9-139">Hello portál Azure poskytuje uživatelské rozhraní pro kopírování některých ukázkových dat soubory toohello výchozí účtu Data Lake Store, včetně souboru protokolu hledání.</span><span class="sxs-lookup"><span data-stu-id="019d9-139">hello Azure portal provides a user interface for copying some sample data files toohello default Data Lake Store account, which include a search log file.</span></span> <span data-ttu-id="019d9-140">V tématu [Příprava zdrojových dat](data-lake-analytics-get-started-portal.md) tooupload hello data toohello výchozí účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="019d9-140">See [Prepare source data](data-lake-analytics-get-started-portal.md) tooupload hello data toohello default Data Lake Store account.</span></span>

<span data-ttu-id="019d9-141">tooupload soubory pomocí rozhraní příkazového řádku 2.0, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="019d9-141">tooupload files using CLI 2.0, use hello following commands:</span></span>

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

<span data-ttu-id="019d9-142">Data Lake Analytics má také přístup k úložišti objektů Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="019d9-142">Data Lake Analytics can also access Azure Blob storage.</span></span>  <span data-ttu-id="019d9-143">Odesílání dat tooAzure Blob storage, najdete v části [hello pomocí rozhraní příkazového řádku Azure s Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="019d9-143">For uploading data tooAzure Blob storage, see [Using hello Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>

## <a name="submit-data-lake-analytics-jobs"></a><span data-ttu-id="019d9-144">Odesílání úloh Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="019d9-144">Submit Data Lake Analytics jobs</span></span>
<span data-ttu-id="019d9-145">Hello úloh Data Lake Analytics se píšou v hello jazykem U-SQL.</span><span class="sxs-lookup"><span data-stu-id="019d9-145">hello Data Lake Analytics jobs are written in hello U-SQL language.</span></span> <span data-ttu-id="019d9-146">toolearn Další informace o U-SQL, najdete v části [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md) a [eence jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="019d9-146">toolearn more about U-SQL, see [Get started with U-SQL language](data-lake-analytics-u-sql-get-started.md) and [U-SQL language eence](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>

<span data-ttu-id="019d9-147">**toocreate skriptu úlohy Data Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="019d9-147">**toocreate a Data Lake Analytics job script**</span></span>

<span data-ttu-id="019d9-148">Vytvořte textový soubor s následující skript U-SQL a uložte hello textového souboru tooyour pracovní stanice:</span><span class="sxs-lookup"><span data-stu-id="019d9-148">Create a text file with following U-SQL script, and save hello text file tooyour workstation:</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="019d9-149">Tento skript U-SQL přečte hello zdrojový datový soubor pomocí **Extractors.Tsv()**a poté vytvoří soubor csv pomocí **Outputters.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="019d9-149">This U-SQL script reads hello source data file using **Extractors.Tsv()**, and then creates a csv file using **Outputters.Csv()**.</span></span>

<span data-ttu-id="019d9-150">Neupravujte dvě cesty hello, pokud zkopírujete hello zdrojového souboru do jiného umístění.</span><span class="sxs-lookup"><span data-stu-id="019d9-150">Don't modify hello two paths unless you copy hello source file into a different location.</span></span>  <span data-ttu-id="019d9-151">Data Lake Analytics vytvoří výstupní složku hello, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="019d9-151">Data Lake Analytics creates hello output folder if it doesn't exist.</span></span>

<span data-ttu-id="019d9-152">Je jednodušší toouse relativní cesty pro soubory uložené v výchozí účty Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="019d9-152">It is simpler toouse relative paths for files stored in default Data Lake Store accounts.</span></span> <span data-ttu-id="019d9-153">Můžete také použít absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="019d9-153">You can also use absolute paths.</span></span>  <span data-ttu-id="019d9-154">Například:</span><span class="sxs-lookup"><span data-stu-id="019d9-154">For example:</span></span>

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

<span data-ttu-id="019d9-155">Je nutné použít absolutní cesty tooaccess souborům v propojených účtech úložiště.</span><span class="sxs-lookup"><span data-stu-id="019d9-155">You must use absolute paths tooaccess files in linked Storage accounts.</span></span>  <span data-ttu-id="019d9-156">Tady je syntax Hello případě souborů uložených v propojeném účtu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="019d9-156">hello syntax for files stored in linked Azure Storage account is:</span></span>

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> <span data-ttu-id="019d9-157">Kontejner Azure Blob s veřejnými objekty blob není podporován.</span><span class="sxs-lookup"><span data-stu-id="019d9-157">Azure Blob container with public blobs are not supported.</span></span>      
> <span data-ttu-id="019d9-158">Kontejner Azure Blob s veřejnými kontejnery není podporován.</span><span class="sxs-lookup"><span data-stu-id="019d9-158">Azure Blob container with public containers are not supported.</span></span>      
>

<span data-ttu-id="019d9-159">**toosubmit úlohy**</span><span class="sxs-lookup"><span data-stu-id="019d9-159">**toosubmit jobs**</span></span>

<span data-ttu-id="019d9-160">Pomocí následující syntaxe toosubmit úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="019d9-160">Use hello following syntax toosubmit a job.</span></span>

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

<span data-ttu-id="019d9-161">Například:</span><span class="sxs-lookup"><span data-stu-id="019d9-161">For example:</span></span>

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

<span data-ttu-id="019d9-162">**toolist úlohy a zobrazit podrobnosti úlohy**</span><span class="sxs-lookup"><span data-stu-id="019d9-162">**toolist jobs and show job details**</span></span>

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

<span data-ttu-id="019d9-163">**toocancel úlohy**</span><span class="sxs-lookup"><span data-stu-id="019d9-163">**toocancel jobs**</span></span>

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a><span data-ttu-id="019d9-164">Načtení výsledků úlohy</span><span class="sxs-lookup"><span data-stu-id="019d9-164">Retrieve job results</span></span>

<span data-ttu-id="019d9-165">Po dokončení úlohy můžete použít následující příkazy toolist hello výstupní soubory hello a stahování souborů hello:</span><span class="sxs-lookup"><span data-stu-id="019d9-165">After a job is completed, you can use hello following commands toolist hello output files, and download hello files:</span></span>

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

<span data-ttu-id="019d9-166">Například:</span><span class="sxs-lookup"><span data-stu-id="019d9-166">For example:</span></span>

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a><span data-ttu-id="019d9-167">Kanály a opakování</span><span class="sxs-lookup"><span data-stu-id="019d9-167">Pipelines and recurrences</span></span>

<span data-ttu-id="019d9-168">**Získání informací o kanálech a opakováních**</span><span class="sxs-lookup"><span data-stu-id="019d9-168">**Get information about pipelines and recurrences**</span></span>

<span data-ttu-id="019d9-169">Použití hello `az dla job pipeline` příkazy toosee hello kanálu informace dříve odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="019d9-169">Use hello `az dla job pipeline` commands toosee hello pipeline information previously submitted jobs.</span></span>

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

<span data-ttu-id="019d9-170">Použití hello `az dla job recurrence` příkazy toosee hello opakování informace pro dříve odeslaný úlohy.</span><span class="sxs-lookup"><span data-stu-id="019d9-170">Use hello `az dla job recurrence` commands toosee hello recurrence information for previously submitted jobs.</span></span>

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a><span data-ttu-id="019d9-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="019d9-171">Next steps</span></span>

* <span data-ttu-id="019d9-172">toosee hello Data Lake Analytics rozhraní příkazového řádku 2.0 referenčním dokumentu, najdete v části [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span><span class="sxs-lookup"><span data-stu-id="019d9-172">toosee hello Data Lake Analytics CLI 2.0 reference document, see [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span></span>
* <span data-ttu-id="019d9-173">toosee hello Data Lake Store rozhraní příkazového řádku 2.0 referenčním dokumentu, najdete v části [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span><span class="sxs-lookup"><span data-stu-id="019d9-173">toosee hello Data Lake Store CLI 2.0 reference document, see [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span></span>
* <span data-ttu-id="019d9-174">toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="019d9-174">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
