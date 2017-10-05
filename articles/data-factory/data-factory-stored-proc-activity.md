---
title: "Systému SQL Server uložené procedury aktivity"
description: "Zjistěte, jak můžete pomocí SQL Server aktivity uložené procedury vyvolat uloženou proceduru v databázi SQL Azure nebo Azure SQL Data Warehouse z objektu pro vytváření dat kanál."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: spelluru
ms.openlocfilehash: 6505d9aa2c7ae003bd928e2fa82cd923a9615394
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="sql-server-stored-procedure-activity"></a><span data-ttu-id="38c49-103">Systému SQL Server uložené procedury aktivity</span><span class="sxs-lookup"><span data-stu-id="38c49-103">SQL Server Stored Procedure Activity</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="38c49-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="38c49-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="38c49-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="38c49-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="38c49-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="38c49-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="38c49-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="38c49-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="38c49-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="38c49-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="38c49-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="38c49-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="38c49-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="38c49-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="38c49-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="38c49-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="38c49-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="38c49-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="38c49-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="38c49-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="overview"></a><span data-ttu-id="38c49-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="38c49-114">Overview</span></span>
<span data-ttu-id="38c49-115">Pomocí aktivit transformace dat v datové továrně [kanálu](data-factory-create-pipelines.md) k transformaci a zpracování nezpracovaných dat do předpovědi a statistiky.</span><span class="sxs-lookup"><span data-stu-id="38c49-115">You use data transformation activities in a Data Factory [pipeline](data-factory-create-pipelines.md) to transform and process raw data into predictions and insights.</span></span> <span data-ttu-id="38c49-116">Aktivita uložené procedury je jedním z aktivit transformace, které podporuje služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="38c49-116">The Stored Procedure Activity is one of the transformation activities that Data Factory supports.</span></span> <span data-ttu-id="38c49-117">Tento článek vychází [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který představuje Obecné přehled o transformaci dat a aktivity podporované transformace v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="38c49-117">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities in Data Factory.</span></span>

<span data-ttu-id="38c49-118">Aktivita uložené procedury můžete vyvolat uloženou proceduru v jednom z následujících úložiště dat ve vašem podniku nebo na virtuální počítač Azure (VM):</span><span class="sxs-lookup"><span data-stu-id="38c49-118">You can use the Stored Procedure Activity to invoke a stored procedure in one of the following data stores in your enterprise or on an Azure virtual machine (VM):</span></span> 

- <span data-ttu-id="38c49-119">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="38c49-119">Azure SQL Database</span></span>
- <span data-ttu-id="38c49-120">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="38c49-120">Azure SQL Data Warehouse</span></span>
- <span data-ttu-id="38c49-121">Databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="38c49-121">SQL Server Database.</span></span>  <span data-ttu-id="38c49-122">Pokud používáte systém SQL Server, nainstalujte Brána pro správu dat na stejném počítači, který je hostitelem databáze nebo na samostatný počítač, který má přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="38c49-122">If you are using SQL Server, install Data Management Gateway on the same machine that hosts the database or on a separate machine that has access to the database.</span></span> <span data-ttu-id="38c49-123">Brána pro správu dat je, že komponenty, která se připojuje data způsobem, zabezpečení a správě zdroje na místní nebo na virtuální počítač Azure s cloudovými službami.</span><span class="sxs-lookup"><span data-stu-id="38c49-123">Data Management Gateway is a component that connects data sources on-premises/on Azure VM with cloud services in a secure and managed way.</span></span> <span data-ttu-id="38c49-124">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článku.</span><span class="sxs-lookup"><span data-stu-id="38c49-124">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="38c49-125">Při kopírování dat do Azure SQL Database nebo SQL Server, můžete nakonfigurovat **SqlSink** v aktivitě kopírování k vyvolání uložené procedury pomocí **sqlWriterStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="38c49-125">When copying data into Azure SQL Database or SQL Server, you can configure the **SqlSink** in copy activity to invoke a stored procedure by using the **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="38c49-126">Další informace najdete v tématu [vyvolat uloženou proceduru z aktivity kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="38c49-126">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="38c49-127">Podrobnosti o vlastnosti, viz následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="38c49-127">For details about the property, see following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span> <span data-ttu-id="38c49-128">Volání uložené procedury při kopírování dat do Azure SQL Data Warehouse pomocí aktivity kopírování není podporována.</span><span class="sxs-lookup"><span data-stu-id="38c49-128">Invoking a stored procedure while copying data into an Azure SQL Data Warehouse by using a copy activity is not supported.</span></span> <span data-ttu-id="38c49-129">Ale aktivity uložené procedury můžete vyvolat uloženou proceduru v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="38c49-129">But, you can use the stored procedure activity to invoke a stored procedure in a SQL Data Warehouse.</span></span> 
>  
> <span data-ttu-id="38c49-130">Při kopírování dat z Azure SQL Database nebo SQL Server nebo Azure SQL Data Warehouse, můžete nakonfigurovat **SqlSource** v aktivitě kopírování vyvolat uloženou proceduru čtení dat ze zdrojové databáze pomocí  **sqlReaderStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="38c49-130">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity to invoke a stored procedure to read data from the source database by using the **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="38c49-131">Další informace naleznete v následujících článcích konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="38c49-131">For more information, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          


<span data-ttu-id="38c49-132">Následující postup používá aktivity uložené procedury v kanálu vyvolat uloženou proceduru v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="38c49-132">The following walkthrough uses the Stored Procedure Activity in a pipeline to invoke a stored procedure in an Azure SQL database.</span></span> 

## <a name="walkthrough"></a><span data-ttu-id="38c49-133">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="38c49-133">Walkthrough</span></span>
### <a name="sample-table-and-stored-procedure"></a><span data-ttu-id="38c49-134">Ukázkové tabulky a uložené procedury</span><span class="sxs-lookup"><span data-stu-id="38c49-134">Sample table and stored procedure</span></span>
1. <span data-ttu-id="38c49-135">Vytvořte následující **tabulky** ve vaší databázi SQL Azure pomocí SQL Server Management Studio nebo jakýkoli jiný nástroj, který jste obeznámeni s.</span><span class="sxs-lookup"><span data-stu-id="38c49-135">Create the following **table** in your Azure SQL Database using SQL Server Management Studio or any other tool you are comfortable with.</span></span> <span data-ttu-id="38c49-136">Sloupec razítko_data_a_času je datum a čas, kdy se vygeneruje odpovídající ID.</span><span class="sxs-lookup"><span data-stu-id="38c49-136">The datetimestamp column is the date and time when the corresponding ID is generated.</span></span>

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    <span data-ttu-id="38c49-137">ID je jedinečný identifikovat a sloupec razítko_data_a_času je datum a čas, kdy se vygeneruje odpovídající ID.</span><span class="sxs-lookup"><span data-stu-id="38c49-137">Id is the unique identified and the datetimestamp column is the date and time when the corresponding ID is generated.</span></span>
    
    ![Ukázková data](./media/data-factory-stored-proc-activity/sample-data.png)

    <span data-ttu-id="38c49-139">V této ukázce je uloženou proceduru v databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="38c49-139">In this sample, the stored procedure is in an Azure SQL Database.</span></span> <span data-ttu-id="38c49-140">Pokud uložená procedura je do Azure SQL Data Warehouse a databáze systému SQL Server, je podobný přístupu.</span><span class="sxs-lookup"><span data-stu-id="38c49-140">If the stored procedure is in an Azure SQL Data Warehouse and SQL Server Database, the approach is similar.</span></span> <span data-ttu-id="38c49-141">Pro databázi systému SQL Server, musíte nainstalovat [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="38c49-141">For a SQL Server database, you must install a [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>
2. <span data-ttu-id="38c49-142">Vytvořte následující **uložené procedury** který vkládá data v do **sampletable**.</span><span class="sxs-lookup"><span data-stu-id="38c49-142">Create the following **stored procedure** that inserts data in to the **sampletable**.</span></span>

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="38c49-143">**Název** a **velká a malá písmena** parametru (data a času v tomto příkladu) musí odpovídat parametr zadaný v kódu JSON kanálu nebo aktivity.</span><span class="sxs-lookup"><span data-stu-id="38c49-143">**Name** and **casing** of the parameter (DateTime in this example) must match that of parameter specified in the pipeline/activity JSON.</span></span> <span data-ttu-id="38c49-144">V definici uloženou proceduru, ujistěte se, že  **@**  slouží jako předpona pro parametr.</span><span class="sxs-lookup"><span data-stu-id="38c49-144">In the stored procedure definition, ensure that **@** is used as a prefix for the parameter.</span></span>

### <a name="create-a-data-factory"></a><span data-ttu-id="38c49-145">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="38c49-145">Create a data factory</span></span>
1. <span data-ttu-id="38c49-146">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="38c49-146">Log in to [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="38c49-147">Klikněte na tlačítko **nový** v levé nabídce klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="38c49-147">Click **NEW** on the left menu, click **Intelligence + Analytics**, and click **Data Factory**.</span></span>

    ![Nový objekt pro vytváření dat](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. <span data-ttu-id="38c49-149">V **nový objekt pro vytváření dat** okno, zadejte **SProcDF** pro název.</span><span class="sxs-lookup"><span data-stu-id="38c49-149">In the **New data factory** blade, enter **SProcDF** for the Name.</span></span> <span data-ttu-id="38c49-150">Azure Data Factory názvů **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="38c49-150">Azure Data Factory names are **globally unique**.</span></span> <span data-ttu-id="38c49-151">Budete muset předponu názvu objektu pro vytváření dat s názvem, pokud chcete povolit úspěšné vytvoření objektu pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="38c49-151">You need to prefix the name of the data factory with your name, to enable the successful creation of the factory.</span></span>

   ![Nový objekt pro vytváření dat](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. <span data-ttu-id="38c49-153">Vyberte vaše **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="38c49-153">Select your **Azure subscription**.</span></span>
5. <span data-ttu-id="38c49-154">Pro **skupiny prostředků**, proveďte jednu z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="38c49-154">For **Resource Group**, do one of the following steps:</span></span>
   1. <span data-ttu-id="38c49-155">Klikněte na tlačítko **vytvořit nový** a zadejte název pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="38c49-155">Click **Create new** and enter a name for the resource group.</span></span>
   2. <span data-ttu-id="38c49-156">Klikněte na tlačítko **použít existující** a vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="38c49-156">Click **Use existing** and select an existing resource group.</span></span>  
6. <span data-ttu-id="38c49-157">Vyberte **umístění** pro objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="38c49-157">Select the **location** for the data factory.</span></span>
7. <span data-ttu-id="38c49-158">Vyberte **připnout na řídicí panel** , aby mohli zobrazit objektu pro vytváření dat na řídicím panelu při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="38c49-158">Select **Pin to dashboard** so that you can see the data factory on the dashboard next time you log in.</span></span>
8. <span data-ttu-id="38c49-159">V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38c49-159">Click **Create** on the **New data factory** blade.</span></span>
9. <span data-ttu-id="38c49-160">Zobrazí objektu pro vytváření dat vytváří ve **řídicí panel** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="38c49-160">You see the data factory being created in the **dashboard** of the Azure portal.</span></span> <span data-ttu-id="38c49-161">Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="38c49-161">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>

   ![Domovská stránka objektu pro vytváření dat](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a><span data-ttu-id="38c49-163">Vytvoření služby Azure SQL propojené</span><span class="sxs-lookup"><span data-stu-id="38c49-163">Create an Azure SQL linked service</span></span>
<span data-ttu-id="38c49-164">Po vytvoření objektu pro vytváření dat, Azure SQL vytvoříte propojené služby, který odkazuje na databázi Azure SQL, který obsahuje tabulku sampletable a sp_sample uložené procedury, do data factory.</span><span class="sxs-lookup"><span data-stu-id="38c49-164">After creating the data factory, you create an Azure SQL linked service that links your Azure SQL database, which contains the sampletable table and sp_sample stored procedure, to your data factory.</span></span>

1. <span data-ttu-id="38c49-165">Klikněte na tlačítko **autora a nasazení** na **objekt pro vytváření dat** okně **SProcDF** ke spuštění editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="38c49-165">Click **Author and deploy** on the **Data Factory** blade for **SProcDF** to launch the Data Factory Editor.</span></span>
2. <span data-ttu-id="38c49-166">Klikněte na tlačítko **nové datové úložiště** na příkaz panelu a vyberte **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="38c49-166">Click **New data store** on the command bar and choose **Azure SQL Database**.</span></span> <span data-ttu-id="38c49-167">Měli byste vidět skript JSON pro vytvoření služby Azure SQL propojené v editoru.</span><span class="sxs-lookup"><span data-stu-id="38c49-167">You should see the JSON script for creating an Azure SQL linked service in the editor.</span></span>

   ![Nové úložiště dat](media/data-factory-stored-proc-activity/new-data-store.png)
3. <span data-ttu-id="38c49-169">Ve skriptu JSON proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="38c49-169">In the JSON script, make the following changes:</span></span>

   1. <span data-ttu-id="38c49-170">Nahraďte `<servername>` s názvem serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="38c49-170">Replace `<servername>` with the name of your Azure SQL Database server.</span></span>
   2. <span data-ttu-id="38c49-171">Nahraďte `<databasename>` s databází, ve které jste vytvořili, tabulky a uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-171">Replace `<databasename>` with the database in which you created the table and the stored procedure.</span></span>
   3. <span data-ttu-id="38c49-172">Nahraďte `<username@servername>` pomocí uživatelského účtu, který má přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="38c49-172">Replace `<username@servername>` with the user account that has access to the database.</span></span>
   4. <span data-ttu-id="38c49-173">Nahraďte `<password>` s heslem pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="38c49-173">Replace `<password>` with the password for the user account.</span></span>

      ![Nové úložiště dat](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. <span data-ttu-id="38c49-175">Chcete-li nasadit propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="38c49-175">To deploy the linked service, click **Deploy** on the command bar.</span></span> <span data-ttu-id="38c49-176">Zkontrolujte, jestli AzureSqlLinkedService ve stromovém zobrazení na levé straně.</span><span class="sxs-lookup"><span data-stu-id="38c49-176">Confirm that you see the AzureSqlLinkedService in the tree view on the left.</span></span>

    ![zobrazení stromu s propojené služby](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a><span data-ttu-id="38c49-178">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="38c49-178">Create an output dataset</span></span>
<span data-ttu-id="38c49-179">I když uloženou proceduru neobsahuje žádná data, je nutné zadat výstupní datovou sadu aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-179">You must specify an output dataset for a stored procedure activity even if the stored procedure does not produce any data.</span></span> <span data-ttu-id="38c49-180">Je to způsobeno je výstupní datovou sadu, která určuje plán aktivity (jak často aktivita spuštěna - hodinové, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="38c49-180">That's because it's the output dataset that drives the schedule of the activity (how often the activity is run - hourly, daily, etc.).</span></span> <span data-ttu-id="38c49-181">Musíte použít výstupní datovou sadu **propojená služba** který odkazuje na databázi SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, který chcete spustit uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="38c49-181">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <span data-ttu-id="38c49-182">Výstupní datovou sadu může sloužit jako způsob, jak předat výsledek uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) v kanálu.</span><span class="sxs-lookup"><span data-stu-id="38c49-182">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in the pipeline.</span></span> <span data-ttu-id="38c49-183">Ale objekt pro vytváření dat nelze zapsat automaticky výstup uložené procedury pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="38c49-183">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="38c49-184">Je uložené procedury, která zapisuje do tabulky SQL, odkazující na výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="38c49-184">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <span data-ttu-id="38c49-185">V některých případech může být výstupní datovou sadu **fiktivní datovou sadu** (datovou sadu, která odkazuje na tabulku, která skutečně neobsahuje výstup uložené procedury).</span><span class="sxs-lookup"><span data-stu-id="38c49-185">In some cases, the output dataset can be a **dummy dataset** (a dataset that points to a table that does not really hold output of the stored procedure).</span></span> <span data-ttu-id="38c49-186">Tato datová sada fiktivní slouží pouze k určení plánu pro spuštěnou aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-186">This dummy dataset is used only to specify the schedule for running the stored procedure activity.</span></span> 

1. <span data-ttu-id="38c49-187">Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na panelu nástrojů klikněte na tlačítko **nová datová sada**a klikněte na tlačítko **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="38c49-187">Click **... More** on the toolbar, click **New dataset**, and click **Azure SQL**.</span></span> <span data-ttu-id="38c49-188">**Nová datová sada** na panelu příkazů a vyberte **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="38c49-188">**New dataset** on the command bar and select **Azure SQL**.</span></span>

    ![zobrazení stromu s propojené služby](media/data-factory-stored-proc-activity/new-dataset.png)
2. <span data-ttu-id="38c49-190">Zkopírujte a vložte následující skript JSON v pro JSON editor.</span><span class="sxs-lookup"><span data-stu-id="38c49-190">Copy/paste the following JSON script in to the JSON editor.</span></span>

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. <span data-ttu-id="38c49-191">Chcete-li nasadit datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="38c49-191">To deploy the dataset, click **Deploy** on the command bar.</span></span> <span data-ttu-id="38c49-192">Zkontrolujte, jestli datové sady ve stromovém zobrazení.</span><span class="sxs-lookup"><span data-stu-id="38c49-192">Confirm that you see the dataset in the tree view.</span></span>

    ![Zobrazení stromu s propojenými službami](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a><span data-ttu-id="38c49-194">Vytvoření kanálu s aktivitou SqlServerStoredProcedure</span><span class="sxs-lookup"><span data-stu-id="38c49-194">Create a pipeline with SqlServerStoredProcedure activity</span></span>
<span data-ttu-id="38c49-195">Teď umožňuje vytvoření kanálu s aktivitou uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-195">Now, let's create a pipeline with a stored procedure activity.</span></span> 

<span data-ttu-id="38c49-196">Všimněte si následujících vlastností:</span><span class="sxs-lookup"><span data-stu-id="38c49-196">Notice the following properties:</span></span> 

- <span data-ttu-id="38c49-197">**Typ** je nastavena na **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="38c49-197">The **type** property is set to **SqlServerStoredProcedure**.</span></span> 
- <span data-ttu-id="38c49-198">**StoredProcedureName** v typu vlastnosti nastavena na **sp_sample** (název uložené procedury).</span><span class="sxs-lookup"><span data-stu-id="38c49-198">The **storedProcedureName** in type properties is set to **sp_sample** (name of the stored procedure).</span></span>
- <span data-ttu-id="38c49-199">**StoredProcedureParameters** část obsahuje jeden parametr s názvem **hodnoty DataTime**.</span><span class="sxs-lookup"><span data-stu-id="38c49-199">The **storedProcedureParameters** section contains one parameter named **DataTime**.</span></span> <span data-ttu-id="38c49-200">Název a malá a velká písmena parametr ve formátu JSON musí odpovídat názvu a velká a malá písmena parametru v definici uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-200">Name and casing of the parameter in JSON must match the name and casing of the parameter in the stored procedure definition.</span></span> <span data-ttu-id="38c49-201">Pokud je třeba předat hodnotu null pro parametr, použijte syntaxi: `"param1": null` (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="38c49-201">If you need pass null for a parameter, use the syntax: `"param1": null` (all lowercase).</span></span>
 
1. <span data-ttu-id="38c49-202">Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na panelu příkazů a klikněte na **nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="38c49-202">Click **... More** on the command bar and click **New pipeline**.</span></span>
2. <span data-ttu-id="38c49-203">Zkopírujte a vložte následující fragment kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="38c49-203">Copy/paste the following JSON snippet:</span></span>   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. <span data-ttu-id="38c49-204">Chcete-li nasadit kanálu, klikněte na tlačítko **nasadit** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="38c49-204">To deploy the pipeline, click **Deploy** on the toolbar.</span></span>  

### <a name="monitor-the-pipeline"></a><span data-ttu-id="38c49-205">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="38c49-205">Monitor the pipeline</span></span>
1. <span data-ttu-id="38c49-206">Kliknutím na **X** zavřete editor služby Data Factory a vrátíte se zpátky do okna Objekt pro vytváření dat. Tam klikněte na **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="38c49-206">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory blade, and click **Diagram**.</span></span>

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. <span data-ttu-id="38c49-208">V **Zobrazení diagramu** uvidíte přehled kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="38c49-208">In the **Diagram View**, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. <span data-ttu-id="38c49-210">V zobrazení diagramu dvakrát klikněte na datovou sadu `sprocsampleout`.</span><span class="sxs-lookup"><span data-stu-id="38c49-210">In the Diagram View, double-click the dataset `sprocsampleout`.</span></span> <span data-ttu-id="38c49-211">Zobrazí řezy ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="38c49-211">You see the slices in Ready state.</span></span> <span data-ttu-id="38c49-212">Měla by existovat pět řezy protože řez se vytvářejí každou hodinu mezi počáteční čas a koncový čas z formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="38c49-212">There should be five slices because a slice is produced for each hour between the start time and end time from the JSON.</span></span>

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. <span data-ttu-id="38c49-214">Pokud je řez v **připraven** stavu, spusťte `select * from sampletable` dotaz proti dané databázi Azure SQL, chcete-li ověřit, že data byla vložena v do tabulky pomocí uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-214">When a slice is in **Ready** state, run a `select * from sampletable` query against the Azure SQL database to verify that the data was inserted in to the table by the stored procedure.</span></span>

   ![výstupní data](./media/data-factory-stored-proc-activity/output.png)

   <span data-ttu-id="38c49-216">V tématu [kanál monitorovat](data-factory-monitor-manage-pipelines.md) podrobné informace o monitorování kanálů služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="38c49-216">See [Monitor the pipeline](data-factory-monitor-manage-pipelines.md) for detailed information about monitoring Azure Data Factory pipelines.</span></span>  


## <a name="specify-an-input-dataset"></a><span data-ttu-id="38c49-217">Zadejte vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="38c49-217">Specify an input dataset</span></span>
<span data-ttu-id="38c49-218">Aktivita uložené procedury v návodu, nemá žádné vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="38c49-218">In the walkthrough, stored procedure activity does not have any input datasets.</span></span> <span data-ttu-id="38c49-219">Pokud zadáte vstupní datové sady, aktivity uložené procedury se nespustí, dokud řezy vstupní datové sady je k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="38c49-219">If you specify an input dataset, the stored procedure activity does not run until the slice of input dataset is available (in Ready state).</span></span> <span data-ttu-id="38c49-220">Datová sada může být externí datová sada (který není vyprodukované jinou aktivitu v kanálu stejné) nebo interní datovou sadu, která je vytvořený nadřízenou aktivitou (aktivitu, která se spouští před tato aktivita).</span><span class="sxs-lookup"><span data-stu-id="38c49-220">The dataset can be an external dataset (that is not produced by another activity in the same pipeline) or an internal dataset that is produced by an upstream activity (the activity that runs before this activity).</span></span> <span data-ttu-id="38c49-221">Můžete určit více vstupní datové sady aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-221">You can specify multiple input datasets for the stored procedure activity.</span></span> <span data-ttu-id="38c49-222">Pokud tak učiníte, aktivity uložené procedury spustí pouze v případě, že všechny řezy vstupní datové sady jsou k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="38c49-222">If you do so, the stored procedure activity runs only when all the input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="38c49-223">Vstupní datové sady nelze zpracovat v uložené proceduře jako parametr.</span><span class="sxs-lookup"><span data-stu-id="38c49-223">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="38c49-224">Používá se pouze ke kontrole závislost před zahájením aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-224">It is only used to check the dependency before starting the stored procedure activity.</span></span>

## <a name="chaining-with-other-activities"></a><span data-ttu-id="38c49-225">Řetězení s ostatními aktivitami</span><span class="sxs-lookup"><span data-stu-id="38c49-225">Chaining with other activities</span></span>
<span data-ttu-id="38c49-226">Pokud chcete zřetězit nadřazeného aktivitu se tato aktivita, zadejte jako vstup této aktivity výstup nadřízené činnosti.</span><span class="sxs-lookup"><span data-stu-id="38c49-226">If you want to chain an upstream activity with this activity, specify the output of the upstream activity as an input of this activity.</span></span> <span data-ttu-id="38c49-227">Pokud tak učiníte, aktivity uložené procedury se nespustí, dokud dokončení nadřazeného aktivity a výstupní datovou sadu nadřízené činnosti je k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="38c49-227">When you do so, the stored procedure activity does not run until the upstream activity completes and the output dataset of the upstream activity is available (in Ready status).</span></span> <span data-ttu-id="38c49-228">Výstupní datové sady z více činností, nadřazeného můžete zadat jako vstupní datové sady aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-228">You can specify output datasets of multiple upstream activities as input datasets of the stored procedure activity.</span></span> <span data-ttu-id="38c49-229">Pokud tak učiníte, aktivity uložené procedury spustí pouze v případě, že všechny řezy vstupní datové sady jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="38c49-229">When you do so, the stored procedure activity runs only when all the input dataset slices are available.</span></span>  

<span data-ttu-id="38c49-230">V následujícím příkladu je výstup aktivity kopírování: OutputDataset, což je vstup aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-230">In the following example, the output of the copy activity is: OutputDataset, which is an input of the stored procedure activity.</span></span> <span data-ttu-id="38c49-231">Proto aktivity uložené procedury nelze spustit až do dokončení aktivity kopírování a OutputDataset řez je k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="38c49-231">Therefore, the stored procedure activity does not run until the copy activity completes and the OutputDataset slice is available (in Ready state).</span></span> <span data-ttu-id="38c49-232">Pokud zadáte více vstupní datové sady, aktivity uložené procedury se nespustí, dokud všechny řezy vstupní datové sady jsou k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="38c49-232">If you specify multiple input datasets, the stored procedure activity does not run until all the input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="38c49-233">Vstupní datové sady nelze použít přímo jako parametry pro aktivitu uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-233">The input datasets cannot be used directly as parameters to the stored procedure activity.</span></span> 

<span data-ttu-id="38c49-234">Další informace o řetězení aktivit najdete v tématu [více aktivit v kanálu](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span><span class="sxs-lookup"><span data-stu-id="38c49-234">For more information on chaining activities, see [multiple activities in a pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span></span>

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob to blob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

<span data-ttu-id="38c49-235">Podobně propojení aktivita procedury úložiště s **podřízené aktivity** (aktivity, které spustit po dokončení aktivity uložené procedury), zadejte výstupní datovou sadu aktivity uložené procedury jako vstup podřízené aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="38c49-235">Similarly, to link the store procedure activity with **downstream activities** (the activities that run after the stored procedure activity completes), specify the output dataset of the stored procedure activity as an input of the downstream activity in the pipeline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="38c49-236">Při kopírování dat do Azure SQL Database nebo SQL Server, můžete nakonfigurovat **SqlSink** v aktivitě kopírování k vyvolání uložené procedury pomocí **sqlWriterStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="38c49-236">When copying data into Azure SQL Database or SQL Server, you can configure the **SqlSink** in copy activity to invoke a stored procedure by using the **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="38c49-237">Další informace najdete v tématu [vyvolat uloženou proceduru z aktivity kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="38c49-237">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="38c49-238">Podrobnosti o vlastnost, najdete v následujících článcích konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="38c49-238">For details about the property, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span>
>  
> <span data-ttu-id="38c49-239">Při kopírování dat z Azure SQL Database nebo SQL Server nebo Azure SQL Data Warehouse, můžete nakonfigurovat **SqlSource** v aktivitě kopírování vyvolat uloženou proceduru čtení dat ze zdrojové databáze pomocí  **sqlReaderStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="38c49-239">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity to invoke a stored procedure to read data from the source database by using the **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="38c49-240">Další informace naleznete v následujících článcích konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="38c49-240">For more information, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          

## <a name="json-format"></a><span data-ttu-id="38c49-241">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="38c49-241">JSON format</span></span>
<span data-ttu-id="38c49-242">Tady je formátu JSON pro definování aktivity uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="38c49-242">Here is the JSON format for defining a Stored Procedure Activity:</span></span>

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of the stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

<span data-ttu-id="38c49-243">Následující tabulka popisuje tyto vlastnosti JSON:</span><span class="sxs-lookup"><span data-stu-id="38c49-243">The following table describes these JSON properties:</span></span>

| <span data-ttu-id="38c49-244">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="38c49-244">Property</span></span> | <span data-ttu-id="38c49-245">Popis</span><span class="sxs-lookup"><span data-stu-id="38c49-245">Description</span></span> | <span data-ttu-id="38c49-246">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="38c49-246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="38c49-247">jméno</span><span class="sxs-lookup"><span data-stu-id="38c49-247">name</span></span> | <span data-ttu-id="38c49-248">Název aktivity</span><span class="sxs-lookup"><span data-stu-id="38c49-248">Name of the activity</span></span> |<span data-ttu-id="38c49-249">Ano</span><span class="sxs-lookup"><span data-stu-id="38c49-249">Yes</span></span> |
| <span data-ttu-id="38c49-250">Popis</span><span class="sxs-lookup"><span data-stu-id="38c49-250">description</span></span> |<span data-ttu-id="38c49-251">Text popisující, co se používá aktivitu pro</span><span class="sxs-lookup"><span data-stu-id="38c49-251">Text describing what the activity is used for</span></span> |<span data-ttu-id="38c49-252">Ne</span><span class="sxs-lookup"><span data-stu-id="38c49-252">No</span></span> |
| <span data-ttu-id="38c49-253">type</span><span class="sxs-lookup"><span data-stu-id="38c49-253">type</span></span> | <span data-ttu-id="38c49-254">Musí být nastavena na: **SqlServerStoredProcedure**</span><span class="sxs-lookup"><span data-stu-id="38c49-254">Must be set to: **SqlServerStoredProcedure**</span></span> | <span data-ttu-id="38c49-255">Ano</span><span class="sxs-lookup"><span data-stu-id="38c49-255">Yes</span></span> |
| <span data-ttu-id="38c49-256">Vstupy</span><span class="sxs-lookup"><span data-stu-id="38c49-256">inputs</span></span> | <span data-ttu-id="38c49-257">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="38c49-257">Optional.</span></span> <span data-ttu-id="38c49-258">Pokud zadáte vstupní datové sady, musí být k dispozici (v 'Připravený' stav) se spouští aktivita uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-258">If you do specify an input dataset, it must be available (in ‘Ready’ status) for the stored procedure activity to run.</span></span> <span data-ttu-id="38c49-259">Vstupní datové sady nelze zpracovat v uložené proceduře jako parametr.</span><span class="sxs-lookup"><span data-stu-id="38c49-259">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="38c49-260">Používá se pouze ke kontrole závislost před zahájením aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-260">It is only used to check the dependency before starting the stored procedure activity.</span></span> |<span data-ttu-id="38c49-261">Ne</span><span class="sxs-lookup"><span data-stu-id="38c49-261">No</span></span> |
| <span data-ttu-id="38c49-262">Výstupy</span><span class="sxs-lookup"><span data-stu-id="38c49-262">outputs</span></span> | <span data-ttu-id="38c49-263">Je nutné zadat výstupní datovou sadu aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-263">You must specify an output dataset for a stored procedure activity.</span></span> <span data-ttu-id="38c49-264">Určuje výstupní datovou sadu **plán** aktivity uložené procedury (každou hodinu, týdně, měsíčně, atd.).</span><span class="sxs-lookup"><span data-stu-id="38c49-264">Output dataset specifies the **schedule** for the stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <br/><br/><span data-ttu-id="38c49-265">Musíte použít výstupní datovou sadu **propojená služba** který odkazuje na databázi SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, který chcete spustit uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="38c49-265">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <br/><br/><span data-ttu-id="38c49-266">Výstupní datovou sadu může sloužit jako způsob, jak předat výsledek uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) v kanálu.</span><span class="sxs-lookup"><span data-stu-id="38c49-266">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in the pipeline.</span></span> <span data-ttu-id="38c49-267">Ale objekt pro vytváření dat nelze zapsat automaticky výstup uložené procedury pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="38c49-267">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="38c49-268">Je uložené procedury, která zapisuje do tabulky SQL, odkazující na výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="38c49-268">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <br/><br/><span data-ttu-id="38c49-269">V některých případech může být výstupní datovou sadu **fiktivní datovou sadu**, který slouží pouze k určení plánu pro spuštěnou aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-269">In some cases, the output dataset can be a **dummy dataset**, which is used only to specify the schedule for running the stored procedure activity.</span></span> |<span data-ttu-id="38c49-270">Ano</span><span class="sxs-lookup"><span data-stu-id="38c49-270">Yes</span></span> |
| <span data-ttu-id="38c49-271">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="38c49-271">storedProcedureName</span></span> |<span data-ttu-id="38c49-272">Zadejte název uložené procedury v Azure SQL database nebo databáze Azure SQL Data Warehouse nebo SQL Server, která je reprezentována propojené služby, která používá výstupní tabulka.</span><span class="sxs-lookup"><span data-stu-id="38c49-272">Specify the name of the stored procedure in the Azure SQL database or Azure SQL Data Warehouse or SQL Server database that is represented by the linked service that the output table uses.</span></span> |<span data-ttu-id="38c49-273">Ano</span><span class="sxs-lookup"><span data-stu-id="38c49-273">Yes</span></span> |
| <span data-ttu-id="38c49-274">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="38c49-274">storedProcedureParameters</span></span> |<span data-ttu-id="38c49-275">Zadejte hodnoty pro parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-275">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="38c49-276">Pokud potřebujete předat hodnotu null pro parametr, použijte syntaxi: "param1": null (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="38c49-276">If you need to pass null for a parameter, use the syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="38c49-277">Viz následující ukázka Další informace o používání této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="38c49-277">See the following sample to learn about using this property.</span></span> |<span data-ttu-id="38c49-278">Ne</span><span class="sxs-lookup"><span data-stu-id="38c49-278">No</span></span> |

## <a name="passing-a-static-value"></a><span data-ttu-id="38c49-279">Předávání na statické hodnoty</span><span class="sxs-lookup"><span data-stu-id="38c49-279">Passing a static value</span></span>
<span data-ttu-id="38c49-280">Nyní Pojďme zvažte přidání jiný sloupec s názvem 'Scénář' v tabulce obsahující statická hodnota volána 'Dokumentu ukázka'.</span><span class="sxs-lookup"><span data-stu-id="38c49-280">Now, let’s consider adding another column named ‘Scenario’ in the table containing a static value called ‘Document sample’.</span></span>

![Ukázková data 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

<span data-ttu-id="38c49-282">**Tabulka:**</span><span class="sxs-lookup"><span data-stu-id="38c49-282">**Table:**</span></span>

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

<span data-ttu-id="38c49-283">**Uložené procedury:**</span><span class="sxs-lookup"><span data-stu-id="38c49-283">**Stored procedure:**</span></span>

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

<span data-ttu-id="38c49-284">Nyní, předat **scénář** parametr a hodnotu z aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="38c49-284">Now, pass the **Scenario** parameter and the value from the stored procedure activity.</span></span> <span data-ttu-id="38c49-285">**Rámci typeProperties** část v předchozím příkladu vypadá jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="38c49-285">The **typeProperties** section in the preceding sample looks like the following snippet:</span></span>

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

<span data-ttu-id="38c49-286">**Datové sady objektu pro vytváření dat:**</span><span class="sxs-lookup"><span data-stu-id="38c49-286">**Data Factory dataset:**</span></span>

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="38c49-287">**Kanál služby Data Factory**</span><span class="sxs-lookup"><span data-stu-id="38c49-287">**Data Factory pipeline**</span></span>

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```