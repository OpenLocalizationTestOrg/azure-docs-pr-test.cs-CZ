---
title: "Vyvolání Spark programy z Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak má být vyvolán Spark programy ze služby Azure data factory pomocí činnost MapReduce."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 57894bbdd9208f8c32eb65e29f04e2ae723780ca
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a><span data-ttu-id="eb477-103">Vyvolání Spark programy z kanálů služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="eb477-103">Invoke Spark programs from Azure Data Factory pipelines</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="eb477-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="eb477-104">Hive Activity</span></span>](data-factory-hive-activity.md)
> * [<span data-ttu-id="eb477-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="eb477-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="eb477-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="eb477-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="eb477-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="eb477-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="eb477-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="eb477-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="eb477-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="eb477-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="eb477-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="eb477-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="eb477-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="eb477-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="eb477-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="eb477-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="eb477-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="eb477-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="eb477-114">Úvod</span><span class="sxs-lookup"><span data-stu-id="eb477-114">Introduction</span></span>
<span data-ttu-id="eb477-115">Aktivita Spark je jedním z [aktivit transformace dat](data-factory-data-transformation-activities.md) podporovaných službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eb477-115">Spark Activity is one of the [data transformation activities](data-factory-data-transformation-activities.md) supported by Azure Data Factory.</span></span> <span data-ttu-id="eb477-116">Tato aktivita běží zadaný program Spark na cluster Apache Spark v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eb477-116">This activity runs the specified Spark program on your Apache Spark cluster in Azure HDInsight.</span></span>    

> [!IMPORTANT]
> - <span data-ttu-id="eb477-117">Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.</span><span class="sxs-lookup"><span data-stu-id="eb477-117">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
> - <span data-ttu-id="eb477-118">Aktivita Spark podporuje pouze existující (vlastní) clustery HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-118">Spark Activity supports only existing (your own) HDInsight Spark clusters.</span></span> <span data-ttu-id="eb477-119">HDInsight propojené služby na vyžádání nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="eb477-119">It does not support an on-demand HDInsight linked service.</span></span>

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a><span data-ttu-id="eb477-120">Návod: vytvoření kanálu s aktivitou Spark</span><span class="sxs-lookup"><span data-stu-id="eb477-120">Walkthrough: create a pipeline with Spark activity</span></span>
<span data-ttu-id="eb477-121">Tady jsou obvyklá kroky k vytvoření objektu pro vytváření dat kanál s aktivitou Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-121">Here are the typical steps to create a Data Factory pipeline with a Spark activity.</span></span>  

1. <span data-ttu-id="eb477-122">Objekt pro vytváření dat vytvořte.</span><span class="sxs-lookup"><span data-stu-id="eb477-122">Create a data factory.</span></span>
2. <span data-ttu-id="eb477-123">Vytvoření služby Azure Storage, propojené k propojení vaší úložiště Azure, který je přidružen k objektu pro vytváření dat cluster HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-123">Create an Azure Storage linked service to link your Azure storage that is associated with your HDInsight Spark cluster to the data factory.</span></span>     
2. <span data-ttu-id="eb477-124">Vytvoření služby Azure HDInsight propojené propojení cluster Apache Spark v Azure HDInsight s data factory.</span><span class="sxs-lookup"><span data-stu-id="eb477-124">Create an Azure HDInsight linked service to link your Apache Spark cluster in Azure HDInsight to the data factory.</span></span>
3. <span data-ttu-id="eb477-125">Vytvořte datovou sadu, která odkazuje propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="eb477-125">Create a dataset that refers to the Azure Storage linked service.</span></span> <span data-ttu-id="eb477-126">V současné době je třeba zadat datovou sadu výstupů pro aktivitu i v případě, že neexistuje žádný výstup se vytváří.</span><span class="sxs-lookup"><span data-stu-id="eb477-126">Currently, you must specify an output dataset for an activity even if there is no output being produced.</span></span>  
4. <span data-ttu-id="eb477-127">Vytvoření kanálu s aktivitou Spark, která odkazuje propojená služba HDInsight vytvořené v #. 2.</span><span class="sxs-lookup"><span data-stu-id="eb477-127">Create a pipeline with Spark activity that refers to the HDInsight linked service created in #2.</span></span> <span data-ttu-id="eb477-128">Aktivita je nakonfigurován s datovou sadou, kterou jste vytvořili v předchozím kroku jako výstupní datové.</span><span class="sxs-lookup"><span data-stu-id="eb477-128">The activity is configured with the dataset you created in the previous step as an output dataset.</span></span> <span data-ttu-id="eb477-129">Výstupní datové sady je určuje plán (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="eb477-129">The output dataset is what drives the schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="eb477-130">Proto je nutné zadat výstupní datovou sadu, i v případě, že aktivita nevytváří skutečně výstup.</span><span class="sxs-lookup"><span data-stu-id="eb477-130">Therefore, you must specify the output dataset even though the activity does not really produce an output.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="eb477-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eb477-131">Prerequisites</span></span>
1. <span data-ttu-id="eb477-132">Vytvořit **účet úložiště pro obecné účely Azure** podle pokynů v návodu: [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="eb477-132">Create a **general-purpose Azure Storage Account** by following instructions in the walkthrough: [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
2. <span data-ttu-id="eb477-133">Vytvořit **cluster Apache Spark v Azure HDInsight** podle pokynů v tomto kurzu: [cluster vytvořit Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="eb477-133">Create an **Apache Spark cluster in Azure HDInsight** by following instructions in the tutorial: [Create Apache Spark cluster in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="eb477-134">Přidružení účtu úložiště Azure, kterou jste vytvořili v kroku #1 s tímto clusterem.</span><span class="sxs-lookup"><span data-stu-id="eb477-134">Associate the Azure storage account you created in step #1 with this cluster.</span></span>  
3. <span data-ttu-id="eb477-135">Stáhnout a revidovat souboru skriptu jazyka python **test.py** nacházející se v: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span><span class="sxs-lookup"><span data-stu-id="eb477-135">Download and review the python script file **test.py** located at: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span></span>  
3.  <span data-ttu-id="eb477-136">Nahrát **test.py** k **pyFiles** složku **adfspark** kontejneru ve službě Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="eb477-136">Upload **test.py** to the **pyFiles** folder in the **adfspark** container in your Azure Blob storage.</span></span> <span data-ttu-id="eb477-137">Vytvořte kontejner, složku, pokud neexistují.</span><span class="sxs-lookup"><span data-stu-id="eb477-137">Create the container and the folder if they do not exist.</span></span>

### <a name="create-data-factory"></a><span data-ttu-id="eb477-138">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="eb477-138">Create data factory</span></span>
<span data-ttu-id="eb477-139">V tomto kroku začneme vytvořením objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eb477-139">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="eb477-140">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eb477-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="eb477-141">V nabídce vlevo klikněte na **NOVÝ**, klikněte na **Data + Analýza** a poté na **Objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="eb477-141">Click **NEW** on the left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="eb477-142">V **nový objekt pro vytváření dat** okno, zadejte **SparkDF** pro název.</span><span class="sxs-lookup"><span data-stu-id="eb477-142">In the **New data factory** blade, enter **SparkDF** for the Name.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="eb477-143">Název objektu pro vytváření dat Azure musí být **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="eb477-143">The name of the Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="eb477-144">Pokud se zobrazí následující chyba: **název objektu pro vytváření dat "SparkDF" není k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="eb477-144">If you see the error: **Data factory name “SparkDF” is not available**.</span></span> <span data-ttu-id="eb477-145">Změňte název objektu pro vytváření dat (například yournameSparkDFdate a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="eb477-145">Change the name of the data factory (for example, yournameSparkDFdate, and try creating again.</span></span> <span data-ttu-id="eb477-146">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eb477-146">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
4. <span data-ttu-id="eb477-147">Vyberte **předplatné Azure**, ve kterém chcete objekt pro vytváření dat vytvořit.</span><span class="sxs-lookup"><span data-stu-id="eb477-147">Select the **Azure subscription** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="eb477-148">Vyberte existující **skupiny prostředků** nebo vytvořte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="eb477-148">Select an existing **resource group** or create an Azure resource group.</span></span>
6. <span data-ttu-id="eb477-149">Vyberte **připnout na řídicí panel** možnost.</span><span class="sxs-lookup"><span data-stu-id="eb477-149">Select **Pin to dashboard** option.</span></span>  
6. <span data-ttu-id="eb477-150">V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eb477-150">Click **Create** on the **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="eb477-151">Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="eb477-151">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
7. <span data-ttu-id="eb477-152">Zobrazí objektu pro vytváření dat vytváří ve **řídicí panel** na portálu Azure, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="eb477-152">You see the data factory being created in the **dashboard** of the Azure portal as follows:</span></span>   
8. <span data-ttu-id="eb477-153">Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eb477-153">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span> <span data-ttu-id="eb477-154">Pokud se stránka objektu pro vytváření dat nezobrazí, klikněte na dlaždici objektu pro vytváření dat na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="eb477-154">If you do not see the data factory page, click the tile for your data factory on the dashboard.</span></span>

    ![Okno Objekt pro vytváření dat](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a><span data-ttu-id="eb477-156">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="eb477-156">Create linked services</span></span>
<span data-ttu-id="eb477-157">V tomto kroku vytvoříte dvě propojené služby, jednu pro váš cluster Spark propojit objekt pro vytváření dat a další propojení úložiště Azure pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="eb477-157">In this step, you create two linked services, one to link your Spark cluster to your data factory, and the other to link your Azure storage to your data factory.</span></span>  

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="eb477-158">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="eb477-158">Create Azure Storage linked service</span></span>
<span data-ttu-id="eb477-159">V tomto kroku propojíte se svým objektem pro vytváření dat svůj účet služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="eb477-159">In this step, you link your Azure Storage account to your data factory.</span></span> <span data-ttu-id="eb477-160">Datové sady, které vytvoříte v kroku později v tomto názorném postupu odkazuje na tato propojená služba.</span><span class="sxs-lookup"><span data-stu-id="eb477-160">A dataset you create in a step later in this walkthrough refers to this linked service.</span></span> <span data-ttu-id="eb477-161">Propojená služba HDInsight, kterou definujete v dalším kroku odkazuje na tato propojená služba příliš.</span><span class="sxs-lookup"><span data-stu-id="eb477-161">The HDInsight linked service that you define in the next step refers to this linked service too.</span></span>  

1. <span data-ttu-id="eb477-162">Klikněte na tlačítko **vytvořit a nasadit** na **Data Factory** okno objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eb477-162">Click **Author and deploy** on the **Data Factory** blade for your data factory.</span></span> <span data-ttu-id="eb477-163">Měli byste vidět editor služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eb477-163">You should see the Data Factory Editor.</span></span>
2. <span data-ttu-id="eb477-164">Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.</span><span class="sxs-lookup"><span data-stu-id="eb477-164">Click **New data store** and choose **Azure storage**.</span></span>

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="eb477-166">Měli byste vidět **skript JSON** pro vytvoření Azure Storage propojená služba v editoru.</span><span class="sxs-lookup"><span data-stu-id="eb477-166">You should see the **JSON script** for creating an Azure Storage linked service in the editor.</span></span>

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="eb477-168">Nahraďte **název účtu** a **klíč účtu** názvem a přístupovým klíčem k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="eb477-168">Replace **account name** and **account key** with the name and access key of your Azure storage account.</span></span> <span data-ttu-id="eb477-169">Chcete-li zjistit, jak získat přístupový klíč k úložišti, přečtěte si informace o zobrazení, kopírování a opětovném vygenerování přístupových klíčů k úložišti v tématu [Správa účtu úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="eb477-169">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="eb477-170">Chcete-li nasadit propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="eb477-170">To deploy the linked service, click **Deploy** on the command bar.</span></span> <span data-ttu-id="eb477-171">Po úspěšném nasazení propojené služby by mělo okno **Koncept-1** zmizet a v zobrazení stromu nalevo se zobrazí služba **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="eb477-171">After the linked service is deployed successfully, the **Draft-1** window should disappear and you see **AzureStorageLinkedService** in the tree view on the left.</span></span>

#### <a name="create-hdinsight-linked-service"></a><span data-ttu-id="eb477-172">Vytvoření propojené služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="eb477-172">Create HDInsight linked service</span></span>
<span data-ttu-id="eb477-173">V tomto kroku vytvoříte propojené služby Azure HDInsight propojit k objektu pro vytváření dat cluster HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-173">In this step, you create Azure HDInsight linked service to link your HDInsight Spark cluster to the data factory.</span></span> <span data-ttu-id="eb477-174">HDInsight cluster se používá ke spuštění programu Spark určeného v aktivitě Spark kanálu v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="eb477-174">The HDInsight cluster is used to run the Spark program specified in the Spark activity of the pipeline in this sample.</span></span>  

1. <span data-ttu-id="eb477-175">Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na panelu nástrojů klikněte na tlačítko **nový výpočet**a potom klikněte na **clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="eb477-175">Click **... More** on the toolbar, click **New compute**, and then click **HDInsight cluster**.</span></span>

    ![Vytvoření propojené služby HDInsight](media/data-factory-spark/new-hdinsight-linked-service.png)
2. <span data-ttu-id="eb477-177">Následující fragment kódu zkopírujte a vložte ho do okna **Koncept-1**.</span><span class="sxs-lookup"><span data-stu-id="eb477-177">Copy and paste the following snippet to the **Draft-1** window.</span></span> <span data-ttu-id="eb477-178">V editoru JSON proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eb477-178">In the JSON editor, do the following steps:</span></span>
    1. <span data-ttu-id="eb477-179">Zadejte **URI** pro cluster HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-179">Specify the **URI** for the HDInsight Spark cluster.</span></span> <span data-ttu-id="eb477-180">Například: `https://<sparkclustername>.azurehdinsight.net/`.</span><span class="sxs-lookup"><span data-stu-id="eb477-180">For example: `https://<sparkclustername>.azurehdinsight.net/`.</span></span>
    2. <span data-ttu-id="eb477-181">Zadejte název **uživatele** kdo má přístup ke clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-181">Specify the name of the **user** who has access to the Spark cluster.</span></span>
    3. <span data-ttu-id="eb477-182">Zadejte **heslo** pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="eb477-182">Specify the **password** for user.</span></span>
    4. <span data-ttu-id="eb477-183">Zadejte **propojená služba Azure Storage** který je přidružen ke clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-183">Specify the **Azure Storage linked service** that is associated with the HDInsight Spark cluster.</span></span> <span data-ttu-id="eb477-184">V tomto příkladu je: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="eb477-184">In this example, it is: **AzureStorageLinkedService**.</span></span>

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - <span data-ttu-id="eb477-185">Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.</span><span class="sxs-lookup"><span data-stu-id="eb477-185">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
    > - <span data-ttu-id="eb477-186">Aktivita Spark podporuje pouze existující (vlastní) clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-186">Spark Activity supports only existing (your own) HDInsight Spark cluster.</span></span> <span data-ttu-id="eb477-187">HDInsight propojené služby na vyžádání nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="eb477-187">It does not support an on-demand HDInsight linked service.</span></span>

    <span data-ttu-id="eb477-188">V tématu [propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) podrobnosti o HDInsight propojené služby.</span><span class="sxs-lookup"><span data-stu-id="eb477-188">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details about the HDInsight linked service.</span></span>
3.  <span data-ttu-id="eb477-189">Chcete-li nasadit propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="eb477-189">To deploy the linked service, click **Deploy** on the command bar.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="eb477-190">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="eb477-190">Create output dataset</span></span>
<span data-ttu-id="eb477-191">Výstupní datové sady je určuje plán (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="eb477-191">The output dataset is what drives the schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="eb477-192">Proto je nutné zadat výstupní datovou sadu aktivity spark v kanálu, i když aktivita skutečně nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eb477-192">Therefore, you must specify an output dataset for the spark activity in the pipeline even though the activity does not really produce any output.</span></span> <span data-ttu-id="eb477-193">Určení vstupní datové sady pro aktivitu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="eb477-193">Specifying an input dataset for the activity is optional.</span></span>

1. <span data-ttu-id="eb477-194">V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nová datová sada** a vyberte **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="eb477-194">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="eb477-195">Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1.</span><span class="sxs-lookup"><span data-stu-id="eb477-195">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="eb477-196">Fragmentu kódu JSON Určuje datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="eb477-196">The JSON snippet defines a dataset called **OutputDataset**.</span></span> <span data-ttu-id="eb477-197">Kromě toho určíte, že se mají výsledky ukládat do kontejneru objektů blob s názvem **adfspark** a ve složce s názvem **pyFiles výstupní**.</span><span class="sxs-lookup"><span data-stu-id="eb477-197">In addition, you specify that the results are stored in the blob container called **adfspark** and the folder called **pyFiles/output**.</span></span> <span data-ttu-id="eb477-198">Jak už bylo zmíněno dříve, je tato datová sada fiktivní datová sada.</span><span class="sxs-lookup"><span data-stu-id="eb477-198">As mentioned earlier, this dataset is a dummy dataset.</span></span> <span data-ttu-id="eb477-199">Program Spark v tomto příkladu nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eb477-199">The Spark program in this example does not produce any output.</span></span> <span data-ttu-id="eb477-200">**Dostupnosti** části určuje, že se výstupní sada vytváří denně.</span><span class="sxs-lookup"><span data-stu-id="eb477-200">The **availability** section specifies that the output dataset is produced daily.</span></span>  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. <span data-ttu-id="eb477-201">Chcete-li nasadit datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="eb477-201">To deploy the dataset, click **Deploy** on the command bar.</span></span>


### <a name="create-pipeline"></a><span data-ttu-id="eb477-202">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="eb477-202">Create pipeline</span></span>
<span data-ttu-id="eb477-203">V tomto kroku vytvoříte kanál s **HDInsightSpark** aktivity.</span><span class="sxs-lookup"><span data-stu-id="eb477-203">In this step, you create a pipeline with a **HDInsightSpark** activity.</span></span> <span data-ttu-id="eb477-204">V současnosti určuje plán výstupní datová sada, takže musíte výstupní datovou sadu vytvořit i v případě, že aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eb477-204">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="eb477-205">Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte.</span><span class="sxs-lookup"><span data-stu-id="eb477-205">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> <span data-ttu-id="eb477-206">Proto žádné vstupní datové sady je zadána v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="eb477-206">Therefore, no input dataset is specified in this example.</span></span>

1. <span data-ttu-id="eb477-207">V **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů a pak klikněte na tlačítko **nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="eb477-207">In the **Data Factory Editor**, click **… More** on the command bar, and then click **New pipeline**.</span></span>
2. <span data-ttu-id="eb477-208">Skript v okna koncept-1 nahraďte následující skript:</span><span class="sxs-lookup"><span data-stu-id="eb477-208">Replace the script in the Draft-1 window with the following script:</span></span>

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    <span data-ttu-id="eb477-209">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="eb477-209">Note the following points:</span></span>
    - <span data-ttu-id="eb477-210">**Typ** je nastavena na **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="eb477-210">The **type** property is set to **HDInsightSpark**.</span></span>
    - <span data-ttu-id="eb477-211">**RootPath** je nastaven na **adfspark\\pyFiles** kde adfspark je kontejner objektů Blob v Azure a pyFiles je dobře složku v daném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eb477-211">The **rootPath** is set to **adfspark\\pyFiles** where adfspark is the Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="eb477-212">V tomto příkladu úložiště objektů Blob Azure je ten, který je přidružen Spark cluster.</span><span class="sxs-lookup"><span data-stu-id="eb477-212">In this example, the Azure Blob Storage is the one that is associated with the Spark cluster.</span></span> <span data-ttu-id="eb477-213">Můžete nahrát soubor do jiného úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="eb477-213">You can upload the file to a different Azure Storage.</span></span> <span data-ttu-id="eb477-214">Pokud tak učiníte, vytvoření služby Azure Storage, propojené propojení objektu pro vytváření dat. Tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="eb477-214">If you do so, create an Azure Storage linked service to link that storage account to the data factory.</span></span> <span data-ttu-id="eb477-215">Potom zadejte název propojené služby, jako hodnotu **sparkJobLinkedService** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="eb477-215">Then, specify the name of the linked service as a value for the **sparkJobLinkedService** property.</span></span> <span data-ttu-id="eb477-216">V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností podporované aktivitou Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-216">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by the Spark Activity.</span></span>  
    - <span data-ttu-id="eb477-217">**EntryFilePath** je nastaven na **test.py**, což je soubor python.</span><span class="sxs-lookup"><span data-stu-id="eb477-217">The **entryFilePath** is set to the **test.py**, which is the python file.</span></span>
    - <span data-ttu-id="eb477-218">**Getdebuginfo –** je nastavena na **vždy**, tzn., soubory protokolů jsou vždy vygeneruje (úspěch nebo neúspěch).</span><span class="sxs-lookup"><span data-stu-id="eb477-218">The **getDebugInfo** property is set to **Always**, which means the log files are always generated (success or failure).</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="eb477-219">Doporučujeme, abyste tuto vlastnost nenastavujte na `Always` v produkčním prostředí Pokud řešíte problém.</span><span class="sxs-lookup"><span data-stu-id="eb477-219">We recommend that you do not set this property to `Always` in a production environment unless you are troubleshooting an issue.</span></span>
    - <span data-ttu-id="eb477-220">**Výstupy** obsahuje jednu výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="eb477-220">The **outputs** section has one output dataset.</span></span> <span data-ttu-id="eb477-221">Je třeba zadat datovou sadu výstupů i v případě, že spark program nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eb477-221">You must specify an output dataset even if the spark program does not produce any output.</span></span> <span data-ttu-id="eb477-222">Výstupní datovou sadu jednotky plán pro kanál (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="eb477-222">The output dataset drives the schedule for the pipeline (hourly, daily, etc.).</span></span>  

        <span data-ttu-id="eb477-223">Podrobnosti o vlastnostech podporovaných aktivitou Spark najdete v tématu [Spark vlastnosti aktivity](#spark-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="eb477-223">For details about the properties supported by Spark activity, see [Spark activity properties](#spark-activity-properties) section.</span></span>
3. <span data-ttu-id="eb477-224">Chcete-li nasadit kanálu, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="eb477-224">To deploy the pipeline, click **Deploy** on the command bar.</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="eb477-225">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="eb477-225">Monitor pipeline</span></span>
1. <span data-ttu-id="eb477-226">Klikněte na tlačítko **X** zavřete okna editoru služby Data Factory a přejděte zpět na domovskou stránku služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eb477-226">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory home page.</span></span> <span data-ttu-id="eb477-227">Klikněte na tlačítko **monitorování a správa** ke spuštění monitorování aplikací na jiné kartě.</span><span class="sxs-lookup"><span data-stu-id="eb477-227">Click **Monitor and Manage** to launch the monitoring application in another tab.</span></span>

    ![Dlaždice monitorování a Správa](media/data-factory-spark/monitor-and-manage-tile.png)
2. <span data-ttu-id="eb477-229">Změna **počáteční čas** filtru v horní části na **2/1/2017**a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="eb477-229">Change the **Start time** filter at the top to **2/1/2017**, and click **Apply**.</span></span>
3. <span data-ttu-id="eb477-230">Pouze jeden interval aktivity byste měli vidět, jako je pouze jeden den mezi počátečním (2017-02-01) a koncový čas (2017-02-02) kanálu.</span><span class="sxs-lookup"><span data-stu-id="eb477-230">You should see only one activity window as there is only one day between the start (2017-02-01) and end times (2017-02-02) of the pipeline.</span></span> <span data-ttu-id="eb477-231">Potvrďte, že datový řez je v **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="eb477-231">Confirm that the data slice is in **ready** state.</span></span>

    ![Monitorování kanálu](media/data-factory-spark/monitor-and-manage-app.png)    
4. <span data-ttu-id="eb477-233">Vyberte **okně aktivita** zobrazíte podrobnosti o aktivity při spuštění.</span><span class="sxs-lookup"><span data-stu-id="eb477-233">Select the **activity window** to see details about the activity run.</span></span> <span data-ttu-id="eb477-234">Pokud dojde k chybě, zobrazí podrobnosti o tom, v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="eb477-234">If there is an error, you see details about it in the right pane.</span></span>

### <a name="verify-the-results"></a><span data-ttu-id="eb477-235">Ověření výsledků</span><span class="sxs-lookup"><span data-stu-id="eb477-235">Verify the results</span></span>

1. <span data-ttu-id="eb477-236">Spusťte **Poznámkový blok Jupyter** pro váš cluster HDInsight Spark přechodem na: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="eb477-236">Launch **Jupyter notebook** for your HDInsight Spark cluster by navigating to: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="eb477-237">Můžete také spustit řídicí panel clusteru pro váš cluster HDInsight Spark a poté spusťte **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="eb477-237">You can also launch cluster dashboard for your HDInsight Spark cluster, and then launch **Jupyter Notebook**.</span></span>
2. <span data-ttu-id="eb477-238">Klikněte na tlačítko **nový** -> **PySpark** spusťte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="eb477-238">Click **New** -> **PySpark** to start a new notebook.</span></span>

    ![Nový poznámkový blok Jupyter](media/data-factory-spark/jupyter-new-book.png)
3. <span data-ttu-id="eb477-240">Spusťte následující příkaz kopírování/vkládání textu a stisknutím klávesy **SHIFT + ENTER** na konci druhý příkaz.</span><span class="sxs-lookup"><span data-stu-id="eb477-240">Run the following command by copy/pasting the text and pressing **SHIFT + ENTER** at the end of the second statement.</span></span>  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. <span data-ttu-id="eb477-241">Zkontrolujte, jestli data z tabulky TVK:</span><span class="sxs-lookup"><span data-stu-id="eb477-241">Confirm that you see the data from the hvac table:</span></span>  

    ![Výsledky dotazu Jupyter](media/data-factory-spark/jupyter-notebook-results.png)

<span data-ttu-id="eb477-243">V tématu [spuštění dotazů Spark SQL](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) části Podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="eb477-243">See [Run a Spark SQL query](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) section for detailed instructions.</span></span> 

### <a name="troubleshooting"></a><span data-ttu-id="eb477-244">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="eb477-244">Troubleshooting</span></span>
<span data-ttu-id="eb477-245">Vzhledem k tomu, že nastavíte **getdebuginfo –** k **vždy**, uvidíte **protokolu** podsložky v **pyFiles** složky v kontejnerech objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="eb477-245">Since you set **getDebugInfo** to **Always**, you see a **log** subfolder in the **pyFiles** folder in your Azure Blob container.</span></span> <span data-ttu-id="eb477-246">Další podrobnosti najdete v souboru protokolu ve složce protokolů.</span><span class="sxs-lookup"><span data-stu-id="eb477-246">The log file in the log folder provides additional details.</span></span> <span data-ttu-id="eb477-247">Tento soubor protokolu je obzvláště užitečná, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="eb477-247">This log file is especially useful when there is an error.</span></span> <span data-ttu-id="eb477-248">V produkčním prostředí, můžete ji nastavit na **selhání**.</span><span class="sxs-lookup"><span data-stu-id="eb477-248">In a production environment, you may want to set it to **Failure**.</span></span>

<span data-ttu-id="eb477-249">Další informace o řešení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="eb477-249">For further troubleshooting, do the following steps:</span></span>


1. <span data-ttu-id="eb477-250">Přejděte na `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span><span class="sxs-lookup"><span data-stu-id="eb477-250">Navigate to `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span></span>

    ![Uživatelské rozhraní YARN aplikace](media/data-factory-spark/yarnui-application.png)  
2. <span data-ttu-id="eb477-252">Klikněte na tlačítko **protokoly** pro jedno spuštění pokusí.</span><span class="sxs-lookup"><span data-stu-id="eb477-252">Click **Logs** for one of the run attempts.</span></span>

    ![Stránka aplikace](media/data-factory-spark/yarn-applications.png)
3. <span data-ttu-id="eb477-254">Měli byste vidět další chybové informace na stránce protokolu.</span><span class="sxs-lookup"><span data-stu-id="eb477-254">You should see additional error information in the log page.</span></span>

    ![Chyba protokolu](media/data-factory-spark/yarnui-application-error.png)

<span data-ttu-id="eb477-256">Následující části obsahují informace o entit služby Data Factory používat cluster Apache Spark a aktivity Spark v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="eb477-256">The following sections provide information about Data Factory entities to use Apache Spark cluster and Spark Activity in your data factory.</span></span>

## <a name="spark-activity-properties"></a><span data-ttu-id="eb477-257">Vlastnosti aktivity Spark</span><span class="sxs-lookup"><span data-stu-id="eb477-257">Spark activity properties</span></span>
<span data-ttu-id="eb477-258">Zde je ukázka definici JSON kanálu s aktivitou Spark:</span><span class="sxs-lookup"><span data-stu-id="eb477-258">Here is the sample JSON definition of a pipeline with Spark Activity:</span></span>    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

<span data-ttu-id="eb477-259">Následující tabulka popisuje vlastnostech JSON použitých v definici JSON:</span><span class="sxs-lookup"><span data-stu-id="eb477-259">The following table describes the JSON properties used in the JSON definition:</span></span>

| <span data-ttu-id="eb477-260">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="eb477-260">Property</span></span> | <span data-ttu-id="eb477-261">Popis</span><span class="sxs-lookup"><span data-stu-id="eb477-261">Description</span></span> | <span data-ttu-id="eb477-262">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="eb477-262">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="eb477-263">jméno</span><span class="sxs-lookup"><span data-stu-id="eb477-263">name</span></span> | <span data-ttu-id="eb477-264">Název aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="eb477-264">Name of the activity in the pipeline.</span></span> | <span data-ttu-id="eb477-265">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-265">Yes</span></span> |
| <span data-ttu-id="eb477-266">Popis</span><span class="sxs-lookup"><span data-stu-id="eb477-266">description</span></span> | <span data-ttu-id="eb477-267">Popisuje, jakým způsobem aktivita naloží text.</span><span class="sxs-lookup"><span data-stu-id="eb477-267">Text describing what the activity does.</span></span> | <span data-ttu-id="eb477-268">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-268">No</span></span> |
| <span data-ttu-id="eb477-269">type</span><span class="sxs-lookup"><span data-stu-id="eb477-269">type</span></span> | <span data-ttu-id="eb477-270">Tato vlastnost musí být nastavená na HDInsightSpark.</span><span class="sxs-lookup"><span data-stu-id="eb477-270">This property must be set to HDInsightSpark.</span></span> | <span data-ttu-id="eb477-271">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-271">Yes</span></span> |
| <span data-ttu-id="eb477-272">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="eb477-272">linkedServiceName</span></span> | <span data-ttu-id="eb477-273">Název propojená služba HDInsight na kterém běží Spark program.</span><span class="sxs-lookup"><span data-stu-id="eb477-273">Name of the HDInsight linked service on which the Spark program runs.</span></span> | <span data-ttu-id="eb477-274">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-274">Yes</span></span> |
| <span data-ttu-id="eb477-275">rootPath</span><span class="sxs-lookup"><span data-stu-id="eb477-275">rootPath</span></span> | <span data-ttu-id="eb477-276">Kontejner objektů Blob v Azure a složky, která obsahuje soubor Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-276">The Azure Blob container and folder that contains the Spark file.</span></span> <span data-ttu-id="eb477-277">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="eb477-277">The file name is case-sensitive.</span></span> | <span data-ttu-id="eb477-278">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-278">Yes</span></span> |
| <span data-ttu-id="eb477-279">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="eb477-279">entryFilePath</span></span> | <span data-ttu-id="eb477-280">Relativní cesta ke kořenové složce Spark kódu nebo balíčku.</span><span class="sxs-lookup"><span data-stu-id="eb477-280">Relative path to the root folder of the Spark code/package.</span></span> | <span data-ttu-id="eb477-281">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-281">Yes</span></span> |
| <span data-ttu-id="eb477-282">Název třídy</span><span class="sxs-lookup"><span data-stu-id="eb477-282">className</span></span> | <span data-ttu-id="eb477-283">Hlavní třídy aplikace Java/Spark</span><span class="sxs-lookup"><span data-stu-id="eb477-283">Application's Java/Spark main class</span></span> | <span data-ttu-id="eb477-284">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-284">No</span></span> |
| <span data-ttu-id="eb477-285">Argumenty</span><span class="sxs-lookup"><span data-stu-id="eb477-285">arguments</span></span> | <span data-ttu-id="eb477-286">Seznam argumentů příkazového řádku pro Spark program.</span><span class="sxs-lookup"><span data-stu-id="eb477-286">A list of command-line arguments to the Spark program.</span></span> | <span data-ttu-id="eb477-287">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-287">No</span></span> |
| <span data-ttu-id="eb477-288">proxyUser</span><span class="sxs-lookup"><span data-stu-id="eb477-288">proxyUser</span></span> | <span data-ttu-id="eb477-289">Uživatelský účet zosobnění spuštění programu Spark</span><span class="sxs-lookup"><span data-stu-id="eb477-289">The user account to impersonate to execute the Spark program</span></span> | <span data-ttu-id="eb477-290">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-290">No</span></span> |
| <span data-ttu-id="eb477-291">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="eb477-291">sparkConfig</span></span> | <span data-ttu-id="eb477-292">Zadejte hodnoty pro vlastnosti konfigurace Spark vypsané v tomto tématu: [Spark konfigurace – vlastnosti aplikace](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span><span class="sxs-lookup"><span data-stu-id="eb477-292">Specify values for Spark configuration properties listed in the topic: [Spark Configuration - Application properties](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span></span> | <span data-ttu-id="eb477-293">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-293">No</span></span> |
| <span data-ttu-id="eb477-294">getdebuginfo –</span><span class="sxs-lookup"><span data-stu-id="eb477-294">getDebugInfo</span></span> | <span data-ttu-id="eb477-295">Určuje, kdy soubory protokolu Spark se zkopírují do úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="eb477-295">Specifies when the Spark log files are copied to the Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="eb477-296">Povolené hodnoty: None, vždy nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="eb477-296">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="eb477-297">Výchozí hodnota: žádné.</span><span class="sxs-lookup"><span data-stu-id="eb477-297">Default value: None.</span></span> | <span data-ttu-id="eb477-298">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-298">No</span></span> |
| <span data-ttu-id="eb477-299">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="eb477-299">sparkJobLinkedService</span></span> | <span data-ttu-id="eb477-300">Azure Storage propojená služba, která obsahuje Spark soubor úlohy, závislosti a protokoly.</span><span class="sxs-lookup"><span data-stu-id="eb477-300">The Azure Storage linked service that holds the Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="eb477-301">Pokud hodnotu pro tuto vlastnost nezadáte, použije se úložiště přidružený k clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eb477-301">If you do not specify a value for this property, the storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="eb477-302">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-302">No</span></span> |

## <a name="folder-structure"></a><span data-ttu-id="eb477-303">Struktura složek</span><span class="sxs-lookup"><span data-stu-id="eb477-303">Folder structure</span></span>
<span data-ttu-id="eb477-304">Aktivita Spark nepodporuje skriptu v řádku jako Pig a do aktivity Hive.</span><span class="sxs-lookup"><span data-stu-id="eb477-304">The Spark activity does not support an in-line script as Pig and Hive activities do.</span></span> <span data-ttu-id="eb477-305">Spark úlohy jsou také více extensible než úlohy Pig nebo Hive.</span><span class="sxs-lookup"><span data-stu-id="eb477-305">Spark jobs are also more extensible than Pig/Hive jobs.</span></span> <span data-ttu-id="eb477-306">Pro úlohy Spark, můžete zadat více závislostí, jako jar balíčky (umístěné v jazyce java cesty pro třídy), soubory python (umístit na PYTHONPATH) a všechny další soubory.</span><span class="sxs-lookup"><span data-stu-id="eb477-306">For Spark jobs, you can provide multiple dependencies such as jar packages (placed in the java CLASSPATH), python files (placed on the PYTHONPATH), and any other files.</span></span>

<span data-ttu-id="eb477-307">Vytvořte následující strukturu složek ve službě Azure Blob storage, na kterou odkazuje propojená služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eb477-307">Create the following folder structure in the Azure Blob storage referenced by the HDInsight linked service.</span></span> <span data-ttu-id="eb477-308">Potom obsah nahrajete soubory závislé na příslušné podsložek v kořenové složce reprezentována **entryFilePath**.</span><span class="sxs-lookup"><span data-stu-id="eb477-308">Then, upload dependent files to the appropriate sub folders in the root folder represented by **entryFilePath**.</span></span> <span data-ttu-id="eb477-309">Například nahrajte python soubory do pyFiles podsložky a soubory jar do podsložky JAR kořenové složky.</span><span class="sxs-lookup"><span data-stu-id="eb477-309">For example, upload python files to the pyFiles subfolder and jar files to the jars subfolder of the root folder.</span></span> <span data-ttu-id="eb477-310">Služba Data Factory v době běhu očekává následující strukturu složek ve službě Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="eb477-310">At runtime, Data Factory service expects the following folder structure in the Azure Blob storage:</span></span>     

| <span data-ttu-id="eb477-311">Cesta</span><span class="sxs-lookup"><span data-stu-id="eb477-311">Path</span></span> | <span data-ttu-id="eb477-312">Popis</span><span class="sxs-lookup"><span data-stu-id="eb477-312">Description</span></span> | <span data-ttu-id="eb477-313">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="eb477-313">Required</span></span> | <span data-ttu-id="eb477-314">Typ</span><span class="sxs-lookup"><span data-stu-id="eb477-314">Type</span></span> |
| ---- | ----------- | -------- | ---- |
| <span data-ttu-id="eb477-315">.</span><span class="sxs-lookup"><span data-stu-id="eb477-315">.</span></span> | <span data-ttu-id="eb477-316">Kořenová cesta úlohy Spark v úložišti propojená služba</span><span class="sxs-lookup"><span data-stu-id="eb477-316">The root path of the Spark job in the storage linked service</span></span>  | <span data-ttu-id="eb477-317">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-317">Yes</span></span> | <span data-ttu-id="eb477-318">Složka</span><span class="sxs-lookup"><span data-stu-id="eb477-318">Folder</span></span> |
| <span data-ttu-id="eb477-319">&lt;uživatelem definované&gt;</span><span class="sxs-lookup"><span data-stu-id="eb477-319">&lt;user defined &gt;</span></span> | <span data-ttu-id="eb477-320">Cesta k souboru položka úlohy Spark</span><span class="sxs-lookup"><span data-stu-id="eb477-320">The path pointing to the entry file of the Spark job</span></span> | <span data-ttu-id="eb477-321">Ano</span><span class="sxs-lookup"><span data-stu-id="eb477-321">Yes</span></span> | <span data-ttu-id="eb477-322">File</span><span class="sxs-lookup"><span data-stu-id="eb477-322">File</span></span> |
| <span data-ttu-id="eb477-323">. / jars</span><span class="sxs-lookup"><span data-stu-id="eb477-323">./jars</span></span> | <span data-ttu-id="eb477-324">Všechny soubory v této složce se nahrála a umístit na cestě třídy java clusteru</span><span class="sxs-lookup"><span data-stu-id="eb477-324">All files under this folder are uploaded and placed on the java classpath of the cluster</span></span> | <span data-ttu-id="eb477-325">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-325">No</span></span> | <span data-ttu-id="eb477-326">Složka</span><span class="sxs-lookup"><span data-stu-id="eb477-326">Folder</span></span> |
| <span data-ttu-id="eb477-327">. / pyFiles</span><span class="sxs-lookup"><span data-stu-id="eb477-327">./pyFiles</span></span> | <span data-ttu-id="eb477-328">Všechny soubory v této složce se nahrála a umístit do PYTHONPATH clusteru</span><span class="sxs-lookup"><span data-stu-id="eb477-328">All files under this folder are uploaded and placed on the PYTHONPATH of the cluster</span></span> | <span data-ttu-id="eb477-329">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-329">No</span></span> | <span data-ttu-id="eb477-330">Složka</span><span class="sxs-lookup"><span data-stu-id="eb477-330">Folder</span></span> |
| <span data-ttu-id="eb477-331">. / soubory</span><span class="sxs-lookup"><span data-stu-id="eb477-331">./files</span></span> | <span data-ttu-id="eb477-332">Všechny soubory v této složce se nahrála a umístit na vykonavatele pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="eb477-332">All files under this folder are uploaded and placed on executor working directory</span></span> | <span data-ttu-id="eb477-333">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-333">No</span></span> | <span data-ttu-id="eb477-334">Složka</span><span class="sxs-lookup"><span data-stu-id="eb477-334">Folder</span></span> |
| <span data-ttu-id="eb477-335">. / archivy</span><span class="sxs-lookup"><span data-stu-id="eb477-335">./archives</span></span> | <span data-ttu-id="eb477-336">Všechny soubory v této složce nekomprimované</span><span class="sxs-lookup"><span data-stu-id="eb477-336">All files under this folder are uncompressed</span></span> | <span data-ttu-id="eb477-337">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-337">No</span></span> | <span data-ttu-id="eb477-338">Složka</span><span class="sxs-lookup"><span data-stu-id="eb477-338">Folder</span></span> |
| <span data-ttu-id="eb477-339">. / protokoly</span><span class="sxs-lookup"><span data-stu-id="eb477-339">./logs</span></span> | <span data-ttu-id="eb477-340">Složky, kde jsou uložené protokoly z clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="eb477-340">The folder where logs from the Spark cluster are stored.</span></span>| <span data-ttu-id="eb477-341">Ne</span><span class="sxs-lookup"><span data-stu-id="eb477-341">No</span></span> | <span data-ttu-id="eb477-342">Složka</span><span class="sxs-lookup"><span data-stu-id="eb477-342">Folder</span></span> |

<span data-ttu-id="eb477-343">Tady je příklad pro úložiště, který obsahuje dva soubory úlohy Spark v Azure Blob Storage odkazuje propojená služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eb477-343">Here is an example for a storage containing two Spark job files in the Azure Blob Storage referenced by the HDInsight linked service.</span></span>

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
