---
title: "aaaCopy data z úložiště objektů Blob tooSQL databázi – Azure | Microsoft Docs"
description: "Tento kurz ukazuje, jak toouse aktivitu kopírování v objektu pro vytváření dat Azure kanálu toocopy data z databáze tooSQL úložiště objektů Blob."
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
ms.openlocfilehash: a2c3fb8a4ddd63b0b6b3e75903b7a7eaf188fda4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-copy-data-from-blob-storage-toosql-database-using-data-factory"></a><span data-ttu-id="23cb5-104">Kurz: Kopírování dat z úložiště objektů Blob tooSQL databáze pomocí objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="23cb5-104">Tutorial: Copy data from Blob Storage tooSQL Database using Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="23cb5-105">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="23cb5-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="23cb5-106">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="23cb5-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="23cb5-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="23cb5-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="23cb5-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="23cb5-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="23cb5-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="23cb5-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="23cb5-110">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="23cb5-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="23cb5-111">REST API</span><span class="sxs-lookup"><span data-stu-id="23cb5-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="23cb5-112">.NET API</span><span class="sxs-lookup"><span data-stu-id="23cb5-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="23cb5-113">V tomto kurzu vytvoříte objekt pro vytváření dat kanál toocopy dat z databáze tooSQL úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="23cb5-113">In this tutorial, you create a data factory with a pipeline toocopy data from Blob storage tooSQL database.</span></span>

<span data-ttu-id="23cb5-114">Hello aktivita kopírování provádí přesun dat hello v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="23cb5-114">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="23cb5-115">Používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="23cb5-115">It is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="23cb5-116">V tématu [aktivity přesunu dat](data-factory-data-movement-activities.md) článku podrobnosti o aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="23cb5-116">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>  

> [!NOTE]
> <span data-ttu-id="23cb5-117">Podrobný přehled o hello služba Data Factory, najdete v části hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článku.</span><span class="sxs-lookup"><span data-stu-id="23cb5-117">For a detailed overview of hello Data Factory service, see hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article.</span></span>
>
>

