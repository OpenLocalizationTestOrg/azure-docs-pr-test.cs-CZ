---
title: "aaaSQL aktivity uložené procedury serveru"
description: "Zjistěte, jak je možné používat tooinvoke aktivity uložené procedury aplikace SQL Server hello uloženou proceduru v databázi SQL Azure nebo Azure SQL Data Warehouse z objektu pro vytváření dat kanál."
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
ms.openlocfilehash: 9116f80eefc59d95e866b2ba1de2feb1bdc4b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-stored-procedure-activity"></a><span data-ttu-id="29b49-103">Systému SQL Server uložené procedury aktivity</span><span class="sxs-lookup"><span data-stu-id="29b49-103">SQL Server Stored Procedure Activity</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="29b49-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="29b49-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="29b49-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="29b49-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="29b49-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="29b49-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="29b49-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="29b49-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="29b49-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="29b49-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="29b49-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="29b49-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="29b49-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="29b49-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="29b49-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="29b49-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="29b49-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="29b49-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="29b49-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="29b49-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="overview"></a><span data-ttu-id="29b49-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="29b49-114">Overview</span></span>
<span data-ttu-id="29b49-115">Pomocí aktivit transformace dat v datové továrně [kanálu](data-factory-create-pipelines.md) tootransform a proces nezpracovaná data do předpovědi a statistiky.</span><span class="sxs-lookup"><span data-stu-id="29b49-115">You use data transformation activities in a Data Factory [pipeline](data-factory-create-pipelines.md) tootransform and process raw data into predictions and insights.</span></span> <span data-ttu-id="29b49-116">Hello aktivity uložené procedury patří mezi hello aktivit transformace, které podporuje služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="29b49-116">hello Stored Procedure Activity is one of hello transformation activities that Data Factory supports.</span></span> <span data-ttu-id="29b49-117">Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporované ve službě Data Factory.</span><span class="sxs-lookup"><span data-stu-id="29b49-117">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities in Data Factory.</span></span>

<span data-ttu-id="29b49-118">Můžete použít tooinvoke hello aktivity uložené procedury uložené procedury v jednom z hello následující data ukládá ve vaší podnikové síti nebo na virtuální počítač Azure (VM):</span><span class="sxs-lookup"><span data-stu-id="29b49-118">You can use hello Stored Procedure Activity tooinvoke a stored procedure in one of hello following data stores in your enterprise or on an Azure virtual machine (VM):</span></span> 

