---
title: "aaaBuild vaše první objekt pro vytváření dat (REST) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí rozhraní REST API služby Data Factory ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7e0a2465-2d85-4143-a4bb-42e03c273097
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 5d8e39bd598cca35f91d501bad74e8a8436f8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a><span data-ttu-id="de64c-103">Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí rozhraní REST API služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="de64c-103">Tutorial: Build your first Azure data factory using Data Factory REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="de64c-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="de64c-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="de64c-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="de64c-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="de64c-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de64c-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="de64c-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="de64c-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="de64c-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="de64c-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="de64c-109">REST API</span><span class="sxs-lookup"><span data-stu-id="de64c-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>


<span data-ttu-id="de64c-110">V tomto článku můžete pomocí REST API služby Data Factory toocreate první objekt pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-110">In this article, you use Data Factory REST API toocreate your first Azure data factory.</span></span> <span data-ttu-id="de64c-111">kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="de64c-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="de64c-112">Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="de64c-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="de64c-113">Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="de64c-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="de64c-114">Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="de64c-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span>

> [!NOTE]
> <span data-ttu-id="de64c-115">Tento článek nepopisuje všechny hello REST API.</span><span class="sxs-lookup"><span data-stu-id="de64c-115">This article does not cover all hello REST API.</span></span> <span data-ttu-id="de64c-116">Úplnou dokumentaci o rozhraní REST API najdete v článku [Rozhraní REST API služby Data Factory – referenční informace](/rest/api/datafactory/).</span><span class="sxs-lookup"><span data-stu-id="de64c-116">For comprehensive documentation on REST API, see [Data Factory REST API Reference](/rest/api/datafactory/).</span></span>
> 
> <span data-ttu-id="de64c-117">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="de64c-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="de64c-118">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="de64c-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="de64c-119">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="de64c-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="de64c-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de64c-120">Prerequisites</span></span>
* <span data-ttu-id="de64c-121">Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="de64c-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="de64c-122">Nainstalujte na svůj počítač nástroj [Curl](https://curl.haxx.se/dlwiz/).</span><span class="sxs-lookup"><span data-stu-id="de64c-122">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="de64c-123">Použijete nástroj CURL hello s toocreate příkazů REST služby data factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-123">You use hello CURL tool with REST commands toocreate a data factory.</span></span>
* <span data-ttu-id="de64c-124">Postupujte podle pokynů v [tomto článku](../azure-resource-manager/resource-group-create-service-principal-portal.md) a proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="de64c-124">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span>
  1. <span data-ttu-id="de64c-125">V Azure Active Directory vytvořte webovou aplikaci s názvem **ADFGetStartedApp**.</span><span class="sxs-lookup"><span data-stu-id="de64c-125">Create a Web application named **ADFGetStartedApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="de64c-126">Získejte **ID klienta** a **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="de64c-126">Get **client ID** and **secret key**.</span></span>
  3. <span data-ttu-id="de64c-127">Získejte **ID tenanta**.</span><span class="sxs-lookup"><span data-stu-id="de64c-127">Get **tenant ID**.</span></span>
  4. <span data-ttu-id="de64c-128">Přiřadit hello **ADFGetStartedApp** aplikace toohello **Data Factory Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="de64c-128">Assign hello **ADFGetStartedApp** application toohello **Data Factory Contributor** role.</span></span>
