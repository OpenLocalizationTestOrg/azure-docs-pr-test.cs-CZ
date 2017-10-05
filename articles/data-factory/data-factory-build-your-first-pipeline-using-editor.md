---
title: "Sestavení prvního objektu pro vytváření dat (Azure Portal) | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte pomocí editoru služby Data Factory na webu Azure Portal ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 9c958aecb841fa02349c6b9e5e1984f6ba4fb611
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a><span data-ttu-id="0b669-103">Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0b669-103">Tutorial: Build your first Azure data factory using Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b669-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="0b669-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="0b669-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0b669-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="0b669-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b669-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="0b669-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0b669-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="0b669-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="0b669-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="0b669-109">REST API</span><span class="sxs-lookup"><span data-stu-id="0b669-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)


<span data-ttu-id="0b669-110">V tomto článku se dozvíte, jak vytvořit první datovou továrnu Azure pomocí [webu Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0b669-110">In this article, you learn how to use [Azure portal](https://portal.azure.com/) to create your first Azure data factory.</span></span> <span data-ttu-id="0b669-111">Pokud chcete udělat kurz pomocí jiných nástrojů nebo sad SDK, vyberte jednu z možností z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b669-111">To do the tutorial using other tools/SDKs, select one of the options from the drop-down list.</span></span> 

<span data-ttu-id="0b669-112">Kanál v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="0b669-112">The pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="0b669-113">Tato aktivita spouští skript Hive v clusteru Azure HDInsight, který transformuje vstupní data pro vytvoření výstupních dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data to produce output data.</span></span> <span data-ttu-id="0b669-114">Spuštění kanálu je naplánované jednou za měsíc mezi zadaným počátečním a koncovým časem.</span><span class="sxs-lookup"><span data-stu-id="0b669-114">The pipeline is scheduled to run once a month between the specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="0b669-115">Datový kanál v tomto kurzu transformuje vstupní data, aby vytvořil výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0b669-115">The data pipeline in this tutorial transforms input data to produce output data.</span></span> <span data-ttu-id="0b669-116">Kurz předvádějící způsoby kopírování dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz kopírování dat z Blob Storage do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0b669-116">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="0b669-117">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="0b669-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="0b669-118">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="0b669-118">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="0b669-119">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="0b669-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b669-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0b669-120">Prerequisites</span></span>
1. <span data-ttu-id="0b669-121">Přečtěte si článek [Přehled kurzu](data-factory-build-your-first-pipeline.md) a proveďte **nutné** kroky.</span><span class="sxs-lookup"><span data-stu-id="0b669-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete the **prerequisite** steps.</span></span>
2. <span data-ttu-id="0b669-122">Tento článek neposkytuje koncepční přehled služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-122">This article does not provide a conceptual overview of the Azure Data Factory service.</span></span> <span data-ttu-id="0b669-123">Doporučujeme projít si podrobnější přehled služby, který najdete v článku [Úvod do Azure Data Factory](data-factory-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0b669-123">We recommend that you go through [Introduction to Azure Data Factory](data-factory-introduction.md) article for a detailed overview of the service.</span></span>  

## <a name="create-data-factory"></a><span data-ttu-id="0b669-124">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="0b669-124">Create data factory</span></span>
<span data-ttu-id="0b669-125">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="0b669-125">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="0b669-126">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="0b669-126">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="0b669-127">Může obsahovat například aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="0b669-127">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="0b669-128">V tomto kroku začneme vytvořením objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-128">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="0b669-129">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0b669-129">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0b669-130">V nabídce vlevo klikněte na **NOVÝ**, klikněte na **Data + Analýza** a poté na **Objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="0b669-130">Click **NEW** on the left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>

   ![Okno Vytvořit](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. <span data-ttu-id="0b669-132">V okně **Nový objekt pro vytváření dat** zadejte název **GetStartedDF**.</span><span class="sxs-lookup"><span data-stu-id="0b669-132">In the **New data factory** blade, enter **GetStartedDF** for the Name.</span></span>

   ![Okno Nový objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > <span data-ttu-id="0b669-134">Název objektu pro vytváření dat Azure musí být **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="0b669-134">The name of the Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="0b669-135">Pokud se zobrazí chyba: **Název objektu pro vytváření dat GetStartedDF není k dispozici**,</span><span class="sxs-lookup"><span data-stu-id="0b669-135">If you receive the error: **Data factory name “GetStartedDF” is not available**.</span></span> <span data-ttu-id="0b669-136">změňte název objektu pro vytváření dat (např. na váš_název_GetStartedDF) a zkuste ho vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="0b669-136">Change the name of the data factory (for example, yournameGetStartedDF) and try creating again.</span></span> <span data-ttu-id="0b669-137">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-137">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
   >
   > <span data-ttu-id="0b669-138">Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název **DNS** a tak se stát veřejně viditelným.</span><span class="sxs-lookup"><span data-stu-id="0b669-138">The name of the data factory may be registered as a **DNS** name in the future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="0b669-139">Vyberte **předplatné Azure**, ve kterém chcete objekt pro vytváření dat vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0b669-139">Select the **Azure subscription** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="0b669-140">Vyberte existující **skupinu prostředků** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="0b669-140">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="0b669-141">Pro účely tohoto kurzu vytvořte skupinu prostředků s názvem **ADFGetStartedRG**.</span><span class="sxs-lookup"><span data-stu-id="0b669-141">For the tutorial, create a resource group named: **ADFGetStartedRG**.</span></span>
6. <span data-ttu-id="0b669-142">Vyberte **umístění** pro objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-142">Select the **location** for the data factory.</span></span> <span data-ttu-id="0b669-143">V rozevíracím seznamu jsou uvedené pouze oblasti podporované službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-143">Only regions supported by the Data Factory service are shown in the drop-down list.</span></span>
7. <span data-ttu-id="0b669-144">Zaškrtněte **Připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="0b669-144">Select **Pin to dashboard**.</span></span> 
8. <span data-ttu-id="0b669-145">V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0b669-145">Click **Create** on the **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="0b669-146">Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0b669-146">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="0b669-147">Na řídicím panelu vidíte následující dlaždice se statusem: Nasazování datové továrny.</span><span class="sxs-lookup"><span data-stu-id="0b669-147">On the dashboard, you see the following tile with status: Deploying data factory.</span></span>    

   ![Stav vytváření objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. <span data-ttu-id="0b669-149">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="0b669-149">Congratulations!</span></span> <span data-ttu-id="0b669-150">Úspěšně jste vytvořili první objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-150">You have successfully created your first data factory.</span></span> <span data-ttu-id="0b669-151">Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-151">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>     

    ![Okno Objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

<span data-ttu-id="0b669-153">Před vytvořením kanálu v objektu pro vytváření dat je nejdříve potřeba vytvořit několik entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-153">Before creating a pipeline in the data factory, you need to create a few Data Factory entities first.</span></span> <span data-ttu-id="0b669-154">Nejprve vytvoříte propojené služby, které propojí úložiště dat a výpočetní služby s vaším úložištěm dat, definujete vstupní a výstupní datové sady reprezentující vstupní a výstupní data v propojených úložištích dat, a poté vytvoříte kanál s aktivitou, která tyto datové sady používá.</span><span class="sxs-lookup"><span data-stu-id="0b669-154">You first create linked services to link data stores/computes to your data store, define input and output datasets to represent input/output data in linked data stores, and then create the pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="0b669-155">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="0b669-155">Create linked services</span></span>
<span data-ttu-id="0b669-156">V tomto kroku propojíte svůj účet služby Azure Storage a cluster Azure HDInsight na vyžádání s objektem pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-156">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster to your data factory.</span></span> <span data-ttu-id="0b669-157">Účet služby Azure Storage v této ukázce obsahuje vstupní a výstupní data pro kanál.</span><span class="sxs-lookup"><span data-stu-id="0b669-157">The Azure Storage account holds the input and output data for the pipeline in this sample.</span></span> <span data-ttu-id="0b669-158">Propojená služba HDInsight slouží v této ukázce ke spuštění skriptu Hive určeného v aktivitě kanálu.</span><span class="sxs-lookup"><span data-stu-id="0b669-158">The HDInsight linked service is used to run a Hive script specified in the activity of the pipeline in this sample.</span></span> <span data-ttu-id="0b669-159">Určete, jaké [úložiště dat](data-factory-data-movement-activities.md) a /[výpočetní služby](data-factory-compute-linked-services.md) se ve vašem scénáři používají, a vytvořením propojených služeb propojte tyto služby s objektem pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-159">Identify what [data store](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) are used in your scenario and link those services to the data factory by creating linked services.</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="0b669-160">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0b669-160">Create Azure Storage linked service</span></span>
<span data-ttu-id="0b669-161">V tomto kroku propojíte se svým objektem pro vytváření dat svůj účet služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0b669-161">In this step, you link your Azure Storage account to your data factory.</span></span> <span data-ttu-id="0b669-162">V tomto kurzu použijete tento účet služby Azure Storage také k uložení vstupních a výstupních dat a souboru skriptu HQL.</span><span class="sxs-lookup"><span data-stu-id="0b669-162">In this tutorial, you use the same Azure Storage account to store input/output data and the HQL script file.</span></span>

1. <span data-ttu-id="0b669-163">V okně **OBJEKT PRO VYTVÁŘENÍ DAT** pro objekt **GetStartedDF** klikněte na **Vytvořit a nasadit**.</span><span class="sxs-lookup"><span data-stu-id="0b669-163">Click **Author and deploy** on the **DATA FACTORY** blade for **GetStartedDF**.</span></span> <span data-ttu-id="0b669-164">Měli byste vidět editor služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-164">You should see the Data Factory Editor.</span></span>

   ![Dlaždice Vytvořit a nasadit](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. <span data-ttu-id="0b669-166">Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.</span><span class="sxs-lookup"><span data-stu-id="0b669-166">Click **New data store** and choose **Azure storage**.</span></span>

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="0b669-168">V editoru by se měl zobrazit skript JSON pro vytvoření propojené služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0b669-168">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="0b669-170">Nahraďte **název účtu** názvem účtu služby Azure Storage a **klíč účtu** přístupovým klíčem k účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0b669-170">Replace **account name** with the name of your Azure storage account and **account key** with the access key of the Azure storage account.</span></span> <span data-ttu-id="0b669-171">Chcete-li zjistit, jak získat přístupový klíč k úložišti, přečtěte si informace o zobrazení, kopírování a opětovném vygenerování přístupových klíčů k úložišti v tématu [Správa účtu úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="0b669-171">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="0b669-172">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0b669-172">Click **Deploy** on the command bar to deploy the linked service.</span></span>

    ![Tlačítko Nasadit](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   <span data-ttu-id="0b669-174">Po úspěšném nasazení propojené služby by mělo okno **Koncept-1** zmizet a v zobrazení stromu nalevo se zobrazí služba **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="0b669-174">After the linked service is deployed successfully, the **Draft-1** window should disappear and you see **AzureStorageLinkedService** in the tree view on the left.</span></span>

    ![Propojená služba úložiště v nabídce](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="0b669-176">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0b669-176">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="0b669-177">V tomto kroku propojíte se svým objektem pro vytváření dat cluster HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="0b669-177">In this step, you link an on-demand HDInsight cluster to your data factory.</span></span> <span data-ttu-id="0b669-178">Cluster HDInsight se automaticky vytvoří za běhu, a až dokončí zpracování, po určité zadané době nečinnosti se odstraní.</span><span class="sxs-lookup"><span data-stu-id="0b669-178">The HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for the specified amount of time.</span></span>

1. <span data-ttu-id="0b669-179">V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nový výpočet** a vyberte **Cluster HDInsight na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="0b669-179">In the **Data Factory Editor**, click **... More**, click **New compute**, and select **On-demand HDInsight cluster**.</span></span>

    ![Nový výpočet](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. <span data-ttu-id="0b669-181">Následující fragment kódu zkopírujte a vložte ho do okna **Koncept-1**.</span><span class="sxs-lookup"><span data-stu-id="0b669-181">Copy and paste the following snippet to the **Draft-1** window.</span></span> <span data-ttu-id="0b669-182">Tento fragment kódu JSON popisuje vlastnosti, které se použijí při vytváření clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="0b669-182">The JSON snippet describes the properties that are used to create the HDInsight cluster on-demand.</span></span>

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

    <span data-ttu-id="0b669-183">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="0b669-183">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

   | <span data-ttu-id="0b669-184">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0b669-184">Property</span></span> | <span data-ttu-id="0b669-185">Popis</span><span class="sxs-lookup"><span data-stu-id="0b669-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0b669-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="0b669-186">ClusterSize</span></span> |<span data-ttu-id="0b669-187">Určuje velikost clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0b669-187">Specifies the size of the HDInsight cluster.</span></span> |
   | <span data-ttu-id="0b669-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="0b669-188">TimeToLive</span></span> | <span data-ttu-id="0b669-189">Určuje dobu nečinnosti před odstraněním clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0b669-189">Specifies that the idle time for the HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="0b669-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="0b669-190">linkedServiceName</span></span> | <span data-ttu-id="0b669-191">Určuje účet úložiště, který se má použít k ukládání protokolů generovaných clusterem HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0b669-191">Specifies the storage account that is used to store the logs that are generated by HDInsight.</span></span> |

    <span data-ttu-id="0b669-192">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="0b669-192">Note the following points:</span></span>

   * <span data-ttu-id="0b669-193">Pomocí tohoto kódu JSON služba Data Factory vytvoří cluster HDInsight **se systémem Linux** za vás.</span><span class="sxs-lookup"><span data-stu-id="0b669-193">The Data Factory creates a **Linux-based** HDInsight cluster for you with the JSON.</span></span> <span data-ttu-id="0b669-194">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="0b669-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="0b669-195">Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="0b669-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="0b669-196">Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).</span><span class="sxs-lookup"><span data-stu-id="0b669-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="0b669-197">Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="0b669-197">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="0b669-198">Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b669-198">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="0b669-199">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="0b669-199">This behavior is by design.</span></span> <span data-ttu-id="0b669-200">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je zpracován určitý řez, pokud neexistuje aktivní cluster (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="0b669-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="0b669-201">Po dokončení zpracování se cluster automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="0b669-201">The cluster is automatically deleted when the processing is done.</span></span>

       <span data-ttu-id="0b669-202">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="0b669-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="0b669-203">Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b669-203">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="0b669-204">Názvy těchto kontejnerů používají následující formát: „adf**název_vašeho_objektu_pro_vytváření_dat**-**název_propojené_služby**-razítko_data_a_času“.</span><span class="sxs-lookup"><span data-stu-id="0b669-204">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="0b669-205">K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="0b669-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="0b669-206">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="0b669-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
3. <span data-ttu-id="0b669-207">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0b669-207">Click **Deploy** on the command bar to deploy the linked service.</span></span>

    ![Nasazení propojené služby HDInsight na vyžádání](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. <span data-ttu-id="0b669-209">Zkontrolujte, jestli se v zobrazení stromu vlevo zobrazuje služba **AzureStorageLinkedService** i služba **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="0b669-209">Confirm that you see both **AzureStorageLinkedService** and **HDInsightOnDemandLinkedService** in the tree view on the left.</span></span>

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a><span data-ttu-id="0b669-211">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="0b669-211">Create datasets</span></span>
<span data-ttu-id="0b669-212">V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data pro zpracování Hive.</span><span class="sxs-lookup"><span data-stu-id="0b669-212">In this step, you create datasets to represent the input and output data for Hive processing.</span></span> <span data-ttu-id="0b669-213">Tyto datové sady odkazují na službu **AzureStorageLinkedService**, kterou už jste v tomto kurzu vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0b669-213">These datasets refer to the **AzureStorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="0b669-214">Propojená služba odkazuje na účet služby Azure Storage a datové sady určují kontejner, složku a název souboru v úložišti, který obsahuje vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0b669-214">The linked service points to an Azure Storage account and datasets specify container, folder, file name in the storage that holds input and output data.</span></span>   

### <a name="create-input-dataset"></a><span data-ttu-id="0b669-215">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="0b669-215">Create input dataset</span></span>
1. <span data-ttu-id="0b669-216">V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nová datová sada** a vyberte **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="0b669-216">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>

    ![Nová datová sada](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. <span data-ttu-id="0b669-218">Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1.</span><span class="sxs-lookup"><span data-stu-id="0b669-218">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="0b669-219">V tomto fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="0b669-219">In the JSON snippet, you are creating a dataset called **AzureBlobInput** that represents input data for an activity in the pipeline.</span></span> <span data-ttu-id="0b669-220">Kromě toho určujete, že se vstupní data nachází v kontejneru objektů blob s názvem **adfgetstarted** ve složce s názvem **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="0b669-220">In addition, you specify that the input data is located in the blob container called **adfgetstarted** and the folder called **inputdata**.</span></span>

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
    <span data-ttu-id="0b669-221">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="0b669-221">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

   | <span data-ttu-id="0b669-222">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0b669-222">Property</span></span> | <span data-ttu-id="0b669-223">Popis</span><span class="sxs-lookup"><span data-stu-id="0b669-223">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0b669-224">type</span><span class="sxs-lookup"><span data-stu-id="0b669-224">type</span></span> |<span data-ttu-id="0b669-225">Vlastnost type je nastavená na hodnotu **AzureBlob**, protože se data nachází ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0b669-225">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
   | <span data-ttu-id="0b669-226">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="0b669-226">linkedServiceName</span></span> |<span data-ttu-id="0b669-227">Odkazuje na službu **AzureStorageLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="0b669-227">Refers to the **AzureStorageLinkedService** you created earlier.</span></span> |
   | <span data-ttu-id="0b669-228">folderPath</span><span class="sxs-lookup"><span data-stu-id="0b669-228">folderPath</span></span> | <span data-ttu-id="0b669-229">Určuje **kontejner** objektů blob a **složku** obsahující vstupní objekty blob.</span><span class="sxs-lookup"><span data-stu-id="0b669-229">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> | 
   | <span data-ttu-id="0b669-230">fileName</span><span class="sxs-lookup"><span data-stu-id="0b669-230">fileName</span></span> |<span data-ttu-id="0b669-231">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="0b669-231">This property is optional.</span></span> <span data-ttu-id="0b669-232">Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath.</span><span class="sxs-lookup"><span data-stu-id="0b669-232">If you omit this property, all the files from the folderPath are picked.</span></span> <span data-ttu-id="0b669-233">V tomto kurzu se zpracovává jenom soubor **input.log**.</span><span class="sxs-lookup"><span data-stu-id="0b669-233">In this tutorial, only the **input.log** is processed.</span></span> |
   | <span data-ttu-id="0b669-234">type</span><span class="sxs-lookup"><span data-stu-id="0b669-234">type</span></span> |<span data-ttu-id="0b669-235">Soubory protokolů jsou v textovém formátu, takže použijeme hodnotu **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="0b669-235">The log files are in text format, so we use **TextFormat**.</span></span> |
   | <span data-ttu-id="0b669-236">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="0b669-236">columnDelimiter</span></span> |<span data-ttu-id="0b669-237">Sloupce v souborech protokolu jsou oddělené **znakem čárky (`,`)**</span><span class="sxs-lookup"><span data-stu-id="0b669-237">columns in the log files are delimited by **comma character (`,`)**</span></span> |
   | <span data-ttu-id="0b669-238">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="0b669-238">frequency/interval</span></span> |<span data-ttu-id="0b669-239">Frekvence je nastavená na hodnotu **Month** (Měsíc) a interval je **1**, takže vstupní řezy jsou dostupné jednou za měsíc.</span><span class="sxs-lookup"><span data-stu-id="0b669-239">frequency set to **Month** and interval is **1**, which means that the input slices are available monthly.</span></span> |
   | <span data-ttu-id="0b669-240">external</span><span class="sxs-lookup"><span data-stu-id="0b669-240">external</span></span> | <span data-ttu-id="0b669-241">Pokud vstupní data nevygeneroval tento kanál, je tato vlastnost nastavená na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="0b669-241">This property is set to **true** if the input data is not generated by this pipeline.</span></span> <span data-ttu-id="0b669-242">V tomto kurzu se soubor input.log pomocí tohoto kanálu negeneruje, takže tuto vlastnost nastavíme na true.</span><span class="sxs-lookup"><span data-stu-id="0b669-242">In this tutorial, the input.log file is not generated by this pipeline, so we set the property to true.</span></span> |

    <span data-ttu-id="0b669-243">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0b669-243">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="0b669-244">Nově vytvořenou datovou sadu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0b669-244">Click **Deploy** on the command bar to deploy the newly created dataset.</span></span> <span data-ttu-id="0b669-245">Datová sada by se měla objevit v zobrazení stromu vlevo.</span><span class="sxs-lookup"><span data-stu-id="0b669-245">You should see the dataset in the tree view on the left.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="0b669-246">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="0b669-246">Create output dataset</span></span>
<span data-ttu-id="0b669-247">Nyní vytvoříte výstupní datovou sadu, která bude představovat výstupní data ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0b669-247">Now, you create the output dataset to represent the output data stored in the Azure Blob storage.</span></span>

1. <span data-ttu-id="0b669-248">V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nová datová sada** a vyberte **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="0b669-248">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="0b669-249">Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1.</span><span class="sxs-lookup"><span data-stu-id="0b669-249">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="0b669-250">V tomto fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobOutput** a určujete strukturu dat vytvářených skriptem Hive.</span><span class="sxs-lookup"><span data-stu-id="0b669-250">In the JSON snippet, you are creating a dataset called **AzureBlobOutput**, and specifying the structure of the data that is produced by the Hive script.</span></span> <span data-ttu-id="0b669-251">Kromě toho určujete, že se mají výsledky ukládat do kontejneru objektů blob s názvem **adfgetstarted** do složky s názvem **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="0b669-251">In addition, you specify that the results are stored in the blob container called **adfgetstarted** and the folder called **partitioneddata**.</span></span> <span data-ttu-id="0b669-252">Oddíl **availability** určuje, že se výstupní sada vytváří jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="0b669-252">The **availability** section specifies that the output dataset is produced on a monthly basis.</span></span>

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
    <span data-ttu-id="0b669-253">Popisy těchto vlastností najdete v části **Vytvoření vstupní datové sady**.</span><span class="sxs-lookup"><span data-stu-id="0b669-253">See **Create the input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="0b669-254">U výstupní datové sady nenajdete vlastnost external, protože datovou sadu vytváří služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-254">You do not set the external property on an output dataset as the dataset is produced by the Data Factory service.</span></span>
3. <span data-ttu-id="0b669-255">Nově vytvořenou datovou sadu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0b669-255">Click **Deploy** on the command bar to deploy the newly created dataset.</span></span>
4. <span data-ttu-id="0b669-256">Zkontrolujte, jestli se datová sada úspěšně vytvořila.</span><span class="sxs-lookup"><span data-stu-id="0b669-256">Verify that the dataset is created successfully.</span></span>

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a><span data-ttu-id="0b669-258">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="0b669-258">Create pipeline</span></span>
<span data-ttu-id="0b669-259">V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="0b669-259">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="0b669-260">Vstupní řez je dostupný jednou měsíčně (frequency: Month, interval: 1), výstupní řez se vytváří také jednou měsíčně a vlastnost scheduler pro aktivitu je také nastavena na jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="0b669-260">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and the scheduler property for the activity is also set to monthly.</span></span> <span data-ttu-id="0b669-261">Nastavení výstupní datové sady a vlastnosti scheduler se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="0b669-261">The settings for the output dataset and the activity scheduler must match.</span></span> <span data-ttu-id="0b669-262">V současnosti určuje plán výstupní datová sada, takže musíte výstupní datovou sadu vytvořit i v případě, že aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="0b669-262">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="0b669-263">Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte.</span><span class="sxs-lookup"><span data-stu-id="0b669-263">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> <span data-ttu-id="0b669-264">Vysvětlení vlastností použitých v následujícím kódu JSON najdete na konci této části.</span><span class="sxs-lookup"><span data-stu-id="0b669-264">The properties used in the following JSON are explained at the end of this section.</span></span>

1. <span data-ttu-id="0b669-265">V **editoru služby Data Factory** klikněte na **(…) Další příkazy** a poté klikněte na **Nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="0b669-265">In the **Data Factory Editor**, click **Ellipsis (…) More commands** and then click **New pipeline**.</span></span>

    ![Tlačítko Nový kanál](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. <span data-ttu-id="0b669-267">Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1.</span><span class="sxs-lookup"><span data-stu-id="0b669-267">Copy and paste the following snippet to the Draft-1 window.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="0b669-268">V kódu JSON nahraďte **storageaccountname** názvem vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b669-268">Replace **storageaccountname** with the name of your storage account in the JSON.</span></span>
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
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
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    <span data-ttu-id="0b669-269">V tomto fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, která zpracovává data v clusteru HDInsight pomocí skriptu Hive.</span><span class="sxs-lookup"><span data-stu-id="0b669-269">In the JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive to process Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="0b669-270">Soubor skriptu Hive **partitionweblogs.hql** je uložený v účtu služby Azure Storage (který určuje služba scriptLinkedService s názvem **AzureStorageLinkedService**) a ve složce **script** v kontejneru **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="0b669-270">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in the container **adfgetstarted**.</span></span>

    <span data-ttu-id="0b669-271">Oddíl **defines** určuje nastavení běhového prostředí, které se předá skriptu Hive jako konfigurační hodnoty Hive (např. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span><span class="sxs-lookup"><span data-stu-id="0b669-271">The **defines** section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="0b669-272">Vlastnosti kanálu **start** a **end** určují období aktivity kanálu.</span><span class="sxs-lookup"><span data-stu-id="0b669-272">The **start** and **end** properties of the pipeline specifies the active period of the pipeline.</span></span>

    <span data-ttu-id="0b669-273">V kódu JSON aktivity určujete, že má skript Hive běžet ve výpočetní službě určené vlastností **linkedServiceName**, tedy **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="0b669-273">In the activity JSON, you specify that the Hive script runs on the compute specified by the **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0b669-274">V části kódu JSON kanálu v tématu [Kanály a aktivity ve službě Azure Data Factory](data-factory-create-pipelines.md) najdete podrobnosti o vlastnostech JSON použitých v příkladu.</span><span class="sxs-lookup"><span data-stu-id="0b669-274">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in the example.</span></span>
   >
   >
3. <span data-ttu-id="0b669-275">Zkontrolujte:</span><span class="sxs-lookup"><span data-stu-id="0b669-275">Confirm the following:</span></span>

   1. <span data-ttu-id="0b669-276">Jestli ve složce **inputdata** v kontejneru **adfgetstarted** v úložišti objektů blob Azure existuje soubor **input.log**.</span><span class="sxs-lookup"><span data-stu-id="0b669-276">**input.log** file exists in the **inputdata** folder of the **adfgetstarted** container in the Azure blob storage</span></span>
   2. <span data-ttu-id="0b669-277">Jestli ve složce **script** v kontejneru **adfgetstarted** v úložišti objektů blob Azure existuje soubor **partitionweblogs.hql**.</span><span class="sxs-lookup"><span data-stu-id="0b669-277">**partitionweblogs.hql** file exists in the **script** folder of the **adfgetstarted** container in the Azure blob storage.</span></span> <span data-ttu-id="0b669-278">Pokud tyto soubory nevidíte, proveďte požadované kroky uvedené v článku [Přehled kurzu](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="0b669-278">Complete the prerequisite steps in the [Tutorial Overview](data-factory-build-your-first-pipeline.md) if you don't see these files.</span></span>
   3. <span data-ttu-id="0b669-279">Ujistěte se, že jste položku **storageaccountname** v kódu JSON kanálu nahradili názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b669-279">Confirm that you replaced **storageaccountname** with the name of your storage account in the pipeline JSON.</span></span>
4. <span data-ttu-id="0b669-280">Kanál nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0b669-280">Click **Deploy** on the command bar to deploy the pipeline.</span></span> <span data-ttu-id="0b669-281">Časy **start** a **end** jsou nastavené na minulost a vlastnost **isPaused** má hodnotu false, takže se kanál (aktivita v kanálu) spustí hned po nasazení.</span><span class="sxs-lookup"><span data-stu-id="0b669-281">Since the **start** and **end** times are set in the past and **isPaused** is set to false, the pipeline (activity in the pipeline) runs immediately after you deploy.</span></span>
5. <span data-ttu-id="0b669-282">Zkontrolujte, jestli se kanál objevil v zobrazení stromu.</span><span class="sxs-lookup"><span data-stu-id="0b669-282">Confirm that you see the pipeline in the tree view.</span></span>

    ![Zobrazení stromu s kanálem](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. <span data-ttu-id="0b669-284">Úspěšně jste vytvořili první kanál, blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="0b669-284">Congratulations, you have successfully created your first pipeline!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="0b669-285">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="0b669-285">Monitor pipeline</span></span>
### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="0b669-286">Monitorování kanálu pomocí Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="0b669-286">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="0b669-287">Kliknutím na **X** zavřete editor služby Data Factory a vrátíte se zpátky do okna Objekt pro vytváření dat. Tam klikněte na **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="0b669-287">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory blade, and click **Diagram**.</span></span>

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. <span data-ttu-id="0b669-289">V zobrazení diagramu uvidíte přehled kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0b669-289">In the Diagram View, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. <span data-ttu-id="0b669-291">Pokud chcete zobrazit všechny aktivity v kanálu, klikněte na kanál v diagramu pravým tlačítkem myši a potom klikněte na Otevřít kanál.</span><span class="sxs-lookup"><span data-stu-id="0b669-291">To view all activities in the pipeline, right-click pipeline in the diagram and click Open Pipeline.</span></span>

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. <span data-ttu-id="0b669-293">Zkontrolujte, jestli je v kanálu vidět aktivita HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="0b669-293">Confirm that you see the HDInsightHive activity in the pipeline.</span></span>

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    <span data-ttu-id="0b669-295">Pokud se chcete vrátit do předchozího zobrazení, klikněte v nabídce navigace s popisem cesty v horní části na **Objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="0b669-295">To navigate back to the previous view, click **Data factory** in the breadcrumb menu at the top.</span></span>
5. <span data-ttu-id="0b669-296">V **zobrazení diagramu** dvakrát klikněte na datovou sadu **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="0b669-296">In the **Diagram View**, double-click the dataset **AzureBlobInput**.</span></span> <span data-ttu-id="0b669-297">Zkontrolujte, jestli je řez ve stavu **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="0b669-297">Confirm that the slice is in **Ready** state.</span></span> <span data-ttu-id="0b669-298">Než se řez zobrazí ve stavu Připraveno, může to několik minut trvat.</span><span class="sxs-lookup"><span data-stu-id="0b669-298">It may take a couple of minutes for the slice to show up in Ready state.</span></span> <span data-ttu-id="0b669-299">Pokud se to do nějaké doby nestane, zkontrolujte, jestli je vstupní soubor (input.log) umístěný ve správném kontejneru (adfgetstarted) a složce (inputdata).</span><span class="sxs-lookup"><span data-stu-id="0b669-299">If it does not happen after you wait for sometime, see if you have the input file (input.log) placed in the right container (adfgetstarted) and folder (inputdata).</span></span>

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. <span data-ttu-id="0b669-301">Kliknutím na tlačítko **X** zavřete okno **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="0b669-301">Click **X** to close **AzureBlobInput** blade.</span></span>
7. <span data-ttu-id="0b669-302">V **zobrazení diagramu** dvakrát klikněte na datovou sadu **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="0b669-302">In the **Diagram View**, double-click the dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="0b669-303">Zobrazí se řez, který se právě zpracovává.</span><span class="sxs-lookup"><span data-stu-id="0b669-303">You see that the slice that is currently being processed.</span></span>

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. <span data-ttu-id="0b669-305">Po dokončení zpracování bude řez ve stavu **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="0b669-305">When processing is done, you see the slice in **Ready** state.</span></span>

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > <span data-ttu-id="0b669-307">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="0b669-307">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="0b669-308">Proto počítejte s tím, že zpracování řezu kanálem bude trvat **přibližně 30 minut**.</span><span class="sxs-lookup"><span data-stu-id="0b669-308">Therefore, expect the pipeline to       take **approximately 30 minutes** to process the slice.</span></span>
   >
   >

9. <span data-ttu-id="0b669-309">Pokud je řez ve stavu **Připraveno**, zkontrolujte, jestli se ve složce **partitioneddata** v kontejneru **adfgetstarted** ve službě Blob Storage nachází výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0b669-309">When the slice is in **Ready** state, check the **partitioneddata** folder in the **adfgetstarted** container in your blob storage for the output data.</span></span>  

   ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. <span data-ttu-id="0b669-311">Kliknutím na řez otevřete okno **Datový řez** s podrobnostmi o řezu.</span><span class="sxs-lookup"><span data-stu-id="0b669-311">Click the slice to see details about it in a **Data slice** blade.</span></span>

   ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. <span data-ttu-id="0b669-313">Kliknutím na spuštění aktivity v **seznamu spuštění aktivit** zobrazíte podrobnosti o spuštění aktivity (v našem scénáři aktivity Hivu) v okně **Podrobnosti o spuštění aktivit**.</span><span class="sxs-lookup"><span data-stu-id="0b669-313">Click an activity run in the **Activity runs list** to see details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span>   

   ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   <span data-ttu-id="0b669-315">V souborech protokolů můžete zobrazit provedený dotaz Hivu s informacemi o jeho stavu.</span><span class="sxs-lookup"><span data-stu-id="0b669-315">From the log files, you can see the Hive query that was executed and status information.</span></span> <span data-ttu-id="0b669-316">Tyto protokoly jsou užitečné při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0b669-316">These logs are useful for troubleshooting any issues.</span></span>
   <span data-ttu-id="0b669-317">Další podrobnosti najdete v článku [Monitorování a správa kanálů pomocí oken Azure Portal](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="0b669-317">See [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) article for more details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b669-318">Po úspěšném zpracování řezu se vstupní soubor odstraní.</span><span class="sxs-lookup"><span data-stu-id="0b669-318">The input file gets deleted when the slice is processed successfully.</span></span> <span data-ttu-id="0b669-319">Pokud tedy chcete spustit řez znovu nebo si znovu projít tento kurz, načtěte vstupní soubor (input.log) do složky inputdata v kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="0b669-319">Therefore, if you want to rerun the slice or do the tutorial again, upload the input file (input.log) to the inputdata folder of the adfgetstarted container.</span></span>
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="0b669-320">Monitorování kanálu pomocí aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="0b669-320">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="0b669-321">K monitorování kanálů můžete také použít aplikaci pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="0b669-321">You can also use Monitor & Manage application to monitor your pipelines.</span></span> <span data-ttu-id="0b669-322">Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="0b669-322">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="0b669-323">Na domovské stránce svého objektu pro vytváření dat klikněte na dlaždici **Monitorování a správa**.</span><span class="sxs-lookup"><span data-stu-id="0b669-323">Click **Monitor & Manage** tile on the home page for your data factory.</span></span>

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. <span data-ttu-id="0b669-325">Měla by se zobrazit **aplikace pro monitorování a správu**.</span><span class="sxs-lookup"><span data-stu-id="0b669-325">You should see **Monitor & Manage application**.</span></span> <span data-ttu-id="0b669-326">Změňte hodnoty **Čas spuštění** a **Čas ukončení** tak, aby odpovídaly času spuštění a ukončení vašeho kanálu, a klikněte na **Použít**.</span><span class="sxs-lookup"><span data-stu-id="0b669-326">Change the **Start time** and **End time** to match start and end times of your pipeline, and click **Apply**.</span></span>

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. <span data-ttu-id="0b669-328">Výběrem okna aktivity v seznamu **Okna aktivit** zobrazíte podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0b669-328">Select an activity window in the **Activity Windows** list to see details about it.</span></span>

    ![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a><span data-ttu-id="0b669-330">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0b669-330">Summary</span></span>
<span data-ttu-id="0b669-331">V tomto kurzu jste vytvořili objekt pro zpracování dat Azure, který zpracovává data pomocí skriptu Hive v clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="0b669-331">In this tutorial, you created an Azure data factory to process data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="0b669-332">Pomocí editoru služby Data Factory na webu Azure Portal jste provedli tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="0b669-332">You used the Data Factory Editor in the Azure portal to do the following steps:</span></span>  

1. <span data-ttu-id="0b669-333">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="0b669-333">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="0b669-334">Vytvořili jste dvě **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="0b669-334">Created two **linked services**:</span></span>
   1. <span data-ttu-id="0b669-335">Propojená služba **Azure Storage** spojuje úložiště objektů blob Azure, které obsahuje vstupní/výstupní soubory, s objektem pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-335">**Azure Storage** linked service to link your Azure blob storage that holds input/output files to the data factory.</span></span>
   2. <span data-ttu-id="0b669-336">Propojená služba na vyžádání **Azure HDInsight** spojuje cluster na vyžádání HDInsight Hadoop s objektem pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0b669-336">**Azure HDInsight** on-demand linked service to link an on-demand HDInsight Hadoop cluster to the data factory.</span></span> <span data-ttu-id="0b669-337">Služba Azure Data Factory vytvoří cluster HDInsight Hadoop ve chvíli, kdy je potřeba zpracovat vstupní data a vygenerovat výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0b669-337">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time to process input data and produce output data.</span></span>
3. <span data-ttu-id="0b669-338">Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu.</span><span class="sxs-lookup"><span data-stu-id="0b669-338">Created two **datasets**, which describe input and output data for HDInsight Hive activity in the pipeline.</span></span>
4. <span data-ttu-id="0b669-339">Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="0b669-339">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b669-340">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b669-340">Next Steps</span></span>
<span data-ttu-id="0b669-341">V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive.</span><span class="sxs-lookup"><span data-stu-id="0b669-341">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="0b669-342">Pokud chcete zjistit, jak pomocí aktivity kopírování zkopírovat data z Azure Blob do Azure SQL, projděte si článek [Kurz: Kopírování dat z Azure Blob do Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0b669-342">To see how to use a Copy Activity to copy data from an Azure Blob to Azure SQL, see [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="0b669-343">Viz také</span><span class="sxs-lookup"><span data-stu-id="0b669-343">See Also</span></span>
| <span data-ttu-id="0b669-344">Téma</span><span class="sxs-lookup"><span data-stu-id="0b669-344">Topic</span></span> | <span data-ttu-id="0b669-345">Popis</span><span class="sxs-lookup"><span data-stu-id="0b669-345">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="0b669-346">Kanály</span><span class="sxs-lookup"><span data-stu-id="0b669-346">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="0b669-347">Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech.</span><span class="sxs-lookup"><span data-stu-id="0b669-347">This article helps you understand pipelines and activities in Azure Data Factory and how to use them to construct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="0b669-348">Datové sady</span><span class="sxs-lookup"><span data-stu-id="0b669-348">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="0b669-349">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-349">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="0b669-350">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="0b669-350">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="0b669-351">Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0b669-351">This article explains the scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="0b669-352">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="0b669-352">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="0b669-353">Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="0b669-353">This article describes how to monitor, manage, and debug pipelines using the Monitoring & Management App.</span></span> |
