---
title: aaaInvoke Spark programy z Azure Data Factory | Microsoft Docs
description: "Zjistěte, jak tooinvoke Spark programy z objektu pro vytváření dat Azure pomocí hello činnost MapReduce."
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
ms.openlocfilehash: f88943ece7ee3d21dedbd857609f1b2713b62741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a><span data-ttu-id="bd903-103">Vyvolání Spark programy z kanálů služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="bd903-103">Invoke Spark programs from Azure Data Factory pipelines</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="bd903-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="bd903-104">Hive Activity</span></span>](data-factory-hive-activity.md)
> * [<span data-ttu-id="bd903-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="bd903-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="bd903-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="bd903-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="bd903-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="bd903-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="bd903-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="bd903-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="bd903-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bd903-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="bd903-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bd903-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="bd903-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="bd903-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="bd903-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="bd903-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="bd903-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="bd903-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="bd903-114">Úvod</span><span class="sxs-lookup"><span data-stu-id="bd903-114">Introduction</span></span>
<span data-ttu-id="bd903-115">Aktivita Spark je jedním z hello [aktivit transformace dat](data-factory-data-transformation-activities.md) podporovaných službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="bd903-115">Spark Activity is one of hello [data transformation activities](data-factory-data-transformation-activities.md) supported by Azure Data Factory.</span></span> <span data-ttu-id="bd903-116">Tato aktivita se spustí hello zadaný program Spark na cluster Apache Spark v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bd903-116">This activity runs hello specified Spark program on your Apache Spark cluster in Azure HDInsight.</span></span>    

> [!IMPORTANT]
> - <span data-ttu-id="bd903-117">Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd903-117">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
> - <span data-ttu-id="bd903-118">Aktivita Spark podporuje pouze existující (vlastní) clustery HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="bd903-118">Spark Activity supports only existing (your own) HDInsight Spark clusters.</span></span> <span data-ttu-id="bd903-119">HDInsight propojené služby na vyžádání nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="bd903-119">It does not support an on-demand HDInsight linked service.</span></span>

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a><span data-ttu-id="bd903-120">Návod: vytvoření kanálu s aktivitou Spark</span><span class="sxs-lookup"><span data-stu-id="bd903-120">Walkthrough: create a pipeline with Spark activity</span></span>
<span data-ttu-id="bd903-121">Tady jsou toocreate hello obvyklé kroky pro vytváření dat kanál s aktivitou Spark.</span><span class="sxs-lookup"><span data-stu-id="bd903-121">Here are hello typical steps toocreate a Data Factory pipeline with a Spark activity.</span></span>  

1. <span data-ttu-id="bd903-122">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="bd903-122">Create a data factory.</span></span>
2. <span data-ttu-id="bd903-123">Vytvoření Azure Storage, propojené služby toolink vašeho úložiště Azure, která souvisí s datovou továrnu toohello clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="bd903-123">Create an Azure Storage linked service toolink your Azure storage that is associated with your HDInsight Spark cluster toohello data factory.</span></span>     
2. <span data-ttu-id="bd903-124">Vytvořte Azure HDInsight propojené služby toolink váš cluster Apache Spark v Azure HDInsight toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="bd903-124">Create an Azure HDInsight linked service toolink your Apache Spark cluster in Azure HDInsight toohello data factory.</span></span>
3. <span data-ttu-id="bd903-125">Vytvořte datovou sadu, která odkazuje toohello propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-125">Create a dataset that refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="bd903-126">V současné době je třeba zadat datovou sadu výstupů pro aktivitu i v případě, že neexistuje žádný výstup se vytváří.</span><span class="sxs-lookup"><span data-stu-id="bd903-126">Currently, you must specify an output dataset for an activity even if there is no output being produced.</span></span>  
4. <span data-ttu-id="bd903-127">Vytvoření kanálu s aktivitou Spark, která odkazuje toohello propojené služby HDInsight vytvořené v #. 2.</span><span class="sxs-lookup"><span data-stu-id="bd903-127">Create a pipeline with Spark activity that refers toohello HDInsight linked service created in #2.</span></span> <span data-ttu-id="bd903-128">Aktivita Hello je nakonfigurovaný s hello datovou sadu, kterou jste vytvořili v předchozím kroku hello jako výstupní datové.</span><span class="sxs-lookup"><span data-stu-id="bd903-128">hello activity is configured with hello dataset you created in hello previous step as an output dataset.</span></span> <span data-ttu-id="bd903-129">Hello výstupní datová sada, jaké jednotky hello plán (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="bd903-129">hello output dataset is what drives hello schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="bd903-130">Proto je nutné zadat hello výstupní datovou sadu, i když hello aktivita nevytváří skutečně výstup.</span><span class="sxs-lookup"><span data-stu-id="bd903-130">Therefore, you must specify hello output dataset even though hello activity does not really produce an output.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bd903-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bd903-131">Prerequisites</span></span>
1. <span data-ttu-id="bd903-132">Vytvořit **účet úložiště pro obecné účely Azure** podle pokynů v Průvodci hello: [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="bd903-132">Create a **general-purpose Azure Storage Account** by following instructions in hello walkthrough: [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
2. <span data-ttu-id="bd903-133">Vytvořit **cluster Apache Spark v Azure HDInsight** podle pokynů v kurzu hello: [cluster vytvořit Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="bd903-133">Create an **Apache Spark cluster in Azure HDInsight** by following instructions in hello tutorial: [Create Apache Spark cluster in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="bd903-134">Přidružte hello účtu úložiště Azure, kterou jste vytvořili v kroku #1 se tento cluster.</span><span class="sxs-lookup"><span data-stu-id="bd903-134">Associate hello Azure storage account you created in step #1 with this cluster.</span></span>  
3. <span data-ttu-id="bd903-135">Stáhnout a revidovat soubor skriptu jazyka python hello **test.py** nacházející se v: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span><span class="sxs-lookup"><span data-stu-id="bd903-135">Download and review hello python script file **test.py** located at: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span></span>  
3.  <span data-ttu-id="bd903-136">Nahrát **test.py** toohello **pyFiles** složky v hello **adfspark** kontejneru ve službě Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bd903-136">Upload **test.py** toohello **pyFiles** folder in hello **adfspark** container in your Azure Blob storage.</span></span> <span data-ttu-id="bd903-137">Pokud neexistuje, vytvořte hello kontejneru a složce hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-137">Create hello container and hello folder if they do not exist.</span></span>

### <a name="create-data-factory"></a><span data-ttu-id="bd903-138">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="bd903-138">Create data factory</span></span>
<span data-ttu-id="bd903-139">Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="bd903-139">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="bd903-140">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bd903-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="bd903-141">Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="bd903-141">Click **NEW** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="bd903-142">V hello **nový objekt pro vytváření dat** okno, zadejte **SparkDF** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="bd903-142">In hello **New data factory** blade, enter **SparkDF** for hello Name.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="bd903-143">musí být Hello název objektu pro vytváření dat Azure hello **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="bd903-143">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="bd903-144">Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "SparkDF" není k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="bd903-144">If you see hello error: **Data factory name “SparkDF” is not available**.</span></span> <span data-ttu-id="bd903-145">Změňte hello název objektu pro vytváření dat hello (například yournameSparkDFdate a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="bd903-145">Change hello name of hello data factory (for example, yournameSparkDFdate, and try creating again.</span></span> <span data-ttu-id="bd903-146">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="bd903-146">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
4. <span data-ttu-id="bd903-147">Vyberte hello **předplatného Azure** místo hello data factory toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="bd903-147">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="bd903-148">Vyberte existující **skupiny prostředků** nebo vytvořte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-148">Select an existing **resource group** or create an Azure resource group.</span></span>
6. <span data-ttu-id="bd903-149">Vyberte **Pin toodashboard** možnost.</span><span class="sxs-lookup"><span data-stu-id="bd903-149">Select **Pin toodashboard** option.</span></span>  
6. <span data-ttu-id="bd903-150">Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="bd903-150">Click **Create** on hello **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="bd903-151">instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-151">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
7. <span data-ttu-id="bd903-152">Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portál Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bd903-152">You see hello data factory being created in hello **dashboard** of hello Azure portal as follows:</span></span>   
8. <span data-ttu-id="bd903-153">Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-153">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span> <span data-ttu-id="bd903-154">Pokud se stránka objektu pro vytváření dat hello nezobrazí, klikněte na dlaždici hello objektu pro vytváření dat na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-154">If you do not see hello data factory page, click hello tile for your data factory on hello dashboard.</span></span>

    ![Okno Objekt pro vytváření dat](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a><span data-ttu-id="bd903-156">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="bd903-156">Create linked services</span></span>
<span data-ttu-id="bd903-157">V tomto kroku vytvoříte dvě propojené služby, toolink jednu datovou továrnu tooyour clusteru Spark a hello jiných toolink datovou továrnu tooyour úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-157">In this step, you create two linked services, one toolink your Spark cluster tooyour data factory, and hello other toolink your Azure storage tooyour data factory.</span></span>  

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="bd903-158">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bd903-158">Create Azure Storage linked service</span></span>
<span data-ttu-id="bd903-159">V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-159">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="bd903-160">Datové sady, které vytvoříte v kroku později v tomto názorném postupu odkazuje toothis propojené služby.</span><span class="sxs-lookup"><span data-stu-id="bd903-160">A dataset you create in a step later in this walkthrough refers toothis linked service.</span></span> <span data-ttu-id="bd903-161">Hello propojené služby HDInsight, kterou definujete v dalším kroku hello příliš odkazuje toothis propojené služby.</span><span class="sxs-lookup"><span data-stu-id="bd903-161">hello HDInsight linked service that you define in hello next step refers toothis linked service too.</span></span>  

1. <span data-ttu-id="bd903-162">Klikněte na tlačítko **vytvořit a nasadit** na hello **Data Factory** okno objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="bd903-162">Click **Author and deploy** on hello **Data Factory** blade for your data factory.</span></span> <span data-ttu-id="bd903-163">Měli byste vidět hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="bd903-163">You should see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="bd903-164">Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.</span><span class="sxs-lookup"><span data-stu-id="bd903-164">Click **New data store** and choose **Azure storage**.</span></span>

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="bd903-166">Měli byste vidět hello **skript JSON** pro vytvoření Azure Storage propojená služba v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-166">You should see hello **JSON script** for creating an Azure Storage linked service in hello editor.</span></span>

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="bd903-168">Nahraďte **název účtu** a **klíč účtu** hello názvem a přístupovým klíčem k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-168">Replace **account name** and **account key** with hello name and access key of your Azure storage account.</span></span> <span data-ttu-id="bd903-169">toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="bd903-169">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="bd903-170">toodeploy hello propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-170">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="bd903-171">Po hello propojené služby nasazení úspěšně hello **koncept-1** by měl zmizet okně a zobrazí **AzureStorageLinkedService** v hello stromovém zobrazení na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-171">After hello linked service is deployed successfully, hello **Draft-1** window should disappear and you see **AzureStorageLinkedService** in hello tree view on hello left.</span></span>

#### <a name="create-hdinsight-linked-service"></a><span data-ttu-id="bd903-172">Vytvoření propojené služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="bd903-172">Create HDInsight linked service</span></span>
<span data-ttu-id="bd903-173">V tomto kroku vytvoříte toolink Azure HDInsight propojené služby clusteru HDInsight Spark toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="bd903-173">In this step, you create Azure HDInsight linked service toolink your HDInsight Spark cluster toohello data factory.</span></span> <span data-ttu-id="bd903-174">Hello HDInsight cluster je použité toorun hello Spark program zadaný v aktivitě Spark hello hello kanálu v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="bd903-174">hello HDInsight cluster is used toorun hello Spark program specified in hello Spark activity of hello pipeline in this sample.</span></span>  

1. <span data-ttu-id="bd903-175">Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na hello nástrojů, klikněte na tlačítko **nový výpočet**a potom klikněte na **clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="bd903-175">Click **... More** on hello toolbar, click **New compute**, and then click **HDInsight cluster**.</span></span>

    ![Vytvoření propojené služby HDInsight](media/data-factory-spark/new-hdinsight-linked-service.png)
2. <span data-ttu-id="bd903-177">Zkopírujte a vložte následující fragment kódu toohello hello **koncept-1** okno.</span><span class="sxs-lookup"><span data-stu-id="bd903-177">Copy and paste hello following snippet toohello **Draft-1** window.</span></span> <span data-ttu-id="bd903-178">V editoru JSON hello hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd903-178">In hello JSON editor, do hello following steps:</span></span>
    1. <span data-ttu-id="bd903-179">Zadejte hello **URI** hello HDInsight Spark clusteru.</span><span class="sxs-lookup"><span data-stu-id="bd903-179">Specify hello **URI** for hello HDInsight Spark cluster.</span></span> <span data-ttu-id="bd903-180">Například: `https://<sparkclustername>.azurehdinsight.net/`.</span><span class="sxs-lookup"><span data-stu-id="bd903-180">For example: `https://<sparkclustername>.azurehdinsight.net/`.</span></span>
    2. <span data-ttu-id="bd903-181">Zadejte název hello hello **uživatele** kdo má cluster Spark toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="bd903-181">Specify hello name of hello **user** who has access toohello Spark cluster.</span></span>
    3. <span data-ttu-id="bd903-182">Zadejte hello **heslo** pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd903-182">Specify hello **password** for user.</span></span>
    4. <span data-ttu-id="bd903-183">Zadejte hello **propojená služba Azure Storage** který je přidružen hello clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="bd903-183">Specify hello **Azure Storage linked service** that is associated with hello HDInsight Spark cluster.</span></span> <span data-ttu-id="bd903-184">V tomto příkladu je: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="bd903-184">In this example, it is: **AzureStorageLinkedService**.</span></span>

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
    > - <span data-ttu-id="bd903-185">Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd903-185">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
    > - <span data-ttu-id="bd903-186">Aktivita Spark podporuje pouze existující (vlastní) clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="bd903-186">Spark Activity supports only existing (your own) HDInsight Spark cluster.</span></span> <span data-ttu-id="bd903-187">HDInsight propojené služby na vyžádání nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="bd903-187">It does not support an on-demand HDInsight linked service.</span></span>

    <span data-ttu-id="bd903-188">V tématu [propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) podrobnosti o hello HDInsight propojené služby.</span><span class="sxs-lookup"><span data-stu-id="bd903-188">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details about hello HDInsight linked service.</span></span>
3.  <span data-ttu-id="bd903-189">toodeploy hello propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-189">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="bd903-190">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="bd903-190">Create output dataset</span></span>
<span data-ttu-id="bd903-191">Hello výstupní datová sada, jaké jednotky hello plán (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="bd903-191">hello output dataset is what drives hello schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="bd903-192">Proto je nutné zadat výstupní datovou sadu aktivity hello spark v kanálu hello, i když aktivita hello skutečně nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="bd903-192">Therefore, you must specify an output dataset for hello spark activity in hello pipeline even though hello activity does not really produce any output.</span></span> <span data-ttu-id="bd903-193">Určení vstupní datové sady pro aktivitu hello je volitelné.</span><span class="sxs-lookup"><span data-stu-id="bd903-193">Specifying an input dataset for hello activity is optional.</span></span>

1. <span data-ttu-id="bd903-194">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a vyberte **úložiště objektů Azure Blob**.</span><span class="sxs-lookup"><span data-stu-id="bd903-194">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="bd903-195">Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-195">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="bd903-196">Hello fragmentu kódu JSON Určuje datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="bd903-196">hello JSON snippet defines a dataset called **OutputDataset**.</span></span> <span data-ttu-id="bd903-197">Kromě toho určíte, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfspark** a hello složku s názvem **pyFiles výstupní**.</span><span class="sxs-lookup"><span data-stu-id="bd903-197">In addition, you specify that hello results are stored in hello blob container called **adfspark** and hello folder called **pyFiles/output**.</span></span> <span data-ttu-id="bd903-198">Jak už bylo zmíněno dříve, je tato datová sada fiktivní datová sada.</span><span class="sxs-lookup"><span data-stu-id="bd903-198">As mentioned earlier, this dataset is a dummy dataset.</span></span> <span data-ttu-id="bd903-199">Hello Spark program v tomto příkladu nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="bd903-199">hello Spark program in this example does not produce any output.</span></span> <span data-ttu-id="bd903-200">Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou denně.</span><span class="sxs-lookup"><span data-stu-id="bd903-200">hello **availability** section specifies that hello output dataset is produced daily.</span></span>  

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
3. <span data-ttu-id="bd903-201">toodeploy hello datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-201">toodeploy hello dataset, click **Deploy** on hello command bar.</span></span>


### <a name="create-pipeline"></a><span data-ttu-id="bd903-202">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="bd903-202">Create pipeline</span></span>
<span data-ttu-id="bd903-203">V tomto kroku vytvoříte kanál s **HDInsightSpark** aktivity.</span><span class="sxs-lookup"><span data-stu-id="bd903-203">In this step, you create a pipeline with a **HDInsightSpark** activity.</span></span> <span data-ttu-id="bd903-204">Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="bd903-204">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="bd903-205">Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-205">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="bd903-206">Proto žádné vstupní datové sady je zadána v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="bd903-206">Therefore, no input dataset is specified in this example.</span></span>

1. <span data-ttu-id="bd903-207">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů text hello a potom klikněte na **nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="bd903-207">In hello **Data Factory Editor**, click **… More** on hello command bar, and then click **New pipeline**.</span></span>
2. <span data-ttu-id="bd903-208">Nahraďte hello skript v okně hello koncept-1 hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="bd903-208">Replace hello script in hello Draft-1 window with hello following script:</span></span>

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
    <span data-ttu-id="bd903-209">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="bd903-209">Note hello following points:</span></span>
    - <span data-ttu-id="bd903-210">Hello **typ** vlastnost je nastavena příliš**HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="bd903-210">hello **type** property is set too**HDInsightSpark**.</span></span>
    - <span data-ttu-id="bd903-211">Hello **rootPath** je nastaven příliš**adfspark\\pyFiles** kde adfspark je kontejner objektů Blob Azure hello a pyFiles je dobře složka v tomto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bd903-211">hello **rootPath** is set too**adfspark\\pyFiles** where adfspark is hello Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="bd903-212">V tomto příkladu je hello Azure Blob Storage hello ten, který je přidružen hello Spark cluster.</span><span class="sxs-lookup"><span data-stu-id="bd903-212">In this example, hello Azure Blob Storage is hello one that is associated with hello Spark cluster.</span></span> <span data-ttu-id="bd903-213">Můžete nahrát soubor tooa hello jiného úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-213">You can upload hello file tooa different Azure Storage.</span></span> <span data-ttu-id="bd903-214">Pokud tak učiníte, vytváření Azure Storage, propojené služby toolink tohoto úložiště účet toohello dat.</span><span class="sxs-lookup"><span data-stu-id="bd903-214">If you do so, create an Azure Storage linked service toolink that storage account toohello data factory.</span></span> <span data-ttu-id="bd903-215">Potom zadejte název hello hello propojené služby jako hodnotu pro hello **sparkJobLinkedService** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bd903-215">Then, specify hello name of hello linked service as a value for hello **sparkJobLinkedService** property.</span></span> <span data-ttu-id="bd903-216">V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností nepodporuje hello Spark aktivity.</span><span class="sxs-lookup"><span data-stu-id="bd903-216">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by hello Spark Activity.</span></span>  
    - <span data-ttu-id="bd903-217">Hello **entryFilePath** nastavena toohello **test.py**, což je soubor python hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-217">hello **entryFilePath** is set toohello **test.py**, which is hello python file.</span></span>
    - <span data-ttu-id="bd903-218">Hello **getdebuginfo –** vlastnost je nastavena příliš**vždy**, což znamená, že soubory protokolu hello jsou vždy vygeneruje (úspěch nebo neúspěch).</span><span class="sxs-lookup"><span data-stu-id="bd903-218">hello **getDebugInfo** property is set too**Always**, which means hello log files are always generated (success or failure).</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="bd903-219">Doporučujeme, abyste tuto vlastnost příliš nenastavujte`Always` v produkčním prostředí Pokud řešíte problém.</span><span class="sxs-lookup"><span data-stu-id="bd903-219">We recommend that you do not set this property too`Always` in a production environment unless you are troubleshooting an issue.</span></span>
    - <span data-ttu-id="bd903-220">Hello **výstupy** obsahuje jednu výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="bd903-220">hello **outputs** section has one output dataset.</span></span> <span data-ttu-id="bd903-221">Je třeba zadat datovou sadu výstupů i v případě, že hello spark program nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="bd903-221">You must specify an output dataset even if hello spark program does not produce any output.</span></span> <span data-ttu-id="bd903-222">plán hello Hello výstupní datovou sadu jednotky pro kanál hello (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="bd903-222">hello output dataset drives hello schedule for hello pipeline (hourly, daily, etc.).</span></span>  

        <span data-ttu-id="bd903-223">Podrobnosti o vlastnostech hello podporovaných aktivitou Spark najdete v tématu [Spark vlastnosti aktivity](#spark-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="bd903-223">For details about hello properties supported by Spark activity, see [Spark activity properties](#spark-activity-properties) section.</span></span>
3. <span data-ttu-id="bd903-224">toodeploy hello kanálu, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-224">toodeploy hello pipeline, click **Deploy** on hello command bar.</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="bd903-225">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="bd903-225">Monitor pipeline</span></span>
1. <span data-ttu-id="bd903-226">Klikněte na tlačítko **X** oken editoru služby Data Factory tooclose a toonavigate zpět toohello objekt pro vytváření dat domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="bd903-226">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory home page.</span></span> <span data-ttu-id="bd903-227">Klikněte na tlačítko **monitorování a správa** toolaunch hello monitorování aplikací na jiné kartě.</span><span class="sxs-lookup"><span data-stu-id="bd903-227">Click **Monitor and Manage** toolaunch hello monitoring application in another tab.</span></span>

    ![Dlaždice monitorování a Správa](media/data-factory-spark/monitor-and-manage-tile.png)
2. <span data-ttu-id="bd903-229">Změna hello **počáteční čas** filtru v horní části hello příliš**2/1/2017**a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="bd903-229">Change hello **Start time** filter at hello top too**2/1/2017**, and click **Apply**.</span></span>
3. <span data-ttu-id="bd903-230">Jak je pouze jeden den mezi hello spuštění (2017-02-01) a ukončení (2017-02-02) hello kanálu, měli byste vidět jenom jeden interval aktivity.</span><span class="sxs-lookup"><span data-stu-id="bd903-230">You should see only one activity window as there is only one day between hello start (2017-02-01) and end times (2017-02-02) of hello pipeline.</span></span> <span data-ttu-id="bd903-231">Potvrďte, že hello datový řez je v **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="bd903-231">Confirm that hello data slice is in **ready** state.</span></span>

    ![Monitorování kanálu hello](media/data-factory-spark/monitor-and-manage-app.png)    
4. <span data-ttu-id="bd903-233">Vyberte hello **okně aktivita** toosee podrobnosti o hello aktivity při spuštění.</span><span class="sxs-lookup"><span data-stu-id="bd903-233">Select hello **activity window** toosee details about hello activity run.</span></span> <span data-ttu-id="bd903-234">Pokud dojde k chybě, zobrazí podrobnosti o něm v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-234">If there is an error, you see details about it in hello right pane.</span></span>

### <a name="verify-hello-results"></a><span data-ttu-id="bd903-235">Zkontrolujte výsledky hello</span><span class="sxs-lookup"><span data-stu-id="bd903-235">Verify hello results</span></span>

1. <span data-ttu-id="bd903-236">Spusťte **Poznámkový blok Jupyter** pro váš cluster HDInsight Spark přechodem na: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="bd903-236">Launch **Jupyter notebook** for your HDInsight Spark cluster by navigating to: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="bd903-237">Můžete také spustit řídicí panel clusteru pro váš cluster HDInsight Spark a poté spusťte **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="bd903-237">You can also launch cluster dashboard for your HDInsight Spark cluster, and then launch **Jupyter Notebook**.</span></span>
2. <span data-ttu-id="bd903-238">Klikněte na tlačítko **nový** -> **PySpark** toostart nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="bd903-238">Click **New** -> **PySpark** toostart a new notebook.</span></span>

    ![Nový poznámkový blok Jupyter](media/data-factory-spark/jupyter-new-book.png)
3. <span data-ttu-id="bd903-240">Spuštění hello následující příkaz kopírování/vkládání textu hello a stisknutím klávesy **SHIFT + ENTER** na konci hello druhý příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-240">Run hello following command by copy/pasting hello text and pressing **SHIFT + ENTER** at hello end of hello second statement.</span></span>  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. <span data-ttu-id="bd903-241">Zkontrolujte, jestli hello data z tabulky TVK hello:</span><span class="sxs-lookup"><span data-stu-id="bd903-241">Confirm that you see hello data from hello hvac table:</span></span>  

    ![Výsledky dotazu Jupyter](media/data-factory-spark/jupyter-notebook-results.png)

<span data-ttu-id="bd903-243">V tématu [spuštění dotazů Spark SQL](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) části Podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="bd903-243">See [Run a Spark SQL query](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) section for detailed instructions.</span></span> 

### <a name="troubleshooting"></a><span data-ttu-id="bd903-244">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="bd903-244">Troubleshooting</span></span>
<span data-ttu-id="bd903-245">Vzhledem k tomu, že nastavíte **getdebuginfo –** příliš**vždy**, uvidíte **protokolu** podsložky v hello **pyFiles** složky v kontejnerech objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="bd903-245">Since you set **getDebugInfo** too**Always**, you see a **log** subfolder in hello **pyFiles** folder in your Azure Blob container.</span></span> <span data-ttu-id="bd903-246">Další podrobnosti najdete v souboru protokolu Hello hello do složky protokolů.</span><span class="sxs-lookup"><span data-stu-id="bd903-246">hello log file in hello log folder provides additional details.</span></span> <span data-ttu-id="bd903-247">Tento soubor protokolu je obzvláště užitečná, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="bd903-247">This log file is especially useful when there is an error.</span></span> <span data-ttu-id="bd903-248">V produkčním prostředí, může být vhodné tooset je příliš**selhání**.</span><span class="sxs-lookup"><span data-stu-id="bd903-248">In a production environment, you may want tooset it too**Failure**.</span></span>

<span data-ttu-id="bd903-249">Pro další informace o řešení, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd903-249">For further troubleshooting, do hello following steps:</span></span>


1. <span data-ttu-id="bd903-250">Přejděte příliš`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span><span class="sxs-lookup"><span data-stu-id="bd903-250">Navigate too`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span></span>

    ![Uživatelské rozhraní YARN aplikace](media/data-factory-spark/yarnui-application.png)  
2. <span data-ttu-id="bd903-252">Klikněte na tlačítko **protokoly** pro jednu hello spustit pokusy.</span><span class="sxs-lookup"><span data-stu-id="bd903-252">Click **Logs** for one of hello run attempts.</span></span>

    ![Stránka aplikace](media/data-factory-spark/yarn-applications.png)
3. <span data-ttu-id="bd903-254">Měli byste vidět další informace o chybě v protokolu stránku hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-254">You should see additional error information in hello log page.</span></span>

    ![Chyba protokolu](media/data-factory-spark/yarnui-application-error.png)

<span data-ttu-id="bd903-256">Hello následující části obsahují informace o cluster Apache Spark toouse entity služby Data Factory a aktivita Spark v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="bd903-256">hello following sections provide information about Data Factory entities toouse Apache Spark cluster and Spark Activity in your data factory.</span></span>

## <a name="spark-activity-properties"></a><span data-ttu-id="bd903-257">Vlastnosti aktivity Spark</span><span class="sxs-lookup"><span data-stu-id="bd903-257">Spark activity properties</span></span>
<span data-ttu-id="bd903-258">Tady je hello ukázka definici JSON kanálu s aktivitou Spark:</span><span class="sxs-lookup"><span data-stu-id="bd903-258">Here is hello sample JSON definition of a pipeline with Spark Activity:</span></span>    

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
                "description": "This activity invokes hello Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

<span data-ttu-id="bd903-259">Hello následující tabulka popisuje hello vlastnostech JSON použitých ve hello definici JSON:</span><span class="sxs-lookup"><span data-stu-id="bd903-259">hello following table describes hello JSON properties used in hello JSON definition:</span></span>

| <span data-ttu-id="bd903-260">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bd903-260">Property</span></span> | <span data-ttu-id="bd903-261">Popis</span><span class="sxs-lookup"><span data-stu-id="bd903-261">Description</span></span> | <span data-ttu-id="bd903-262">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="bd903-262">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="bd903-263">jméno</span><span class="sxs-lookup"><span data-stu-id="bd903-263">name</span></span> | <span data-ttu-id="bd903-264">Název hello aktivity v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-264">Name of hello activity in hello pipeline.</span></span> | <span data-ttu-id="bd903-265">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-265">Yes</span></span> |
| <span data-ttu-id="bd903-266">description</span><span class="sxs-lookup"><span data-stu-id="bd903-266">description</span></span> | <span data-ttu-id="bd903-267">Text popisující jaké aktivity hello nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="bd903-267">Text describing what hello activity does.</span></span> | <span data-ttu-id="bd903-268">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-268">No</span></span> |
| <span data-ttu-id="bd903-269">type</span><span class="sxs-lookup"><span data-stu-id="bd903-269">type</span></span> | <span data-ttu-id="bd903-270">Tato vlastnost musí být nastavená tooHDInsightSpark.</span><span class="sxs-lookup"><span data-stu-id="bd903-270">This property must be set tooHDInsightSpark.</span></span> | <span data-ttu-id="bd903-271">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-271">Yes</span></span> |
| <span data-ttu-id="bd903-272">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="bd903-272">linkedServiceName</span></span> | <span data-ttu-id="bd903-273">Název hello HDInsight propojené služby, na které hello Spark program se spustí.</span><span class="sxs-lookup"><span data-stu-id="bd903-273">Name of hello HDInsight linked service on which hello Spark program runs.</span></span> | <span data-ttu-id="bd903-274">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-274">Yes</span></span> |
| <span data-ttu-id="bd903-275">rootPath</span><span class="sxs-lookup"><span data-stu-id="bd903-275">rootPath</span></span> | <span data-ttu-id="bd903-276">kontejner objektů Blob Azure Hello a složky, která obsahuje soubor Spark hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-276">hello Azure Blob container and folder that contains hello Spark file.</span></span> <span data-ttu-id="bd903-277">Název souboru Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="bd903-277">hello file name is case-sensitive.</span></span> | <span data-ttu-id="bd903-278">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-278">Yes</span></span> |
| <span data-ttu-id="bd903-279">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="bd903-279">entryFilePath</span></span> | <span data-ttu-id="bd903-280">Relativní cesta toohello kořenové složky hello Spark nebo balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="bd903-280">Relative path toohello root folder of hello Spark code/package.</span></span> | <span data-ttu-id="bd903-281">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-281">Yes</span></span> |
| <span data-ttu-id="bd903-282">Název třídy</span><span class="sxs-lookup"><span data-stu-id="bd903-282">className</span></span> | <span data-ttu-id="bd903-283">Hlavní třídy aplikace Java/Spark</span><span class="sxs-lookup"><span data-stu-id="bd903-283">Application's Java/Spark main class</span></span> | <span data-ttu-id="bd903-284">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-284">No</span></span> |
| <span data-ttu-id="bd903-285">Argumenty</span><span class="sxs-lookup"><span data-stu-id="bd903-285">arguments</span></span> | <span data-ttu-id="bd903-286">Seznam argumentů příkazového řádku toohello Spark programu.</span><span class="sxs-lookup"><span data-stu-id="bd903-286">A list of command-line arguments toohello Spark program.</span></span> | <span data-ttu-id="bd903-287">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-287">No</span></span> |
| <span data-ttu-id="bd903-288">proxyUser</span><span class="sxs-lookup"><span data-stu-id="bd903-288">proxyUser</span></span> | <span data-ttu-id="bd903-289">Spark program hello tooimpersonate tooexecute Hello uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="bd903-289">hello user account tooimpersonate tooexecute hello Spark program</span></span> | <span data-ttu-id="bd903-290">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-290">No</span></span> |
| <span data-ttu-id="bd903-291">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="bd903-291">sparkConfig</span></span> | <span data-ttu-id="bd903-292">Zadejte hodnoty pro vlastnosti konfigurace Spark uvedených v tématu hello: [Spark konfigurace – vlastnosti aplikace](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span><span class="sxs-lookup"><span data-stu-id="bd903-292">Specify values for Spark configuration properties listed in hello topic: [Spark Configuration - Application properties](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span></span> | <span data-ttu-id="bd903-293">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-293">No</span></span> |
| <span data-ttu-id="bd903-294">getdebuginfo –</span><span class="sxs-lookup"><span data-stu-id="bd903-294">getDebugInfo</span></span> | <span data-ttu-id="bd903-295">Určuje, když jsou soubory protokolu Spark hello zkopírovaný toohello úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="bd903-295">Specifies when hello Spark log files are copied toohello Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="bd903-296">Povolené hodnoty: None, vždy nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="bd903-296">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="bd903-297">Výchozí hodnota: žádné.</span><span class="sxs-lookup"><span data-stu-id="bd903-297">Default value: None.</span></span> | <span data-ttu-id="bd903-298">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-298">No</span></span> |
| <span data-ttu-id="bd903-299">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="bd903-299">sparkJobLinkedService</span></span> | <span data-ttu-id="bd903-300">Hello Azure Storage propojená služba, která obsahuje soubor úlohy Spark hello, závislosti a protokoly.</span><span class="sxs-lookup"><span data-stu-id="bd903-300">hello Azure Storage linked service that holds hello Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="bd903-301">Pokud hodnotu pro tuto vlastnost nezadáte, použije se hello úložiště přidruženého k clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bd903-301">If you do not specify a value for this property, hello storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="bd903-302">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-302">No</span></span> |

## <a name="folder-structure"></a><span data-ttu-id="bd903-303">Struktura složek</span><span class="sxs-lookup"><span data-stu-id="bd903-303">Folder structure</span></span>
<span data-ttu-id="bd903-304">Hello Spark aktivity nepodporuje skriptu v řádku jako Pig a do aktivity Hive.</span><span class="sxs-lookup"><span data-stu-id="bd903-304">hello Spark activity does not support an in-line script as Pig and Hive activities do.</span></span> <span data-ttu-id="bd903-305">Spark úlohy jsou také více extensible než úlohy Pig nebo Hive.</span><span class="sxs-lookup"><span data-stu-id="bd903-305">Spark jobs are also more extensible than Pig/Hive jobs.</span></span> <span data-ttu-id="bd903-306">Pro úlohy Spark, můžete zadat více závislostí, jako jar balíčky (umístěné v jazyce java hello cesty pro třídy), soubory python (umístit na hello PYTHONPATH) a všechny další soubory.</span><span class="sxs-lookup"><span data-stu-id="bd903-306">For Spark jobs, you can provide multiple dependencies such as jar packages (placed in hello java CLASSPATH), python files (placed on hello PYTHONPATH), and any other files.</span></span>

<span data-ttu-id="bd903-307">Vytvořte hello následující strukturu složek v hello úložiště objektů Azure Blob odkazuje hello propojené služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bd903-307">Create hello following folder structure in hello Azure Blob storage referenced by hello HDInsight linked service.</span></span> <span data-ttu-id="bd903-308">Potom obsah nahrajete závislé soubory toohello odpovídající podsložek hello kořenové složky, která je reprezentována **entryFilePath**.</span><span class="sxs-lookup"><span data-stu-id="bd903-308">Then, upload dependent files toohello appropriate sub folders in hello root folder represented by **entryFilePath**.</span></span> <span data-ttu-id="bd903-309">Můžete například nahrajte python soubory toohello pyFiles podsložky a soubory jar toohello JAR podsložky hello kořenové složky.</span><span class="sxs-lookup"><span data-stu-id="bd903-309">For example, upload python files toohello pyFiles subfolder and jar files toohello jars subfolder of hello root folder.</span></span> <span data-ttu-id="bd903-310">Služba Data Factory v době běhu očekává hello následující strukturu složek v hello úložiště objektů Blob v Azure:</span><span class="sxs-lookup"><span data-stu-id="bd903-310">At runtime, Data Factory service expects hello following folder structure in hello Azure Blob storage:</span></span>     

| <span data-ttu-id="bd903-311">Cesta</span><span class="sxs-lookup"><span data-stu-id="bd903-311">Path</span></span> | <span data-ttu-id="bd903-312">Popis</span><span class="sxs-lookup"><span data-stu-id="bd903-312">Description</span></span> | <span data-ttu-id="bd903-313">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="bd903-313">Required</span></span> | <span data-ttu-id="bd903-314">Typ</span><span class="sxs-lookup"><span data-stu-id="bd903-314">Type</span></span> |
| ---- | ----------- | -------- | ---- |
| <span data-ttu-id="bd903-315">.</span><span class="sxs-lookup"><span data-stu-id="bd903-315">.</span></span> | <span data-ttu-id="bd903-316">Kořenová cesta Hello hello úlohy Spark v hello propojená služba úložiště</span><span class="sxs-lookup"><span data-stu-id="bd903-316">hello root path of hello Spark job in hello storage linked service</span></span>    | <span data-ttu-id="bd903-317">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-317">Yes</span></span> | <span data-ttu-id="bd903-318">Složka</span><span class="sxs-lookup"><span data-stu-id="bd903-318">Folder</span></span> |
| <span data-ttu-id="bd903-319">&lt;uživatelem definované&gt;</span><span class="sxs-lookup"><span data-stu-id="bd903-319">&lt;user defined &gt;</span></span> | <span data-ttu-id="bd903-320">Cesta Hello odkazující toohello vstupní soubor úlohy Spark hello</span><span class="sxs-lookup"><span data-stu-id="bd903-320">hello path pointing toohello entry file of hello Spark job</span></span> | <span data-ttu-id="bd903-321">Ano</span><span class="sxs-lookup"><span data-stu-id="bd903-321">Yes</span></span> | <span data-ttu-id="bd903-322">File</span><span class="sxs-lookup"><span data-stu-id="bd903-322">File</span></span> |
| <span data-ttu-id="bd903-323">. / jars</span><span class="sxs-lookup"><span data-stu-id="bd903-323">./jars</span></span> | <span data-ttu-id="bd903-324">Všechny soubory v této složce se nahrála a umístit na cestě třídy java hello hello clusteru</span><span class="sxs-lookup"><span data-stu-id="bd903-324">All files under this folder are uploaded and placed on hello java classpath of hello cluster</span></span> | <span data-ttu-id="bd903-325">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-325">No</span></span> | <span data-ttu-id="bd903-326">Složka</span><span class="sxs-lookup"><span data-stu-id="bd903-326">Folder</span></span> |
| <span data-ttu-id="bd903-327">. / pyFiles</span><span class="sxs-lookup"><span data-stu-id="bd903-327">./pyFiles</span></span> | <span data-ttu-id="bd903-328">Všechny soubory v této složce se nahrála a umístit na hello PYTHONPATH hello clusteru</span><span class="sxs-lookup"><span data-stu-id="bd903-328">All files under this folder are uploaded and placed on hello PYTHONPATH of hello cluster</span></span> | <span data-ttu-id="bd903-329">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-329">No</span></span> | <span data-ttu-id="bd903-330">Složka</span><span class="sxs-lookup"><span data-stu-id="bd903-330">Folder</span></span> |
| <span data-ttu-id="bd903-331">. / soubory</span><span class="sxs-lookup"><span data-stu-id="bd903-331">./files</span></span> | <span data-ttu-id="bd903-332">Všechny soubory v této složce se nahrála a umístit na vykonavatele pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="bd903-332">All files under this folder are uploaded and placed on executor working directory</span></span> | <span data-ttu-id="bd903-333">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-333">No</span></span> | <span data-ttu-id="bd903-334">Složka</span><span class="sxs-lookup"><span data-stu-id="bd903-334">Folder</span></span> |
| <span data-ttu-id="bd903-335">. / archivy</span><span class="sxs-lookup"><span data-stu-id="bd903-335">./archives</span></span> | <span data-ttu-id="bd903-336">Všechny soubory v této složce nekomprimované</span><span class="sxs-lookup"><span data-stu-id="bd903-336">All files under this folder are uncompressed</span></span> | <span data-ttu-id="bd903-337">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-337">No</span></span> | <span data-ttu-id="bd903-338">Složka</span><span class="sxs-lookup"><span data-stu-id="bd903-338">Folder</span></span> |
| <span data-ttu-id="bd903-339">. / protokoly</span><span class="sxs-lookup"><span data-stu-id="bd903-339">./logs</span></span> | <span data-ttu-id="bd903-340">Hello složku, kde jsou uloženy protokoly z clusteru Spark hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-340">hello folder where logs from hello Spark cluster are stored.</span></span>| <span data-ttu-id="bd903-341">Ne</span><span class="sxs-lookup"><span data-stu-id="bd903-341">No</span></span> | <span data-ttu-id="bd903-342">Složka</span><span class="sxs-lookup"><span data-stu-id="bd903-342">Folder</span></span> |

<span data-ttu-id="bd903-343">Tady je příklad pro úložiště, který obsahuje dva soubory úlohy Spark v Azure Blob Storage odkazuje hello propojené služby HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="bd903-343">Here is an example for a storage containing two Spark job files in hello Azure Blob Storage referenced by hello HDInsight linked service.</span></span>

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