* <span data-ttu-id="de64c-129">Nainstalujte [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="de64c-129">Install [Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="de64c-130">Spusťte **prostředí PowerShell** a hello spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="de64c-130">Launch **PowerShell** and run hello following command.</span></span> <span data-ttu-id="de64c-131">Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="de64c-131">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="de64c-132">Pokud zavřete a znovu otevřete, toorun hello příkazů je třeba znovu.</span><span class="sxs-lookup"><span data-stu-id="de64c-132">If you close and reopen, you need toorun hello commands again.</span></span>
  1. <span data-ttu-id="de64c-133">Spustit **Login-AzureRmAccount** a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-133">Run **Login-AzureRmAccount** and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
  2. <span data-ttu-id="de64c-134">Spustit **Get-AzureRmSubscription** tooview všechny hello předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="de64c-134">Run **Get-AzureRmSubscription** tooview all hello subscriptions for this account.</span></span>
  3. <span data-ttu-id="de64c-135">Spustit **Get-AzureRmSubscription - Název_předplatného NameOfAzureSubscription | Set-AzureRmContext** tooselect hello předplatné, které chcete toowork s.</span><span class="sxs-lookup"><span data-stu-id="de64c-135">Run **Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="de64c-136">Nahraďte **NameOfAzureSubscription** s názvem hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-136">Replace **NameOfAzureSubscription** with hello name of your Azure subscription.</span></span>
* <span data-ttu-id="de64c-137">Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="de64c-137">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

   <span data-ttu-id="de64c-138">Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="de64c-138">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="de64c-139">Pokud používáte jiné skupině prostředků, musíte v tomto kurzu toouse hello název vaší skupiny prostředků místo skupiny ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="de64c-139">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="de64c-140">Vytvoření definic JSON</span><span class="sxs-lookup"><span data-stu-id="de64c-140">Create JSON definitions</span></span>
<span data-ttu-id="de64c-141">Vytvořte následující soubory JSON ve složce hello, kde se nachází curl.exe.</span><span class="sxs-lookup"><span data-stu-id="de64c-141">Create following JSON files in hello folder where curl.exe is located.</span></span>

### <a name="datafactoryjson"></a><span data-ttu-id="de64c-142">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="de64c-142">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="de64c-143">Název musí být globálně jedinečné, proto je vhodné toomake ADFCopyTutorialDF tooprefix či přípon je jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="de64c-143">Name must be globally unique, so you may want tooprefix/suffix ADFCopyTutorialDF toomake it a unique name.</span></span>
>
>

```JSON
{
    "name": "FirstDataFactoryREST",
    "location": "WestUS"
}
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="de64c-144">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="de64c-144">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="de64c-145">Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="de64c-145">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="de64c-146">toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="de64c-146">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
>
>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="hdinsightondemandlinkedservicejson"></a><span data-ttu-id="de64c-147">hdinsightondemandlinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="de64c-147">hdinsightondemandlinkedservice.json</span></span>

```JSON
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

<span data-ttu-id="de64c-148">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="de64c-148">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="de64c-149">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="de64c-149">Property</span></span> | <span data-ttu-id="de64c-150">Popis</span><span class="sxs-lookup"><span data-stu-id="de64c-150">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="de64c-151">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="de64c-151">ClusterSize</span></span> |<span data-ttu-id="de64c-152">Velikost clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-152">Size of hello HDInsight cluster.</span></span> |
| <span data-ttu-id="de64c-153">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="de64c-153">TimeToLive</span></span> |<span data-ttu-id="de64c-154">Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní.</span><span class="sxs-lookup"><span data-stu-id="de64c-154">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
| <span data-ttu-id="de64c-155">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="de64c-155">linkedServiceName</span></span> |<span data-ttu-id="de64c-156">Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem HDInsight</span><span class="sxs-lookup"><span data-stu-id="de64c-156">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight</span></span> |

<span data-ttu-id="de64c-157">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="de64c-157">Note hello following points:</span></span>

* <span data-ttu-id="de64c-158">Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello výše JSON.</span><span class="sxs-lookup"><span data-stu-id="de64c-158">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello above JSON.</span></span> <span data-ttu-id="de64c-159">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="de64c-159">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
* <span data-ttu-id="de64c-160">Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="de64c-160">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="de64c-161">Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).</span><span class="sxs-lookup"><span data-stu-id="de64c-161">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="de64c-162">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="de64c-162">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="de64c-163">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-163">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="de64c-164">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="de64c-164">This behavior is by design.</span></span> <span data-ttu-id="de64c-165">S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez je zpracovat, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="de64c-165">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>

    <span data-ttu-id="de64c-166">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="de64c-166">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="de64c-167">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="de64c-167">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="de64c-168">vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času".</span><span class="sxs-lookup"><span data-stu-id="de64c-168">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="de64c-169">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="de64c-169">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

<span data-ttu-id="de64c-170">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="de64c-170">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="de64c-171">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="de64c-171">inputdataset.json</span></span>

```JSON
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

<span data-ttu-id="de64c-172">Hello JSON Určuje datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-172">hello JSON defines a dataset named **AzureBlobInput**, which represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="de64c-173">Kromě toho určuje, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="de64c-173">In addition, it specifies that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

<span data-ttu-id="de64c-174">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="de64c-174">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="de64c-175">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="de64c-175">Property</span></span> | <span data-ttu-id="de64c-176">Popis</span><span class="sxs-lookup"><span data-stu-id="de64c-176">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="de64c-177">type</span><span class="sxs-lookup"><span data-stu-id="de64c-177">type</span></span> |<span data-ttu-id="de64c-178">Hello vlastnost Typ nastavena tooAzureBlob, protože data se nachází v úložišti objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-178">hello type property is set tooAzureBlob because data resides in Azure blob storage.</span></span> |
| <span data-ttu-id="de64c-179">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="de64c-179">linkedServiceName</span></span> |<span data-ttu-id="de64c-180">odkazuje toohello StorageLinkedService, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="de64c-180">refers toohello StorageLinkedService you created earlier.</span></span> |
| <span data-ttu-id="de64c-181">fileName</span><span class="sxs-lookup"><span data-stu-id="de64c-181">fileName</span></span> |<span data-ttu-id="de64c-182">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="de64c-182">This property is optional.</span></span> <span data-ttu-id="de64c-183">Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="de64c-183">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="de64c-184">V takovém případě pouze hello soubor Input.log.</span><span class="sxs-lookup"><span data-stu-id="de64c-184">In this case, only hello input.log is processed.</span></span> |
| <span data-ttu-id="de64c-185">type</span><span class="sxs-lookup"><span data-stu-id="de64c-185">type</span></span> |<span data-ttu-id="de64c-186">soubory protokolu Hello jsou v textovém formátu, takže používáme TextFormat.</span><span class="sxs-lookup"><span data-stu-id="de64c-186">hello log files are in text format, so we use TextFormat.</span></span> |
| <span data-ttu-id="de64c-187">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="de64c-187">columnDelimiter</span></span> |<span data-ttu-id="de64c-188">sloupce v souborech protokolů hello oddělené čárkou ()</span><span class="sxs-lookup"><span data-stu-id="de64c-188">columns in hello log files are delimited by a comma character (,)</span></span> |
| <span data-ttu-id="de64c-189">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="de64c-189">frequency/interval</span></span> |<span data-ttu-id="de64c-190">frekvence nastavená tooMonth a interval je 1, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="de64c-190">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span> |
| <span data-ttu-id="de64c-191">external</span><span class="sxs-lookup"><span data-stu-id="de64c-191">external</span></span> |<span data-ttu-id="de64c-192">Tato vlastnost nastavena tootrue, pokud hello vstupní data nevygenerovala služba Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-192">this property is set tootrue if hello input data is not generated by hello Data Factory service.</span></span> |

### <a name="outputdatasetjson"></a><span data-ttu-id="de64c-193">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="de64c-193">outputdataset.json</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="de64c-194">Hello JSON Určuje datovou sadu s názvem **AzureBlobOutput**, která představuje výstupní data pro aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-194">hello JSON defines a dataset named **AzureBlobOutput**, which represents output data for an activity in hello pipeline.</span></span> <span data-ttu-id="de64c-195">Kromě toho určuje, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="de64c-195">In addition, it specifies that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="de64c-196">Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="de64c-196">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="de64c-197">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="de64c-197">pipeline.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="de64c-198">Nahraďte **storageaccountname** názvem vašeho účtu služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="de64c-198">Replace **storageaccountname** with name of your Azure storage account.</span></span>
>
>

```JSON
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                "scriptLinkedService": "AzureStorageLinkedService",
                "defines": {
                    "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                    "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                }
            },
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "policy": {
                "concurrency": 1,
                "retry": 3
            },
            "scheduler": {
                "frequency": "Month",
                "interval": 1
            },
            "name": "RunSampleHiveActivity",
            "linkedServiceName": "HDInsightOnDemandLinkedService"
        }],
        "start": "2017-07-10T00:00:00Z",
        "end": "2017-07-11T00:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="de64c-199">V hello fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, která používá Hive tooprocess data v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="de64c-199">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess data on a HDInsight cluster.</span></span>