## <a name="prerequisites-for-hello-tutorial"></a><span data-ttu-id="23cb5-118">Předpoklady pro kurz hello</span><span class="sxs-lookup"><span data-stu-id="23cb5-118">Prerequisites for hello tutorial</span></span>
<span data-ttu-id="23cb5-119">Než začnete tento kurz, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="23cb5-119">Before you begin this tutorial, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="23cb5-120">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-120">**Azure subscription**.</span></span>  <span data-ttu-id="23cb5-121">Pokud nemáte předplatné, můžete si během několika minut bezplatně vytvořit zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="23cb5-121">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="23cb5-122">V tématu hello [bezplatné zkušební verze](http://azure.microsoft.com/pricing/free-trial/) článku.</span><span class="sxs-lookup"><span data-stu-id="23cb5-122">See hello [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="23cb5-123">**Účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-123">**Azure Storage Account**.</span></span> <span data-ttu-id="23cb5-124">Použít úložiště objektů blob hello jako **zdroj** úložiště dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-124">You use hello blob storage as a **source** data store in this tutorial.</span></span> <span data-ttu-id="23cb5-125">Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="23cb5-125">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
* <span data-ttu-id="23cb5-126">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-126">**Azure SQL Database**.</span></span> <span data-ttu-id="23cb5-127">Použít databázi Azure SQL jako **cílové** úložiště dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-127">You use an Azure SQL database as a **destination** data store in this tutorial.</span></span> <span data-ttu-id="23cb5-128">Pokud nemáte Azure SQL database, můžete použít v hello kurzu, najdete v tématu [jak toocreate a konfigurace Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="23cb5-128">If you don't have an Azure SQL database that you can use in hello tutorial, See [How toocreate and configure an Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate one.</span></span>
* <span data-ttu-id="23cb5-129">**SQL Server 2012 nebo 2014 nebo Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-129">**SQL Server 2012/2014 or Visual Studio 2013**.</span></span> <span data-ttu-id="23cb5-130">Použít SQL Server Management Studio nebo Visual Studio toocreate ukázkovou databázi a tooview hello Výsledná data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="23cb5-130">You use SQL Server Management Studio or Visual Studio toocreate a sample database and tooview hello result data in hello database.</span></span>  

## <a name="collect-blob-storage-account-name-and-key"></a><span data-ttu-id="23cb5-131">Shromažďovat název účtu úložiště objektů blob a klíč</span><span class="sxs-lookup"><span data-stu-id="23cb5-131">Collect blob storage account name and key</span></span>
<span data-ttu-id="23cb5-132">Budete potřebovat účet hello názvu a klíče účtu úložiště Azure účet toodo v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-132">You need hello account name and account key of your Azure storage account toodo this tutorial.</span></span> <span data-ttu-id="23cb5-133">Poznamenejte si **název účtu** a **klíč účtu** pro váš účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="23cb5-133">Note down **account name** and **account key** for your Azure storage account.</span></span>

1. <span data-ttu-id="23cb5-134">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="23cb5-134">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="23cb5-135">Klikněte na tlačítko **další služby** v levé nabídce vyberte hello **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-135">Click **More services** on hello left menu and select **Storage Accounts**.</span></span>

    ![Procházet - účty úložiště](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. <span data-ttu-id="23cb5-137">V hello **účty úložiště** okně, vyberte hello **účtu úložiště Azure** chcete toouse v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-137">In hello **Storage Accounts** blade, select hello **Azure storage account** that you want toouse in this tutorial.</span></span>
4. <span data-ttu-id="23cb5-138">Vyberte **přístupové klíče** v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-138">Select **Access keys** link under **SETTINGS**.</span></span>
5. <span data-ttu-id="23cb5-139">Klikněte na tlačítko **kopie** (obrázek) tlačítko vedle příliš**název účtu úložiště** textové pole a uložit a vložte ji někam (například: do textového souboru).</span><span class="sxs-lookup"><span data-stu-id="23cb5-139">Click **copy** (image) button next too**Storage account name** text box and save/paste it somewhere (for example: in a text file).</span></span>
6. <span data-ttu-id="23cb5-140">Opakujte předchozí krok toocopy hello nebo zapište hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-140">Repeat hello previous step toocopy or note down hello **key1**.</span></span>

    ![Přístupový klíč k úložišti](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. <span data-ttu-id="23cb5-142">Zavřete všechna okna hello kliknutím **X**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-142">Close all hello blades by clicking **X**.</span></span>

## <a name="collect-sql-server-database-user-names"></a><span data-ttu-id="23cb5-143">Shromažďování systému SQL server, databáze, uživatelská jména</span><span class="sxs-lookup"><span data-stu-id="23cb5-143">Collect SQL server, database, user names</span></span>
<span data-ttu-id="23cb5-144">Názvy hello serveru Azure SQL, databáze a toodo uživatele je nutné v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-144">You need hello names of Azure SQL server, database, and user toodo this tutorial.</span></span> <span data-ttu-id="23cb5-145">Zapište názvy **server**, **databáze**, a **uživatele** pro vaši databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="23cb5-145">Note down names of **server**, **database**, and **user** for your Azure SQL database.</span></span>

1. <span data-ttu-id="23cb5-146">V hello **portál Azure**, klikněte na tlačítko **další služby** na hello vlevo a vyberte **databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-146">In hello **Azure portal**, click **More services** on hello left and select **SQL databases**.</span></span>
2. <span data-ttu-id="23cb5-147">V hello **okna databáze SQL**, vyberte hello **databáze** chcete toouse v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-147">In hello **SQL databases blade**, select hello **database** that you want toouse in this tutorial.</span></span> <span data-ttu-id="23cb5-148">Zapište hello **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-148">Note down hello **database name**.</span></span>  
3. <span data-ttu-id="23cb5-149">V hello **databáze SQL** okně klikněte na tlačítko **vlastnosti** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-149">In hello **SQL database** blade, click **Properties** under **SETTINGS**.</span></span>
4. <span data-ttu-id="23cb5-150">Zapište hello hodnoty pro **název serveru** a **přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-150">Note down hello values for **SERVER NAME** and **SERVER ADMIN LOGIN**.</span></span>
5. <span data-ttu-id="23cb5-151">Zavřete všechna okna hello kliknutím **X**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-151">Close all hello blades by clicking **X**.</span></span>

## <a name="allow-azure-services-tooaccess-sql-server"></a><span data-ttu-id="23cb5-152">Povolit službám Azure tooaccess SQL serveru</span><span class="sxs-lookup"><span data-stu-id="23cb5-152">Allow Azure services tooaccess SQL server</span></span>
<span data-ttu-id="23cb5-153">Ujistěte se, že **povolit přístup k službám tooAzure** nastavení zapnuté **ON** pro server Azure SQL tak, že hello služba Data Factory k přístup serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="23cb5-153">Ensure that **Allow access tooAzure services** setting turned **ON** for your Azure SQL server so that hello Data Factory service can access your Azure SQL server.</span></span> <span data-ttu-id="23cb5-154">tooverify a zapněte toto nastavení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="23cb5-154">tooverify and turn on this setting, do hello following steps:</span></span>

1. <span data-ttu-id="23cb5-155">Klikněte na tlačítko **další služby** rozbočovače na levé straně hello a klikněte na **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-155">Click **More services** hub on hello left and click **SQL servers**.</span></span>
2. <span data-ttu-id="23cb5-156">Vyberte svůj server a v části **NASTAVENÍ** klikněte na **Brána firewall**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-156">Select your server, and click **Firewall** under **SETTINGS**.</span></span>
3. <span data-ttu-id="23cb5-157">V hello **nastavení brány Firewall** okně klikněte na tlačítko **ON** pro **povolit přístup k službám tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-157">In hello **Firewall settings** blade, click **ON** for **Allow access tooAzure services**.</span></span>
4. <span data-ttu-id="23cb5-158">Zavřete všechna okna hello kliknutím **X**.</span><span class="sxs-lookup"><span data-stu-id="23cb5-158">Close all hello blades by clicking **X**.</span></span>

## <a name="prepare-blob-storage-and-sql-database"></a><span data-ttu-id="23cb5-159">Příprava úložiště objektů Blob a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="23cb5-159">Prepare Blob Storage and SQL Database</span></span>
<span data-ttu-id="23cb5-160">Nyní připravte Azure blob storage a Azure SQL database hello kurzu provedením hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="23cb5-160">Now, prepare your Azure blob storage and Azure SQL database for hello tutorial by performing hello following steps:</span></span>  

1. <span data-ttu-id="23cb5-161">Spusťte Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="23cb5-161">Launch Notepad.</span></span> <span data-ttu-id="23cb5-162">Zkopírujte následující text hello a uložte ho jako **emp.txt** příliš**C:\ADFGetStarted** složky na pevném disku.</span><span class="sxs-lookup"><span data-stu-id="23cb5-162">Copy hello following text and save it as **emp.txt** too**C:\ADFGetStarted** folder on your hard drive.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
2. <span data-ttu-id="23cb5-163">Pomocí nástrojů, jako [Azure Storage Explorer](http://storageexplorer.com/) toocreate hello **adftutorial** kontejneru a tooupload hello **emp.txt** toohello kontejner souborů.</span><span class="sxs-lookup"><span data-stu-id="23cb5-163">Use tools such as [Azure Storage Explorer](http://storageexplorer.com/) toocreate hello **adftutorial** container and tooupload hello **emp.txt** file toohello container.</span></span>

    ![Azure Storage Explorer.](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. <span data-ttu-id="23cb5-166">Použití hello následující SQL skriptu toocreate hello **emp** tabulky v databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="23cb5-166">Use hello following SQL script toocreate hello **emp** table in your Azure SQL Database.</span></span>  

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

    <span data-ttu-id="23cb5-167">**Pokud máte SQL Server 2012 nebo 2014 v počítači nainstalována:** postupujte podle pokynů v [Správa Azure SQL Database pomocí SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server a spustit hello ze skriptu SQL.</span><span class="sxs-lookup"><span data-stu-id="23cb5-167">**If you have SQL Server 2012/2014 installed on your computer:** follow instructions from [Managing Azure SQL Database using SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server and run hello SQL script.</span></span> <span data-ttu-id="23cb5-168">Tento článek používá hello [portál Azure classic](http://manage.windowsazure.com), není hello [nový portál Azure](https://portal.azure.com), tooconfigure brány firewall pro server Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="23cb5-168">This article uses hello [classic Azure portal](http://manage.windowsazure.com), not hello [new Azure portal](https://portal.azure.com), tooconfigure firewall for an Azure SQL server.</span></span>

    <span data-ttu-id="23cb5-169">Pokud klient nemá povolený tooaccess hello Azure SQL server, musíte tooconfigure brány firewall pro přístup k vaší Azure SQL serveru tooallow z vašeho počítače (IP adresa).</span><span class="sxs-lookup"><span data-stu-id="23cb5-169">If your client is not allowed tooaccess hello Azure SQL server, you need tooconfigure firewall for your Azure SQL server tooallow access from your machine (IP Address).</span></span> <span data-ttu-id="23cb5-170">V tématu [v tomto článku](../sql-database/sql-database-configure-firewall-settings.md) kroky tooconfigure hello brány firewall pro server Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="23cb5-170">See [this article](../sql-database/sql-database-configure-firewall-settings.md) for steps tooconfigure hello firewall for your Azure SQL server.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="23cb5-171">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="23cb5-171">Create a data factory</span></span>
<span data-ttu-id="23cb5-172">Když jste dokončili hello požadavky.</span><span class="sxs-lookup"><span data-stu-id="23cb5-172">You have completed hello prerequisites.</span></span> <span data-ttu-id="23cb5-173">Můžete vytvořit objekt pro vytváření dat pomocí jedné z následujících způsobů hello.</span><span class="sxs-lookup"><span data-stu-id="23cb5-173">You can create a data factory using one of hello following ways.</span></span> <span data-ttu-id="23cb5-174">Klikněte na jednu z možností hello v rozevíracím seznamu hello hello horní nebo hello následující odkazy tooperform hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="23cb5-174">Click one of hello options in hello drop-down list at hello top or hello following links tooperform hello tutorial.</span></span>     

* [<span data-ttu-id="23cb5-175">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="23cb5-175">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
* [<span data-ttu-id="23cb5-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="23cb5-176">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [<span data-ttu-id="23cb5-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="23cb5-177">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [<span data-ttu-id="23cb5-178">PowerShell</span><span class="sxs-lookup"><span data-stu-id="23cb5-178">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
* [<span data-ttu-id="23cb5-179">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="23cb5-179">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="23cb5-180">REST API</span><span class="sxs-lookup"><span data-stu-id="23cb5-180">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
* [<span data-ttu-id="23cb5-181">.NET API</span><span class="sxs-lookup"><span data-stu-id="23cb5-181">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> <span data-ttu-id="23cb5-182">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="23cb5-182">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="23cb5-183">Ho nebudou transformovat vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="23cb5-183">It does not transform input data tooproduce output data.</span></span> <span data-ttu-id="23cb5-184">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: vytvoření vaší první dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="23cb5-184">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build your first pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>
> 
> <span data-ttu-id="23cb5-185">Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="23cb5-185">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="23cb5-186">Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="23cb5-186">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 
