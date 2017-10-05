---
title: "Kopírování dat z úložiště objektů Blob do SQL Database – Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak pomocí aktivity kopírování v kanál služby Azure Data Factory ke zkopírování dat z úložiště objektů Blob do databáze SQL."
keywords: "objekt BLOB sql, úložiště objektů blob kopírování dat"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 730140d15f4dec7ddc1280c2e4da1d247902fe4a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a><span data-ttu-id="78e22-104">Kurz: Kopírování dat z úložiště objektů Blob do SQL Database pomocí objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="78e22-104">Tutorial: Copy data from Blob Storage to SQL Database using Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="78e22-105">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="78e22-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="78e22-106">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="78e22-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="78e22-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78e22-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="78e22-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78e22-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="78e22-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="78e22-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="78e22-110">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="78e22-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="78e22-111">REST API</span><span class="sxs-lookup"><span data-stu-id="78e22-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="78e22-112">.NET API</span><span class="sxs-lookup"><span data-stu-id="78e22-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="78e22-113">V tomto kurzu vytvoříte objekt pro vytváření dat kanál ke zkopírování dat z úložiště objektů Blob do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="78e22-113">In this tutorial, you create a data factory with a pipeline to copy data from Blob storage to SQL database.</span></span>

<span data-ttu-id="78e22-114">Aktivita kopírování provádí přesun dat ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="78e22-114">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="78e22-115">Používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="78e22-115">It is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="78e22-116">Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="78e22-116">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>  

> [!NOTE]
> <span data-ttu-id="78e22-117">Podrobný přehled služby Data Factory najdete [Úvod do Azure Data Factory](data-factory-introduction.md) článku.</span><span class="sxs-lookup"><span data-stu-id="78e22-117">For a detailed overview of the Data Factory service, see the [Introduction to Azure Data Factory](data-factory-introduction.md) article.</span></span>
>
>

## <a name="prerequisites-for-the-tutorial"></a><span data-ttu-id="78e22-118">Předpoklady pro kurz</span><span class="sxs-lookup"><span data-stu-id="78e22-118">Prerequisites for the tutorial</span></span>
<span data-ttu-id="78e22-119">Je nutné, abyste před zahájením tohoto kurzu splňovali následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="78e22-119">Before you begin this tutorial, you must have the following prerequisites:</span></span>