<span data-ttu-id="de64c-200">soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **StorageLinkedService**) a v **skriptu**  složky v kontejneru hello **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="de64c-200">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **StorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

<span data-ttu-id="de64c-201">Hello **definuje** část určuje nastavení běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např. ${hiveconf: inputtable}, ${určuje}).</span><span class="sxs-lookup"><span data-stu-id="de64c-201">hello **defines** section specifies runtime settings that are passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

<span data-ttu-id="de64c-202">Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-202">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

<span data-ttu-id="de64c-203">V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="de64c-203">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

> [!NOTE]
> <span data-ttu-id="de64c-204">V části "JSON kanálu" v [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) podrobnosti o vlastnostech JSON použitých v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-204">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in hello preceding example.</span></span>
>
>

## <a name="set-global-variables"></a><span data-ttu-id="de64c-205">Nastavení globálních proměnných</span><span class="sxs-lookup"><span data-stu-id="de64c-205">Set global variables</span></span>
<span data-ttu-id="de64c-206">V prostředí Azure PowerShell spusťte následující příkazy po nahrazení hello hodnoty vlastními hello:</span><span class="sxs-lookup"><span data-stu-id="de64c-206">In Azure PowerShell, execute hello following commands after replacing hello values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="de64c-207">Postup získání ID klienta, tajného klíče klienta, ID tenanta a ID předplatného naleznete v sekci [Požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="de64c-207">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>
>
>

```PowerShell
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "FirstDataFactoryREST"
```


## <a name="authenticate-with-aad"></a><span data-ttu-id="de64c-208">Ověření pomocí ADD</span><span class="sxs-lookup"><span data-stu-id="de64c-208">Authenticate with AAD</span></span>

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken)
```


## <a name="create-data-factory"></a><span data-ttu-id="de64c-209">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="de64c-209">Create data factory</span></span>
<span data-ttu-id="de64c-210">V tomto kroku vytvoříte službu Azure Data Factory s názvem **FirstDataFactoryREST**.</span><span class="sxs-lookup"><span data-stu-id="de64c-210">In this step, you create an Azure Data Factory named **FirstDataFactoryREST**.</span></span> <span data-ttu-id="de64c-211">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="de64c-211">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="de64c-212">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="de64c-212">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="de64c-213">Například aktivitu kopírování toocopy dat z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive dat tootransform skriptu Hive.</span><span class="sxs-lookup"><span data-stu-id="de64c-213">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform data.</span></span> <span data-ttu-id="de64c-214">Spusťte následující příkazy toocreate hello objekt pro vytváření dat hello:</span><span class="sxs-lookup"><span data-stu-id="de64c-214">Run hello following commands toocreate hello data factory:</span></span>

1. <span data-ttu-id="de64c-215">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="de64c-215">Assign hello command toovariable named **cmd**.</span></span>

    <span data-ttu-id="de64c-216">Potvrďte, že hello název objektu pro vytváření dat hello zde určíte název zadaný v hello hello (ADFCopyTutorialDF) odpovídá **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="de64c-216">Confirm that hello name of hello data factory you specify here (ADFCopyTutorialDF) matches hello name specified in hello **datafactory.json**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
    ```