- <span data-ttu-id="29b49-119">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="29b49-119">Azure SQL Database</span></span>
- <span data-ttu-id="29b49-120">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="29b49-120">Azure SQL Data Warehouse</span></span>
- <span data-ttu-id="29b49-121">Databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="29b49-121">SQL Server Database.</span></span>  <span data-ttu-id="29b49-122">Pokud používáte systém SQL Server, nainstalujte Brána pro správu dat na stejný počítač, hostitele hello databáze nebo na samostatný počítač, který má přístup k databázi toohello hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-122">If you are using SQL Server, install Data Management Gateway on hello same machine that hosts hello database or on a separate machine that has access toohello database.</span></span> <span data-ttu-id="29b49-123">Brána pro správu dat je, že komponenty, která se připojuje data způsobem, zabezpečení a správě zdroje na místní nebo na virtuální počítač Azure s cloudovými službami.</span><span class="sxs-lookup"><span data-stu-id="29b49-123">Data Management Gateway is a component that connects data sources on-premises/on Azure VM with cloud services in a secure and managed way.</span></span> <span data-ttu-id="29b49-124">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článku.</span><span class="sxs-lookup"><span data-stu-id="29b49-124">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29b49-125">Při kopírování dat do Azure SQL Database nebo SQL Server, můžete nakonfigurovat hello **SqlSink** v tooinvoke kopie aktivity uložené procedury pomocí hello **sqlWriterStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="29b49-125">When copying data into Azure SQL Database or SQL Server, you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure by using hello **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="29b49-126">Další informace najdete v tématu [vyvolat uloženou proceduru z aktivity kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="29b49-126">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="29b49-127">Podrobnosti o hello vlastnosti, viz následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="29b49-127">For details about hello property, see following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span> <span data-ttu-id="29b49-128">Volání uložené procedury při kopírování dat do Azure SQL Data Warehouse pomocí aktivity kopírování není podporována.</span><span class="sxs-lookup"><span data-stu-id="29b49-128">Invoking a stored procedure while copying data into an Azure SQL Data Warehouse by using a copy activity is not supported.</span></span> <span data-ttu-id="29b49-129">Ale hello uložené procedury aktivity tooinvoke uloženou proceduru můžete použít v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="29b49-129">But, you can use hello stored procedure activity tooinvoke a stored procedure in a SQL Data Warehouse.</span></span> 
>  
> <span data-ttu-id="29b49-130">Při kopírování dat z Azure SQL Database nebo SQL Server nebo Azure SQL Data Warehouse, můžete nakonfigurovat **SqlSource** v tooinvoke aktivity kopírování dat ze zdrojové databáze hello pomocí hello tooread uložené procedury  **sqlReaderStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="29b49-130">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity tooinvoke a stored procedure tooread data from hello source database by using hello **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="29b49-131">Další informace najdete v tématu hello následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="29b49-131">For more information, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          


<span data-ttu-id="29b49-132">Následující postup používá Hello hello aktivity uložené procedury v kanálu tooinvoke uloženou proceduru v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="29b49-132">hello following walkthrough uses hello Stored Procedure Activity in a pipeline tooinvoke a stored procedure in an Azure SQL database.</span></span> 

## <a name="walkthrough"></a><span data-ttu-id="29b49-133">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="29b49-133">Walkthrough</span></span>
### <a name="sample-table-and-stored-procedure"></a><span data-ttu-id="29b49-134">Ukázkové tabulky a uložené procedury</span><span class="sxs-lookup"><span data-stu-id="29b49-134">Sample table and stored procedure</span></span>
1. <span data-ttu-id="29b49-135">Vytvořte následující hello **tabulky** ve vaší databázi SQL Azure pomocí SQL Server Management Studio nebo jakýkoli jiný nástroj, který jste obeznámeni s.</span><span class="sxs-lookup"><span data-stu-id="29b49-135">Create hello following **table** in your Azure SQL Database using SQL Server Management Studio or any other tool you are comfortable with.</span></span> <span data-ttu-id="29b49-136">sloupec razítko_data_a_času Hello je hello datum a čas, kdy se vygeneruje odpovídající ID hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-136">hello datetimestamp column is hello date and time when hello corresponding ID is generated.</span></span>

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
    <span data-ttu-id="29b49-137">ID je jedinečný hello identifikovat a hello razítko_data_a_času sloupec je hello datum a čas, kdy se vygeneruje odpovídající ID hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-137">Id is hello unique identified and hello datetimestamp column is hello date and time when hello corresponding ID is generated.</span></span>
    
    ![Ukázková data](./media/data-factory-stored-proc-activity/sample-data.png)

    <span data-ttu-id="29b49-139">V této ukázce hello uložená procedura je v databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="29b49-139">In this sample, hello stored procedure is in an Azure SQL Database.</span></span> <span data-ttu-id="29b49-140">Pokud hello uložené procedury je do Azure SQL Data Warehouse a databáze systému SQL Server, je podobný hello přístup.</span><span class="sxs-lookup"><span data-stu-id="29b49-140">If hello stored procedure is in an Azure SQL Data Warehouse and SQL Server Database, hello approach is similar.</span></span> <span data-ttu-id="29b49-141">Pro databázi systému SQL Server, musíte nainstalovat [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="29b49-141">For a SQL Server database, you must install a [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>
2. <span data-ttu-id="29b49-142">Vytvořte následující hello **uložené procedury** který vkládá data v toohello **sampletable**.</span><span class="sxs-lookup"><span data-stu-id="29b49-142">Create hello following **stored procedure** that inserts data in toohello **sampletable**.</span></span>

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="29b49-143">**Název** a **velká a malá písmena** z hello parametr (hodnota DateTime v tomto příkladu) musí odpovídat parametr zadaný v kanálu nebo aktivity hello JSON.</span><span class="sxs-lookup"><span data-stu-id="29b49-143">**Name** and **casing** of hello parameter (DateTime in this example) must match that of parameter specified in hello pipeline/activity JSON.</span></span> <span data-ttu-id="29b49-144">V hello Definice uloženou proceduru, ujistěte se, že  **@**  slouží jako předpona pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-144">In hello stored procedure definition, ensure that **@** is used as a prefix for hello parameter.</span></span>

### <a name="create-a-data-factory"></a><span data-ttu-id="29b49-145">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="29b49-145">Create a data factory</span></span>
1. <span data-ttu-id="29b49-146">Přihlaste se příliš[portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29b49-146">Log in too[Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="29b49-147">Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="29b49-147">Click **NEW** on hello left menu, click **Intelligence + Analytics**, and click **Data Factory**.</span></span>

    ![Nový objekt pro vytváření dat](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. <span data-ttu-id="29b49-149">V hello **nový objekt pro vytváření dat** okno, zadejte **SProcDF** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="29b49-149">In hello **New data factory** blade, enter **SProcDF** for hello Name.</span></span> <span data-ttu-id="29b49-150">Azure Data Factory názvů **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="29b49-150">Azure Data Factory names are **globally unique**.</span></span> <span data-ttu-id="29b49-151">Je nutné tooprefix hello název objektu pro vytváření dat hello s názvem, tooenable hello úspěšném vytvoření objektu pro vytváření hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-151">You need tooprefix hello name of hello data factory with your name, tooenable hello successful creation of hello factory.</span></span>

   ![Nový objekt pro vytváření dat](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. <span data-ttu-id="29b49-153">Vyberte vaše **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="29b49-153">Select your **Azure subscription**.</span></span>
5. <span data-ttu-id="29b49-154">Pro **skupiny prostředků**, proveďte jednu z následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="29b49-154">For **Resource Group**, do one of hello following steps:</span></span>
   1. <span data-ttu-id="29b49-155">Klikněte na tlačítko **vytvořit nový** a zadejte název pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-155">Click **Create new** and enter a name for hello resource group.</span></span>
   2. <span data-ttu-id="29b49-156">Klikněte na tlačítko **použít existující** a vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="29b49-156">Click **Use existing** and select an existing resource group.</span></span>  
6. <span data-ttu-id="29b49-157">Vyberte hello **umístění** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="29b49-157">Select hello **location** for hello data factory.</span></span>
7. <span data-ttu-id="29b49-158">Vyberte **Pin toodashboard** , aby mohli zobrazit objektu pro vytváření dat hello na řídicím panelu hello příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="29b49-158">Select **Pin toodashboard** so that you can see hello data factory on hello dashboard next time you log in.</span></span>
8. <span data-ttu-id="29b49-159">Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="29b49-159">Click **Create** on hello **New data factory** blade.</span></span>
9. <span data-ttu-id="29b49-160">Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="29b49-160">You see hello data factory being created in hello **dashboard** of hello Azure portal.</span></span> <span data-ttu-id="29b49-161">Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-161">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>

   ![Domovská stránka objektu pro vytváření dat](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a><span data-ttu-id="29b49-163">Vytvoření služby Azure SQL propojené</span><span class="sxs-lookup"><span data-stu-id="29b49-163">Create an Azure SQL linked service</span></span>
<span data-ttu-id="29b49-164">Po vytvoření objektu pro vytváření dat hello, vytváření služby SQL Azure, propojené, který odkazuje na databázi Azure SQL, který obsahuje hello sampletable tabulky a sp_sample uložené procedury, objekt pro vytváření dat tooyour.</span><span class="sxs-lookup"><span data-stu-id="29b49-164">After creating hello data factory, you create an Azure SQL linked service that links your Azure SQL database, which contains hello sampletable table and sp_sample stored procedure, tooyour data factory.</span></span>

1. <span data-ttu-id="29b49-165">Klikněte na tlačítko **vytvořit a nasadit** na hello **Data Factory** okno pro **SProcDF** toolaunch hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="29b49-165">Click **Author and deploy** on hello **Data Factory** blade for **SProcDF** toolaunch hello Data Factory Editor.</span></span>
2. <span data-ttu-id="29b49-166">Klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a zvolte **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="29b49-166">Click **New data store** on hello command bar and choose **Azure SQL Database**.</span></span> <span data-ttu-id="29b49-167">Měli byste vidět, že hello skript JSON pro vytvoření Azure SQL propojená služba v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-167">You should see hello JSON script for creating an Azure SQL linked service in hello editor.</span></span>

   ![Nové úložiště dat](media/data-factory-stored-proc-activity/new-data-store.png)
3. <span data-ttu-id="29b49-169">V hello skript JSON proveďte hello následující změny:</span><span class="sxs-lookup"><span data-stu-id="29b49-169">In hello JSON script, make hello following changes:</span></span>

   1. <span data-ttu-id="29b49-170">Nahraďte `<servername>` s názvem hello serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="29b49-170">Replace `<servername>` with hello name of your Azure SQL Database server.</span></span>
   2. <span data-ttu-id="29b49-171">Nahraďte `<databasename>` s hello databáze, ve které jste vytvořili tabulku hello a hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-171">Replace `<databasename>` with hello database in which you created hello table and hello stored procedure.</span></span>
   3. <span data-ttu-id="29b49-172">Nahraďte `<username@servername>` s hello uživatelský účet, který má přístup toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="29b49-172">Replace `<username@servername>` with hello user account that has access toohello database.</span></span>
   4. <span data-ttu-id="29b49-173">Nahraďte `<password>` s hello heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-173">Replace `<password>` with hello password for hello user account.</span></span>

      ![Nové úložiště dat](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. <span data-ttu-id="29b49-175">toodeploy hello propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-175">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="29b49-176">Zkontrolujte, jestli hello AzureSqlLinkedService ve stromu hello zobrazení na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-176">Confirm that you see hello AzureSqlLinkedService in hello tree view on hello left.</span></span>

    ![zobrazení stromu s propojené služby](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a><span data-ttu-id="29b49-178">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="29b49-178">Create an output dataset</span></span>
<span data-ttu-id="29b49-179">Je nutné zadat, že výstupní datovou sadu aktivity uložené procedury i v případě, že hello uložené procedury neobsahuje žádná data.</span><span class="sxs-lookup"><span data-stu-id="29b49-179">You must specify an output dataset for a stored procedure activity even if hello stored procedure does not produce any data.</span></span> <span data-ttu-id="29b49-180">Je to způsobeno hello jeho výstupní datovou sadu, která řídí hello plán hello aktivity (jak často hello aktivita spuštěna - hodinové, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="29b49-180">That's because it's hello output dataset that drives hello schedule of hello activity (how often hello activity is run - hourly, daily, etc.).</span></span> <span data-ttu-id="29b49-181">Hello výstupní datovou sadu musí používat **propojená služba** který odkazuje tooan databáze SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, ve kterém chcete hello toorun uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-181">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <span data-ttu-id="29b49-182">Hello výstupní datovou sadu může sloužit jako výsledek hello toopass způsob hello uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-182">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in hello pipeline.</span></span> <span data-ttu-id="29b49-183">Ale objekt pro vytváření dat nelze zapsat automaticky hello výstupní datové sady toothis uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-183">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="29b49-184">Je, že hello této tabulky SQL tooa zápisů, které hello výstupní datová sada odkazuje na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-184">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <span data-ttu-id="29b49-185">V některých případech může být hello výstupní datovou sadu **fiktivní datovou sadu** (datovou sadu, která ukazuje tooa tabulku, která skutečně neobsahuje výstup hello uložené procedury).</span><span class="sxs-lookup"><span data-stu-id="29b49-185">In some cases, hello output dataset can be a **dummy dataset** (a dataset that points tooa table that does not really hold output of hello stored procedure).</span></span> <span data-ttu-id="29b49-186">Tato fiktivní datová sada se používá jen toospecify hello plánu pro spuštěnou hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-186">This dummy dataset is used only toospecify hello schedule for running hello stored procedure activity.</span></span> 

1. <span data-ttu-id="29b49-187">Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na hello nástrojů, klikněte na tlačítko **nová datová sada**a klikněte na tlačítko **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="29b49-187">Click **... More** on hello toolbar, click **New dataset**, and click **Azure SQL**.</span></span> <span data-ttu-id="29b49-188">**Nová datová sada** na příkaz hello panelu a vyberte možnost **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="29b49-188">**New dataset** on hello command bar and select **Azure SQL**.</span></span>

    ![zobrazení stromu s propojené služby](media/data-factory-stored-proc-activity/new-dataset.png)
2. <span data-ttu-id="29b49-190">Zkopírujte a vložte následující skript JSON v editoru JSON toohello hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-190">Copy/paste hello following JSON script in toohello JSON editor.</span></span>

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
3. <span data-ttu-id="29b49-191">toodeploy hello datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-191">toodeploy hello dataset, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="29b49-192">Zkontrolujte, jestli hello datovou sadu v zobrazení stromu hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-192">Confirm that you see hello dataset in hello tree view.</span></span>

    ![Zobrazení stromu s propojenými službami](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a><span data-ttu-id="29b49-194">Vytvoření kanálu s aktivitou SqlServerStoredProcedure</span><span class="sxs-lookup"><span data-stu-id="29b49-194">Create a pipeline with SqlServerStoredProcedure activity</span></span>
<span data-ttu-id="29b49-195">Teď umožňuje vytvoření kanálu s aktivitou uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-195">Now, let's create a pipeline with a stored procedure activity.</span></span> 

<span data-ttu-id="29b49-196">Všimněte si hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="29b49-196">Notice hello following properties:</span></span> 

- <span data-ttu-id="29b49-197">Hello **typ** vlastnost je nastavena příliš**SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="29b49-197">hello **type** property is set too**SqlServerStoredProcedure**.</span></span> 
- <span data-ttu-id="29b49-198">Hello **storedProcedureName** v typu vlastnosti nastavena příliš**sp_sample** (název hello uložené procedury).</span><span class="sxs-lookup"><span data-stu-id="29b49-198">hello **storedProcedureName** in type properties is set too**sp_sample** (name of hello stored procedure).</span></span>
- <span data-ttu-id="29b49-199">Hello **storedProcedureParameters** část obsahuje jeden parametr s názvem **hodnoty DataTime**.</span><span class="sxs-lookup"><span data-stu-id="29b49-199">hello **storedProcedureParameters** section contains one parameter named **DataTime**.</span></span> <span data-ttu-id="29b49-200">Název a malá a velká písmena hello parametr ve formátu JSON musí odpovídat názvu hello a velká a malá písmena hello parametru v definici hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-200">Name and casing of hello parameter in JSON must match hello name and casing of hello parameter in hello stored procedure definition.</span></span> <span data-ttu-id="29b49-201">Pokud je třeba předat hodnotu null pro parametr, použijte syntaxi hello: `"param1": null` (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="29b49-201">If you need pass null for a parameter, use hello syntax: `"param1": null` (all lowercase).</span></span>
 
1. <span data-ttu-id="29b49-202">Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na panelu příkazů hello a klikněte na tlačítko **nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="29b49-202">Click **... More** on hello command bar and click **New pipeline**.</span></span>
2. <span data-ttu-id="29b49-203">Hello zkopírujte a vložte následující fragment kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="29b49-203">Copy/paste hello following JSON snippet:</span></span>   

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
3. <span data-ttu-id="29b49-204">toodeploy hello kanálu, klikněte na tlačítko **nasadit** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-204">toodeploy hello pipeline, click **Deploy** on hello toolbar.</span></span>  

### <a name="monitor-hello-pipeline"></a><span data-ttu-id="29b49-205">Monitorování kanálu hello</span><span class="sxs-lookup"><span data-stu-id="29b49-205">Monitor hello pipeline</span></span>
1. <span data-ttu-id="29b49-206">Klikněte na tlačítko **X** okna editoru služby Data Factory tooclose toonavigate zpět toohello okno objekt pro vytváření dat a klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="29b49-206">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory blade, and click **Diagram**.</span></span>

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. <span data-ttu-id="29b49-208">V hello **zobrazení diagramu**, zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="29b49-208">In hello **Diagram View**, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. <span data-ttu-id="29b49-210">V hello zobrazení diagramu, klikněte dvakrát na datovou sadu hello `sprocsampleout`.</span><span class="sxs-lookup"><span data-stu-id="29b49-210">In hello Diagram View, double-click hello dataset `sprocsampleout`.</span></span> <span data-ttu-id="29b49-211">Zobrazí hello řezy ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="29b49-211">You see hello slices in Ready state.</span></span> <span data-ttu-id="29b49-212">Měla by existovat pět řezy protože řez se vytvářejí každou hodinu mezi hello počáteční a koncový čas z hello JSON.</span><span class="sxs-lookup"><span data-stu-id="29b49-212">There should be five slices because a slice is produced for each hour between hello start time and end time from hello JSON.</span></span>

    ![Dlaždice diagram](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. <span data-ttu-id="29b49-214">Pokud je řez v **připraven** stavu, spusťte `select * from sampletable` dotaz hello tooverify databáze Azure SQL, který hello data byla vložena v tabulce toohello hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-214">When a slice is in **Ready** state, run a `select * from sampletable` query against hello Azure SQL database tooverify that hello data was inserted in toohello table by hello stored procedure.</span></span>

   ![výstupní data](./media/data-factory-stored-proc-activity/output.png)

   <span data-ttu-id="29b49-216">V tématu [monitorování hello kanálu](data-factory-monitor-manage-pipelines.md) podrobné informace o monitorování kanálů služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="29b49-216">See [Monitor hello pipeline](data-factory-monitor-manage-pipelines.md) for detailed information about monitoring Azure Data Factory pipelines.</span></span>  


## <a name="specify-an-input-dataset"></a><span data-ttu-id="29b49-217">Zadejte vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="29b49-217">Specify an input dataset</span></span>
<span data-ttu-id="29b49-218">Aktivita uložené procedury v návodu hello nemá žádné vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="29b49-218">In hello walkthrough, stored procedure activity does not have any input datasets.</span></span> <span data-ttu-id="29b49-219">Pokud zadáte vstupní datové sady, hello aktivity uložené procedury nespustí dokud hello řezy vstupní datové sady je k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="29b49-219">If you specify an input dataset, hello stored procedure activity does not run until hello slice of input dataset is available (in Ready state).</span></span> <span data-ttu-id="29b49-220">Hello datová sada může být externí datová sada (který není vyprodukované jinou aktivitu v hello stejné kanálu) nebo interní datovou sadu, která je vytvořený nadřízenou aktivitou (hello aktivity, která se spouští před této aktivity).</span><span class="sxs-lookup"><span data-stu-id="29b49-220">hello dataset can be an external dataset (that is not produced by another activity in hello same pipeline) or an internal dataset that is produced by an upstream activity (hello activity that runs before this activity).</span></span> <span data-ttu-id="29b49-221">Můžete určit více vstupní datové sady pro aktivity hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-221">You can specify multiple input datasets for hello stored procedure activity.</span></span> <span data-ttu-id="29b49-222">Pokud tak učiníte, hello aktivity uložené procedury spustí jenom v případě, že všechny řezy hello vstupní datové sady jsou k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="29b49-222">If you do so, hello stored procedure activity runs only when all hello input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="29b49-223">vstupní datové sady Hello nelze zpracovat v hello uložené procedury jako parametr.</span><span class="sxs-lookup"><span data-stu-id="29b49-223">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="29b49-224">Je pouze použité toocheck hello závislostí před počáteční hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-224">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span>

## <a name="chaining-with-other-activities"></a><span data-ttu-id="29b49-225">Řetězení s ostatními aktivitami</span><span class="sxs-lookup"><span data-stu-id="29b49-225">Chaining with other activities</span></span>
<span data-ttu-id="29b49-226">Pokud chcete toochain nadřazeného aktivitu se tato aktivita, zadejte jako vstup této aktivity hello výstup hello nadřazené aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-226">If you want toochain an upstream activity with this activity, specify hello output of hello upstream activity as an input of this activity.</span></span> <span data-ttu-id="29b49-227">Pokud tak učiníte, hello aktivity uložené procedury nelze spustit až do dokončení hello nadřazeného aktivity a hello výstupní datovou sadu nadřízené činnosti hello je k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="29b49-227">When you do so, hello stored procedure activity does not run until hello upstream activity completes and hello output dataset of hello upstream activity is available (in Ready status).</span></span> <span data-ttu-id="29b49-228">Výstupní datové sady z více činností, nadřazeného můžete zadat jako vstupní datové sady hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-228">You can specify output datasets of multiple upstream activities as input datasets of hello stored procedure activity.</span></span> <span data-ttu-id="29b49-229">Když to uděláte tak hello uložené procedury aktivity spustí pouze v případě, že všechny řezy hello vstupní datové sady jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="29b49-229">When you do so, hello stored procedure activity runs only when all hello input dataset slices are available.</span></span>  

<span data-ttu-id="29b49-230">V následujícím příkladu hello, je hello výstup aktivity kopírování hello: OutputDataset, což je vstup hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-230">In hello following example, hello output of hello copy activity is: OutputDataset, which is an input of hello stored procedure activity.</span></span> <span data-ttu-id="29b49-231">Proto hello aktivity uložené procedury nelze spustit až do dokončení aktivity kopírování hello a hello OutputDataset řez je k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="29b49-231">Therefore, hello stored procedure activity does not run until hello copy activity completes and hello OutputDataset slice is available (in Ready state).</span></span> <span data-ttu-id="29b49-232">Pokud zadáte více vstupní datové sady, hello aktivity uložené procedury nelze spustit až do všech řezech hello vstupní datové sady jsou k dispozici (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="29b49-232">If you specify multiple input datasets, hello stored procedure activity does not run until all hello input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="29b49-233">vstupní datové sady Hello nelze použít přímo jako aktivita parametry toohello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-233">hello input datasets cannot be used directly as parameters toohello stored procedure activity.</span></span> 

<span data-ttu-id="29b49-234">Další informace o řetězení aktivit najdete v tématu [více aktivit v kanálu](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span><span class="sxs-lookup"><span data-stu-id="29b49-234">For more information on chaining activities, see [multiple activities in a pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span></span>

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob tooblob",
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

<span data-ttu-id="29b49-235">Podobně toolink hello úložiště aktivita procedury s **podřízené aktivity** (hello aktivity, které spustí po dokončení technologie hello aktivity uložené procedury), zadejte hello výstupní datovou sadu aktivity hello uložené procedury jako vstup hello podřízené aktivity v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-235">Similarly, toolink hello store procedure activity with **downstream activities** (hello activities that run after hello stored procedure activity completes), specify hello output dataset of hello stored procedure activity as an input of hello downstream activity in hello pipeline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29b49-236">Při kopírování dat do Azure SQL Database nebo SQL Server, můžete nakonfigurovat hello **SqlSink** v tooinvoke kopie aktivity uložené procedury pomocí hello **sqlWriterStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="29b49-236">When copying data into Azure SQL Database or SQL Server, you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure by using hello **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="29b49-237">Další informace najdete v tématu [vyvolat uloženou proceduru z aktivity kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="29b49-237">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="29b49-238">Podrobnosti o hello vlastnosti najdete v tématu hello následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="29b49-238">For details about hello property, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span>
>  
> <span data-ttu-id="29b49-239">Při kopírování dat z Azure SQL Database nebo SQL Server nebo Azure SQL Data Warehouse, můžete nakonfigurovat **SqlSource** v tooinvoke aktivity kopírování dat ze zdrojové databáze hello pomocí hello tooread uložené procedury  **sqlReaderStoredProcedureName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="29b49-239">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity tooinvoke a stored procedure tooread data from hello source database by using hello **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="29b49-240">Další informace najdete v tématu hello následující články konektor: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [systému SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="29b49-240">For more information, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          

## <a name="json-format"></a><span data-ttu-id="29b49-241">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="29b49-241">JSON format</span></span>
<span data-ttu-id="29b49-242">Tady je hello formátu JSON pro definování aktivity uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="29b49-242">Here is hello JSON format for defining a Stored Procedure Activity:</span></span>

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of hello stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

<span data-ttu-id="29b49-243">Hello následující tabulka popisuje tyto vlastnosti JSON:</span><span class="sxs-lookup"><span data-stu-id="29b49-243">hello following table describes these JSON properties:</span></span>

| <span data-ttu-id="29b49-244">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="29b49-244">Property</span></span> | <span data-ttu-id="29b49-245">Popis</span><span class="sxs-lookup"><span data-stu-id="29b49-245">Description</span></span> | <span data-ttu-id="29b49-246">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="29b49-246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="29b49-247">jméno</span><span class="sxs-lookup"><span data-stu-id="29b49-247">name</span></span> | <span data-ttu-id="29b49-248">Název aktivity hello</span><span class="sxs-lookup"><span data-stu-id="29b49-248">Name of hello activity</span></span> |<span data-ttu-id="29b49-249">Ano</span><span class="sxs-lookup"><span data-stu-id="29b49-249">Yes</span></span> |
| <span data-ttu-id="29b49-250">description</span><span class="sxs-lookup"><span data-stu-id="29b49-250">description</span></span> |<span data-ttu-id="29b49-251">Popisuje, jaké aktivity hello se používá pro text</span><span class="sxs-lookup"><span data-stu-id="29b49-251">Text describing what hello activity is used for</span></span> |<span data-ttu-id="29b49-252">Ne</span><span class="sxs-lookup"><span data-stu-id="29b49-252">No</span></span> |
| <span data-ttu-id="29b49-253">type</span><span class="sxs-lookup"><span data-stu-id="29b49-253">type</span></span> | <span data-ttu-id="29b49-254">Musí být nastavena na: **SqlServerStoredProcedure**</span><span class="sxs-lookup"><span data-stu-id="29b49-254">Must be set to: **SqlServerStoredProcedure**</span></span> | <span data-ttu-id="29b49-255">Ano</span><span class="sxs-lookup"><span data-stu-id="29b49-255">Yes</span></span> |
| <span data-ttu-id="29b49-256">Vstupy</span><span class="sxs-lookup"><span data-stu-id="29b49-256">inputs</span></span> | <span data-ttu-id="29b49-257">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="29b49-257">Optional.</span></span> <span data-ttu-id="29b49-258">Pokud zadáte vstupní datové sady, musí být k dispozici (v 'Připravený' stav) pro hello uložené procedury aktivity toorun.</span><span class="sxs-lookup"><span data-stu-id="29b49-258">If you do specify an input dataset, it must be available (in ‘Ready’ status) for hello stored procedure activity toorun.</span></span> <span data-ttu-id="29b49-259">vstupní datové sady Hello nelze zpracovat v hello uložené procedury jako parametr.</span><span class="sxs-lookup"><span data-stu-id="29b49-259">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="29b49-260">Je pouze použité toocheck hello závislostí před počáteční hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-260">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span> |<span data-ttu-id="29b49-261">Ne</span><span class="sxs-lookup"><span data-stu-id="29b49-261">No</span></span> |
| <span data-ttu-id="29b49-262">Výstupy</span><span class="sxs-lookup"><span data-stu-id="29b49-262">outputs</span></span> | <span data-ttu-id="29b49-263">Je nutné zadat výstupní datovou sadu aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-263">You must specify an output dataset for a stored procedure activity.</span></span> <span data-ttu-id="29b49-264">Výstupní datové sady určuje hello **plán** pro hello uložené procedury aktivity (každou hodinu, týdně, měsíčně, atd.).</span><span class="sxs-lookup"><span data-stu-id="29b49-264">Output dataset specifies hello **schedule** for hello stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <br/><br/><span data-ttu-id="29b49-265">Hello výstupní datovou sadu musí používat **propojená služba** který odkazuje tooan databáze SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, ve kterém chcete hello toorun uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-265">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <br/><br/><span data-ttu-id="29b49-266">Hello výstupní datovou sadu může sloužit jako výsledek hello toopass způsob hello uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-266">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in hello pipeline.</span></span> <span data-ttu-id="29b49-267">Ale objekt pro vytváření dat nelze zapsat automaticky hello výstupní datové sady toothis uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-267">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="29b49-268">Je, že hello této tabulky SQL tooa zápisů, které hello výstupní datová sada odkazuje na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-268">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <br/><br/><span data-ttu-id="29b49-269">V některých případech může být hello výstupní datovou sadu **fiktivní datovou sadu**, který se používá jen toospecify hello plánu pro spuštěnou hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-269">In some cases, hello output dataset can be a **dummy dataset**, which is used only toospecify hello schedule for running hello stored procedure activity.</span></span> |<span data-ttu-id="29b49-270">Ano</span><span class="sxs-lookup"><span data-stu-id="29b49-270">Yes</span></span> |
| <span data-ttu-id="29b49-271">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="29b49-271">storedProcedureName</span></span> |<span data-ttu-id="29b49-272">Zadejte název hello hello uložené procedury v hello Azure SQL database nebo databáze Azure SQL Data Warehouse nebo SQL Server, která je reprezentována hello propojené služby, která hello používá výstupní tabulka.</span><span class="sxs-lookup"><span data-stu-id="29b49-272">Specify hello name of hello stored procedure in hello Azure SQL database or Azure SQL Data Warehouse or SQL Server database that is represented by hello linked service that hello output table uses.</span></span> |<span data-ttu-id="29b49-273">Ano</span><span class="sxs-lookup"><span data-stu-id="29b49-273">Yes</span></span> |
| <span data-ttu-id="29b49-274">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="29b49-274">storedProcedureParameters</span></span> |<span data-ttu-id="29b49-275">Zadejte hodnoty pro parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="29b49-275">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="29b49-276">Pokud potřebujete toopass hodnotu null pro parametr, použijte syntaxi hello: "param1": null (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="29b49-276">If you need toopass null for a parameter, use hello syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="29b49-277">Viz následující ukázka toolearn o používání této vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="29b49-277">See hello following sample toolearn about using this property.</span></span> |<span data-ttu-id="29b49-278">Ne</span><span class="sxs-lookup"><span data-stu-id="29b49-278">No</span></span> |

## <a name="passing-a-static-value"></a><span data-ttu-id="29b49-279">Předávání na statické hodnoty</span><span class="sxs-lookup"><span data-stu-id="29b49-279">Passing a static value</span></span>
<span data-ttu-id="29b49-280">Nyní Pojďme zvažte přidání jiný sloupec s názvem 'Scénář' v tabulce hello obsahující statická hodnota volána 'Dokumentu ukázka'.</span><span class="sxs-lookup"><span data-stu-id="29b49-280">Now, let’s consider adding another column named ‘Scenario’ in hello table containing a static value called ‘Document sample’.</span></span>

![Ukázková data 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

<span data-ttu-id="29b49-282">**Tabulka:**</span><span class="sxs-lookup"><span data-stu-id="29b49-282">**Table:**</span></span>

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

<span data-ttu-id="29b49-283">**Uložené procedury:**</span><span class="sxs-lookup"><span data-stu-id="29b49-283">**Stored procedure:**</span></span>

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

<span data-ttu-id="29b49-284">Nyní, předat hello **scénář** hodnotu parametru a hello z hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="29b49-284">Now, pass hello **Scenario** parameter and hello value from hello stored procedure activity.</span></span> <span data-ttu-id="29b49-285">Hello **rámci typeProperties** část v hello předchozích ukázkových zdá hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="29b49-285">hello **typeProperties** section in hello preceding sample looks like hello following snippet:</span></span>

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

<span data-ttu-id="29b49-286">**Datové sady objektu pro vytváření dat:**</span><span class="sxs-lookup"><span data-stu-id="29b49-286">**Data Factory dataset:**</span></span>

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

<span data-ttu-id="29b49-287">**Kanál služby Data Factory**</span><span class="sxs-lookup"><span data-stu-id="29b49-287">**Data Factory pipeline**</span></span>

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