* <span data-ttu-id="78e22-120">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="78e22-120">**Azure subscription**.</span></span>  <span data-ttu-id="78e22-121">Pokud nemáte předplatné, můžete si během několika minut bezplatně vytvořit zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="78e22-121">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="78e22-122">Najdete v článku [bezplatné zkušební verze](http://azure.microsoft.com/pricing/free-trial/) článku.</span><span class="sxs-lookup"><span data-stu-id="78e22-122">See the [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="78e22-123">**Účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="78e22-123">**Azure Storage Account**.</span></span> <span data-ttu-id="78e22-124">Použít jako úložiště objektů blob **zdroj** úložiště dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78e22-124">You use the blob storage as a **source** data store in this tutorial.</span></span> <span data-ttu-id="78e22-125">Pokud nemáte účet úložiště Azure, najdete v článku [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="78e22-125">if you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps to create one.</span></span>
* <span data-ttu-id="78e22-126">**Databáze Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="78e22-126">**Azure SQL Database**.</span></span> <span data-ttu-id="78e22-127">Použít databázi Azure SQL jako **cílové** úložiště dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78e22-127">You use an Azure SQL database as a **destination** data store in this tutorial.</span></span> <span data-ttu-id="78e22-128">Pokud nemáte Azure SQL database, můžete použít v tomto kurzu, najdete v tématu [jak vytvořit a nakonfigurovat Azure SQL Database](../sql-database/sql-database-get-started.md) k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="78e22-128">If you don't have an Azure SQL database that you can use in the tutorial, See [How to create and configure an Azure SQL Database](../sql-database/sql-database-get-started.md) to create one.</span></span>
* <span data-ttu-id="78e22-129">**SQL Server 2012 nebo 2014 nebo Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="78e22-129">**SQL Server 2012/2014 or Visual Studio 2013**.</span></span> <span data-ttu-id="78e22-130">Používáte SQL Server Management Studio nebo Visual Studio k vytvoření ukázkové databáze a k zobrazení výsledných dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="78e22-130">You use SQL Server Management Studio or Visual Studio to create a sample database and to view the result data in the database.</span></span>  

## <a name="collect-blob-storage-account-name-and-key"></a><span data-ttu-id="78e22-131">Shromažďovat název účtu úložiště objektů blob a klíč</span><span class="sxs-lookup"><span data-stu-id="78e22-131">Collect blob storage account name and key</span></span>
<span data-ttu-id="78e22-132">Potřebujete název účtu a klíč účtu vašeho účtu úložiště Azure uděláte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78e22-132">You need the account name and account key of your Azure storage account to do this tutorial.</span></span> <span data-ttu-id="78e22-133">Poznamenejte si **název účtu** a **klíč účtu** pro váš účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="78e22-133">Note down **account name** and **account key** for your Azure storage account.</span></span>

1. <span data-ttu-id="78e22-134">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="78e22-134">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="78e22-135">Klikněte na tlačítko **další služby** v levé nabídce a vyberte **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="78e22-135">Click **More services** on the left menu and select **Storage Accounts**.</span></span>

    ![Procházet - účty úložiště](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. <span data-ttu-id="78e22-137">V **účty úložiště** okně, vyberte **účtu úložiště Azure** , kterou chcete použít v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78e22-137">In the **Storage Accounts** blade, select the **Azure storage account** that you want to use in this tutorial.</span></span>
4. <span data-ttu-id="78e22-138">Vyberte **přístupové klíče** v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78e22-138">Select **Access keys** link under **SETTINGS**.</span></span>
5. <span data-ttu-id="78e22-139">Klikněte na tlačítko **kopie** tlačítko (obrázek) vedle **název účtu úložiště** text pole a uložit a vložte ji někam (například: do textového souboru).</span><span class="sxs-lookup"><span data-stu-id="78e22-139">Click **copy** (image) button next to **Storage account name** text box and save/paste it somewhere (for example: in a text file).</span></span>
6. <span data-ttu-id="78e22-140">Opakujte předchozí krok zkopírovat nebo poznamenejte **key1**.</span><span class="sxs-lookup"><span data-stu-id="78e22-140">Repeat the previous step to copy or note down the **key1**.</span></span>

    ![Přístupový klíč k úložišti](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. <span data-ttu-id="78e22-142">Zavřete všechna okna kliknutím **X**.</span><span class="sxs-lookup"><span data-stu-id="78e22-142">Close all the blades by clicking **X**.</span></span>

## <a name="collect-sql-server-database-user-names"></a><span data-ttu-id="78e22-143">Shromažďování systému SQL server, databáze, uživatelská jména</span><span class="sxs-lookup"><span data-stu-id="78e22-143">Collect SQL server, database, user names</span></span>
<span data-ttu-id="78e22-144">Názvy serveru Azure SQL, databáze a uživatel mohl tohoto kurzu potřebujete.</span><span class="sxs-lookup"><span data-stu-id="78e22-144">You need the names of Azure SQL server, database, and user to do this tutorial.</span></span> <span data-ttu-id="78e22-145">Zapište názvy **server**, **databáze**, a **uživatele** pro vaši databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="78e22-145">Note down names of **server**, **database**, and **user** for your Azure SQL database.</span></span>

1. <span data-ttu-id="78e22-146">V **portál Azure**, klikněte na tlačítko **další služby** na levé straně a vyberte **databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="78e22-146">In the **Azure portal**, click **More services** on the left and select **SQL databases**.</span></span>
2. <span data-ttu-id="78e22-147">V **okna databáze SQL**, vyberte **databáze** , kterou chcete použít v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78e22-147">In the **SQL databases blade**, select the **database** that you want to use in this tutorial.</span></span> <span data-ttu-id="78e22-148">Poznamenejte si **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="78e22-148">Note down the **database name**.</span></span>  
3. <span data-ttu-id="78e22-149">V **databáze SQL** okně klikněte na tlačítko **vlastnosti** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78e22-149">In the **SQL database** blade, click **Properties** under **SETTINGS**.</span></span>
4. <span data-ttu-id="78e22-150">Zapište hodnoty **název serveru** a **přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="78e22-150">Note down the values for **SERVER NAME** and **SERVER ADMIN LOGIN**.</span></span>
5. <span data-ttu-id="78e22-151">Zavřete všechna okna kliknutím **X**.</span><span class="sxs-lookup"><span data-stu-id="78e22-151">Close all the blades by clicking **X**.</span></span>

## <a name="allow-azure-services-to-access-sql-server"></a><span data-ttu-id="78e22-152">Povolit službám Azure přístup k systému SQL server</span><span class="sxs-lookup"><span data-stu-id="78e22-152">Allow Azure services to access SQL server</span></span>
<span data-ttu-id="78e22-153">Ujistěte se, že **povolit přístup ke službám Azure** nastavení zapnuté **ON** pro server Azure SQL tak, aby služba Data Factory přístup k serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="78e22-153">Ensure that **Allow access to Azure services** setting turned **ON** for your Azure SQL server so that the Data Factory service can access your Azure SQL server.</span></span> <span data-ttu-id="78e22-154">Chcete-li ověřit a zapnout toto nastavení, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="78e22-154">To verify and turn on this setting, do the following steps:</span></span>

1. <span data-ttu-id="78e22-155">Klikněte na tlačítko **další služby** rozbočovače na levé straně a klikněte na **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="78e22-155">Click **More services** hub on the left and click **SQL servers**.</span></span>
2. <span data-ttu-id="78e22-156">Vyberte svůj server a klikněte na tlačítko **brány Firewall** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78e22-156">Select your server, and click **Firewall** under **SETTINGS**.</span></span>
3. <span data-ttu-id="78e22-157">V okně **Nastavení brány firewall** klikněte na **ZAPNUTO** u možnosti **Povolit přístup ke službám Azure**.</span><span class="sxs-lookup"><span data-stu-id="78e22-157">In the **Firewall settings** blade, click **ON** for **Allow access to Azure services**.</span></span>
4. <span data-ttu-id="78e22-158">Zavřete všechna okna kliknutím **X**.</span><span class="sxs-lookup"><span data-stu-id="78e22-158">Close all the blades by clicking **X**.</span></span>

## <a name="prepare-blob-storage-and-sql-database"></a><span data-ttu-id="78e22-159">Příprava úložiště objektů Blob a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="78e22-159">Prepare Blob Storage and SQL Database</span></span>
<span data-ttu-id="78e22-160">Nyní Příprava úložiště objektů blob v Azure a Azure SQL database pro tento kurz provedením následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="78e22-160">Now, prepare your Azure blob storage and Azure SQL database for the tutorial by performing the following steps:</span></span>  

1. <span data-ttu-id="78e22-161">Spusťte Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="78e22-161">Launch Notepad.</span></span> <span data-ttu-id="78e22-162">Zkopírujte následující text a uložte ho jako **emp.txt** k **C:\ADFGetStarted** složky na pevném disku.</span><span class="sxs-lookup"><span data-stu-id="78e22-162">Copy the following text and save it as **emp.txt** to **C:\ADFGetStarted** folder on your hard drive.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
2. <span data-ttu-id="78e22-163">Pomocí nástrojů, jako je [Azure Storage Explorer](http://storageexplorer.com/), vytvořte kontejner **adftutorial** a odešlete soubor **emp.txt** do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="78e22-163">Use tools such as [Azure Storage Explorer](http://storageexplorer.com/) to create the **adftutorial** container and to upload the **emp.txt** file to the container.</span></span>

    ![Azure Storage Explorer.](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. <span data-ttu-id="78e22-166">Pomocí následujícího skriptu SQL vytvořte tabulku **emp** v Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="78e22-166">Use the following SQL script to create the **emp** table in your Azure SQL Database.</span></span>  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    <span data-ttu-id="78e22-167">**Pokud máte SQL Server 2012 nebo 2014 v počítači nainstalována:** postupujte podle pokynů v [Správa Azure SQL Database pomocí SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) pro připojení k serveru Azure SQL a spusťte skript SQL.</span><span class="sxs-lookup"><span data-stu-id="78e22-167">**If you have SQL Server 2012/2014 installed on your computer:** follow instructions from [Managing Azure SQL Database using SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) to connect to your Azure SQL server and run the SQL script.</span></span> <span data-ttu-id="78e22-168">Tento článek používá [portál Azure classic](http://manage.windowsazure.com), nikoli [nový portál Azure](https://portal.azure.com), konfigurace brány firewall pro server Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="78e22-168">This article uses the [classic Azure portal](http://manage.windowsazure.com), not the [new Azure portal](https://portal.azure.com), to configure firewall for an Azure SQL server.</span></span>

    <span data-ttu-id="78e22-169">Pokud klient nemá povolený přístup ke službě Azure SQL Server, budete muset nakonfigurovat bránu firewall pro Azure SQL Server tak, aby povolovala přístup z vašeho počítače (IP adresa).</span><span class="sxs-lookup"><span data-stu-id="78e22-169">If your client is not allowed to access the Azure SQL server, you need to configure firewall for your Azure SQL server to allow access from your machine (IP Address).</span></span> <span data-ttu-id="78e22-170">Postup konfigurace brány firewall pro server SQL Azure najdete v [tomto článku](../sql-database/sql-database-configure-firewall-settings.md).</span><span class="sxs-lookup"><span data-stu-id="78e22-170">See [this article](../sql-database/sql-database-configure-firewall-settings.md) for steps to configure the firewall for your Azure SQL server.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="78e22-171">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="78e22-171">Create a data factory</span></span>
<span data-ttu-id="78e22-172">Dokončili jste požadavky.</span><span class="sxs-lookup"><span data-stu-id="78e22-172">You have completed the prerequisites.</span></span> <span data-ttu-id="78e22-173">Můžete vytvořit objekt pro vytváření dat pomocí jedné z následujících způsobů.</span><span class="sxs-lookup"><span data-stu-id="78e22-173">You can create a data factory using one of the following ways.</span></span> <span data-ttu-id="78e22-174">Klikněte na jednu z možností v rozevíracím seznamu v horní části nebo následující odkazy k provedení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="78e22-174">Click one of the options in the drop-down list at the top or the following links to perform the tutorial.</span></span>     

* [<span data-ttu-id="78e22-175">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="78e22-175">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
* [<span data-ttu-id="78e22-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78e22-176">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [<span data-ttu-id="78e22-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78e22-177">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [<span data-ttu-id="78e22-178">PowerShell</span><span class="sxs-lookup"><span data-stu-id="78e22-178">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
* [<span data-ttu-id="78e22-179">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="78e22-179">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="78e22-180">REST API</span><span class="sxs-lookup"><span data-stu-id="78e22-180">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
* [<span data-ttu-id="78e22-181">.NET API</span><span class="sxs-lookup"><span data-stu-id="78e22-181">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> <span data-ttu-id="78e22-182">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="78e22-182">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="78e22-183">Neprovádí transformaci vstupních dat, aby vytvořil výstupní data.</span><span class="sxs-lookup"><span data-stu-id="78e22-183">It does not transform input data to produce output data.</span></span> <span data-ttu-id="78e22-184">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření prvního kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="78e22-184">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>
> 
> <span data-ttu-id="78e22-185">Dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="78e22-185">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="78e22-186">Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="78e22-186">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 