2. <span data-ttu-id="de64c-217">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="de64c-217">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="de64c-218">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-218">View hello results.</span></span> <span data-ttu-id="de64c-219">Pokud objekt pro vytváření dat hello byl úspěšně vytvořen, zobrazí hello JSON pro vytváření dat hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="de64c-219">If hello data factory has been successfully created, you see hello JSON for hello data factory in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

<span data-ttu-id="de64c-220">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="de64c-220">Note hello following points:</span></span>

* <span data-ttu-id="de64c-221">Název Hello hello objekt pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="de64c-221">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="de64c-222">Pokud se zobrazí chyba hello ve výsledcích: **název objektu pro vytváření dat "FirstDataFactoryREST" není k dispozici**, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de64c-222">If you see hello error in results: **Data factory name “FirstDataFactoryREST” is not available**, do hello following steps:</span></span>
  1. <span data-ttu-id="de64c-223">Změna hello název (například yournameFirstDataFactoryREST) v hello **datafactory.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="de64c-223">Change hello name (for example, yournameFirstDataFactoryREST) in hello **datafactory.json** file.</span></span> <span data-ttu-id="de64c-224">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-224">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
  2. <span data-ttu-id="de64c-225">V první příkaz hello kde hello **$cmd** přiřazena hodnota proměnné, nahraďte FirstDataFactoryREST hello nový název a spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-225">In hello first command where hello **$cmd** variable is assigned a value, replace FirstDataFactoryREST with hello new name and run hello command.</span></span>
  3. <span data-ttu-id="de64c-226">Spusťte hello následující dva příkazy tooinvoke hello REST API toocreate hello data factory a tisk hello výsledky operace hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-226">Run hello next two commands tooinvoke hello REST API toocreate hello data factory and print hello results of hello operation.</span></span>
* <span data-ttu-id="de64c-227">instance služby Data Factory toocreate, je nutné toobe přispěvatelem/správcem předplatného Azure hello</span><span class="sxs-lookup"><span data-stu-id="de64c-227">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="de64c-228">Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="de64c-228">hello name of hello data factory may be registered as a DNS name in hello future and hence become publicly visible.</span></span>
* <span data-ttu-id="de64c-229">Pokud se zobrazí chyba hello: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**", proveďte jednu z následujících hello a zkuste to znovu publikovat:</span><span class="sxs-lookup"><span data-stu-id="de64c-229">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span>

  * <span data-ttu-id="de64c-230">V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello:</span><span class="sxs-lookup"><span data-stu-id="de64c-230">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

      <span data-ttu-id="de64c-231">Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="de64c-231">You can run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="de64c-232">Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-232">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="de64c-233">Tato akce automaticky registruje zprostředkovatele hello za vás.</span><span class="sxs-lookup"><span data-stu-id="de64c-233">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="de64c-234">Před vytvořením kanálu, je nutné toocreate několik entit služby Data Factory nejdřív.</span><span class="sxs-lookup"><span data-stu-id="de64c-234">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="de64c-235">Nejdřív vytvoříte propojené služby toolink dat úložiště nebo výpočtů tooyour data ukládat, definujete vstupní a výstupní datové sady toorepresent data v propojených úložištích dat.</span><span class="sxs-lookup"><span data-stu-id="de64c-235">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent data in linked data stores.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="de64c-236">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="de64c-236">Create linked services</span></span>
<span data-ttu-id="de64c-237">V tomto kroku propojíte účtu úložiště Azure a služby na vyžádání Azure HDInsight clusteru tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-237">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="de64c-238">blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="de64c-238">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="de64c-239">Hello propojené služby HDInsight je použité toorun skriptu Hive určeného v hello aktivitu hello kanál v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="de64c-239">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span>

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="de64c-240">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="de64c-240">Create Azure Storage linked service</span></span>
<span data-ttu-id="de64c-241">V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-241">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="de64c-242">V tomto kurzu použijete hello stejný účet úložiště Azure toostore vstupní a výstupní data a hello HQL soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="de64c-242">With this tutorial, you use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="de64c-243">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="de64c-243">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="de64c-244">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="de64c-244">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="de64c-245">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-245">View hello results.</span></span> <span data-ttu-id="de64c-246">Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="de64c-246">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="de64c-247">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="de64c-247">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="de64c-248">V tomto kroku propojíte služby na vyžádání HDInsight clusteru tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-248">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="de64c-249">Hello HDInsight cluster je automaticky vytvoří za běhu a až dokončí zpracování a nečinnosti hello zadaného množství času se odstraní.</span><span class="sxs-lookup"><span data-stu-id="de64c-249">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span> <span data-ttu-id="de64c-250">Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="de64c-250">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="de64c-251">Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="de64c-251">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>

1. <span data-ttu-id="de64c-252">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="de64c-252">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
    ```
2. <span data-ttu-id="de64c-253">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="de64c-253">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="de64c-254">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-254">View hello results.</span></span> <span data-ttu-id="de64c-255">Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="de64c-255">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="de64c-256">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="de64c-256">Create datasets</span></span>
<span data-ttu-id="de64c-257">V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive.</span><span class="sxs-lookup"><span data-stu-id="de64c-257">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="de64c-258">Tyto datové sady odkazují toohello **StorageLinkedService** jste vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="de64c-258">These datasets refer toohello **StorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="de64c-259">Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="de64c-259">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>

### <a name="create-input-dataset"></a><span data-ttu-id="de64c-260">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="de64c-260">Create input dataset</span></span>
<span data-ttu-id="de64c-261">V tomto kroku vytvoříte hello vstupní datové sady toorepresent vstupní data uložená v úložišti objektů Blob v Azure hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-261">In this step, you create hello input dataset toorepresent input data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="de64c-262">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="de64c-262">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="de64c-263">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="de64c-263">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="de64c-264">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-264">View hello results.</span></span> <span data-ttu-id="de64c-265">Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="de64c-265">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="de64c-266">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="de64c-266">Create output dataset</span></span>
<span data-ttu-id="de64c-267">V tomto kroku vytvoříte hello výstupní datovou sadu toorepresent výstupní data ve hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-267">In this step, you create hello output dataset toorepresent output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="de64c-268">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="de64c-268">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="de64c-269">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="de64c-269">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="de64c-270">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-270">View hello results.</span></span> <span data-ttu-id="de64c-271">Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="de64c-271">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-pipeline"></a><span data-ttu-id="de64c-272">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="de64c-272">Create pipeline</span></span>
<span data-ttu-id="de64c-273">V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="de64c-273">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="de64c-274">Vstupní řez je dostupný jednou měsíčně (frekvence: Month, interval: 1), výstupní řez se vytváří jednou měsíčně a vlastnost scheduler hello aktivity hello nastavena také toomonthly.</span><span class="sxs-lookup"><span data-stu-id="de64c-274">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="de64c-275">nastavení Hello hello výstupní datovou sadu a hello aktivitu Plánovač se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="de64c-275">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="de64c-276">Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="de64c-276">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="de64c-277">Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-277">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span>

<span data-ttu-id="de64c-278">Zkontrolujte, jestli hello **input.log** souboru v hello **adfgetstarted/inputdata** složky v hello úložiště objektů blob v Azure a spusťte hello následující příkaz toodeploy hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="de64c-278">Confirm that you see hello **input.log** file in hello **adfgetstarted/inputdata** folder in hello Azure blob storage, and run hello following command toodeploy hello pipeline.</span></span> <span data-ttu-id="de64c-279">Od hello **spustit** a **end** jsou nastavené v posledních hello a **isPaused** je sada toofalse, hello kanál (aktivita v kanálu hello) spustí hned po nasazení.</span><span class="sxs-lookup"><span data-stu-id="de64c-279">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>

1. <span data-ttu-id="de64c-280">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="de64c-280">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="de64c-281">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="de64c-281">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="de64c-282">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-282">View hello results.</span></span> <span data-ttu-id="de64c-283">Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="de64c-283">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```
4. <span data-ttu-id="de64c-284">Úspěšně jste vytvořili první kanál pomocí prostředí Azure PowerShell, blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="de64c-284">Congratulations, you have successfully created your first pipeline using Azure PowerShell!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="de64c-285">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="de64c-285">Monitor pipeline</span></span>
<span data-ttu-id="de64c-286">V tomto kroku použijete řezy toomonitor REST API služby Data Factory vytvářen hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="de64c-286">In this step, you use Data Factory REST API toomonitor slices being produced by hello pipeline.</span></span>

```PowerShell
$ds ="AzureBlobOutput"

$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

$results2 = Invoke-Command -scriptblock $cmd;

IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

> [!IMPORTANT]
> <span data-ttu-id="de64c-287">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="de64c-287">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="de64c-288">Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="de64c-288">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
>
>

<span data-ttu-id="de64c-289">Spustit hello Invoke-Command a hello dalšímu, dokud neuvidíte hello řez ve **připraven** stavu nebo **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="de64c-289">Run hello Invoke-Command and hello next one until you see hello slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="de64c-290">Když hello řez ve stavu Připraveno, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="de64c-290">When hello slice is in Ready state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  <span data-ttu-id="de64c-291">Hello vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="de64c-291">hello creation of an on-demand HDInsight cluster usually takes some time.</span></span>

![Výstupní data](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [!IMPORTANT]
> <span data-ttu-id="de64c-293">vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="de64c-293">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="de64c-294">Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="de64c-294">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

<span data-ttu-id="de64c-295">Můžete také použít Azure portálu toomonitor řezy a vyřešte všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="de64c-295">You can also use Azure portal toomonitor slices and troubleshoot any issues.</span></span> <span data-ttu-id="de64c-296">Přečtěte si podrobnosti o [monitorování kanálů pomocí webu Azure Portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline).</span><span class="sxs-lookup"><span data-stu-id="de64c-296">See [Monitor pipelines using Azure portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) details.</span></span>

## <a name="summary"></a><span data-ttu-id="de64c-297">Souhrn</span><span class="sxs-lookup"><span data-stu-id="de64c-297">Summary</span></span>
<span data-ttu-id="de64c-298">V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop.</span><span class="sxs-lookup"><span data-stu-id="de64c-298">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="de64c-299">Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de64c-299">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>

1. <span data-ttu-id="de64c-300">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="de64c-300">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="de64c-301">Vytvořili jste dvě **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="de64c-301">Created two **linked services**:</span></span>
   1. <span data-ttu-id="de64c-302">**Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-302">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="de64c-303">**Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-303">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="de64c-304">Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="de64c-304">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="de64c-305">Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="de64c-305">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="de64c-306">Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="de64c-306">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de64c-307">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de64c-307">Next steps</span></span>
<span data-ttu-id="de64c-308">V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru Azure HDInsight na vyžádání spouští skript Hive.</span><span class="sxs-lookup"><span data-stu-id="de64c-308">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="de64c-309">jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů Blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="de64c-309">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="de64c-310">Viz také</span><span class="sxs-lookup"><span data-stu-id="de64c-310">See Also</span></span>
| <span data-ttu-id="de64c-311">Téma</span><span class="sxs-lookup"><span data-stu-id="de64c-311">Topic</span></span> | <span data-ttu-id="de64c-312">Popis</span><span class="sxs-lookup"><span data-stu-id="de64c-312">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="de64c-313">Rozhraní REST API služby Data Factory – referenční informace</span><span class="sxs-lookup"><span data-stu-id="de64c-313">Data Factory REST API Reference</span></span>](/rest/api/datafactory/) |<span data-ttu-id="de64c-314">Tady najdete rozsáhlou dokumentaci o rutinách služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-314">See comprehensive documentation on Data Factory cmdlets</span></span> |
| [<span data-ttu-id="de64c-315">Kanály</span><span class="sxs-lookup"><span data-stu-id="de64c-315">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="de64c-316">Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku.</span><span class="sxs-lookup"><span data-stu-id="de64c-316">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="de64c-317">Datové sady</span><span class="sxs-lookup"><span data-stu-id="de64c-317">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="de64c-318">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-318">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="de64c-319">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="de64c-319">Scheduling and Execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="de64c-320">Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="de64c-320">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="de64c-321">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="de64c-321">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="de64c-322">Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="de64c-322">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
