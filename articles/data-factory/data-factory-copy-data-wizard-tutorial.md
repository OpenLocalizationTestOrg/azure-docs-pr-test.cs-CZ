---
title: "Kurz: Vytvoření kanálu pomocí průvodce kopírováním | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí Průvodce kopírováním podporovaného službou Data Factory hello"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 567b89e7a54c245c134cd0674690e6f3499b46d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a><span data-ttu-id="c9a4c-103">Kurz: Vytvoření kanálu s aktivitou kopírování pomocí průvodce kopírováním služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-103">Tutorial: Create a pipeline with Copy Activity using Data Factory Copy Wizard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9a4c-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="c9a4c-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="c9a4c-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="c9a4c-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="c9a4c-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c9a4c-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="c9a4c-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9a4c-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="c9a4c-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9a4c-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="c9a4c-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="c9a4c-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="c9a4c-110">REST API</span><span class="sxs-lookup"><span data-stu-id="c9a4c-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="c9a4c-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="c9a4c-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="c9a4c-112">Tento kurz ukazuje, jak toouse hello **Průvodce kopírováním** toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-112">This tutorial shows you how toouse hello **Copy Wizard** toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 

<span data-ttu-id="c9a4c-113">Hello Azure Data Factory **Průvodce kopírováním** vám umožní vytvořit tooquickly datovém kanálu, který kopíruje data z podporované zdrojové data store tooa podporované cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-113">hello Azure Data Factory **Copy Wizard** allows you tooquickly create a data pipeline that copies data from a supported source data store tooa supported destination data store.</span></span> <span data-ttu-id="c9a4c-114">Proto doporučujeme pro váš scénář přesun dat pomocí Průvodce hello jako první krok toocreate ukázkový kanál služby.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-114">Therefore, we recommend that you use hello wizard as a first step toocreate a sample pipeline for your data movement scenario.</span></span> <span data-ttu-id="c9a4c-115">Seznam úložišť dat podporovaných jako zdroje a cíle najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-115">For a list of data stores supported as sources and as destinations, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>  

<span data-ttu-id="c9a4c-116">Tento kurz ukazuje, jak toocreate služby Azure data factory, hello spuštění Průvodce kopírováním přejít pomocí několika kroků tooprovide podrobnosti o váš scénář přijímání nebo přesun dat.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-116">This tutorial shows you how toocreate an Azure data factory, launch hello Copy Wizard, go through a series of steps tooprovide details about your data ingestion/movement scenario.</span></span> <span data-ttu-id="c9a4c-117">Po dokončení kroků v Průvodci hello hello průvodce automaticky vytvoří kanál s aktivitou kopírování toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-117">When you finish steps in hello wizard, hello wizard automatically creates a pipeline with a Copy Activity toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="c9a4c-118">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-118">For more information about Copy Activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9a4c-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9a4c-119">Prerequisites</span></span>
<span data-ttu-id="c9a4c-120">Dokončení požadavky uvedené v hello [přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článek před provedením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-120">Complete prerequisites listed in hello [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article before performing this tutorial.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="c9a4c-121">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="c9a4c-121">Create data factory</span></span>
<span data-ttu-id="c9a4c-122">V tomto kroku použijete hello Azure portálu toocreate objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-122">In this step, you use hello Azure portal toocreate an Azure data factory named **ADFTutorialDataFactory**.</span></span>

1. <span data-ttu-id="c9a4c-123">Přihlaste se příliš[portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-123">Log in too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c9a4c-124">Klikněte na tlačítko **+ nový** z levého horního rohu hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-124">Click **+ NEW** from hello top-left corner, click **Data + analytics**, and click **Data Factory**.</span></span> 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. <span data-ttu-id="c9a4c-126">V hello **nový objekt pro vytváření dat** okno:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-126">In hello **New data factory** blade:</span></span>
   
   1. <span data-ttu-id="c9a4c-127">Zadejte **ADFTutorialDataFactory** pro hello **název**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-127">Enter **ADFTutorialDataFactory** for hello **name**.</span></span>
       <span data-ttu-id="c9a4c-128">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-128">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="c9a4c-129">Pokud se zobrazí chyba hello: `Data factory name “ADFTutorialDataFactory” is not available`, změňte hello název objektu pro vytváření dat hello (například yournameADFTutorialDataFactoryYYYYMMDD) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-129">If you receive hello error: `Data factory name “ADFTutorialDataFactory” is not available`, change hello name of hello data factory (for example, yournameADFTutorialDataFactoryYYYYMMDD) and try creating again.</span></span> <span data-ttu-id="c9a4c-130">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-130">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. <span data-ttu-id="c9a4c-132">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-132">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="c9a4c-133">Pro skupinu prostředků proveďte jednu z následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-133">For Resource Group, do one of hello following steps:</span></span> 
      
      - <span data-ttu-id="c9a4c-134">Vyberte **použít existující** tooselect existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-134">Select **Use existing** tooselect an existing resource group.</span></span>
      - <span data-ttu-id="c9a4c-135">Vyberte **vytvořit nový** tooenter název pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-135">Select **Create new** tooenter a name for a resource group.</span></span>
          
        <span data-ttu-id="c9a4c-136">Některé hello kroky v tomto kurzu vychází z předpokladu, že použijete název hello: **ADFTutorialResourceGroup** pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-136">Some of hello steps in this tutorial assume that you use hello name: **ADFTutorialResourceGroup** for hello resource group.</span></span> <span data-ttu-id="c9a4c-137">toolearn o skupinách prostředků najdete v části [pomocí prostředků skupiny prostředků Azure toomanage](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-137">toolearn about resource groups, see [Using resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-overview.md).</span></span>
   4. <span data-ttu-id="c9a4c-138">Vyberte **umístění** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-138">Select a **location** for hello data factory.</span></span>
   5. <span data-ttu-id="c9a4c-139">Vyberte **Pin toodashboard** políčko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-139">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>  
   6. <span data-ttu-id="c9a4c-140">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-140">Click **Create**.</span></span>
      
       ![Okno Nový objekt pro vytváření dat](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. <span data-ttu-id="c9a4c-142">Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-142">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image:</span></span>
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a><span data-ttu-id="c9a4c-144">Spuštění průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="c9a4c-144">Launch Copy Wizard</span></span>
1. <span data-ttu-id="c9a4c-145">V okně hello objekt pro vytváření dat, klikněte na tlačítko **kopírování dat [PREVIEW]** toolaunch hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-145">On hello Data Factory blade, click **Copy data [PREVIEW]** toolaunch hello **Copy Wizard**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="c9a4c-146">Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení v nastavení prohlížeče hello (nebo) se povolit udržování a vytvořte výjimku pro  **Login.microsoftonline.com** a poté se pokuste spustit hello průvodce znovu.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-146">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting in hello browser settings (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
2. <span data-ttu-id="c9a4c-147">V hello **vlastnosti** stránky:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-147">In hello **Properties** page:</span></span>
   
   1. <span data-ttu-id="c9a4c-148">Jako **Název úlohy** zadejte **CopyFromBlobToAzureSql**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-148">Enter **CopyFromBlobToAzureSql** for **Task name**</span></span>
   2. <span data-ttu-id="c9a4c-149">Zadejte **popis** (volitelné).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-149">Enter **description** (optional).</span></span>
   3. <span data-ttu-id="c9a4c-150">Změna hello **datum a čas zahájení** a hello **datum a čas ukončení** tak, aby hello koncové datum je nastaví tootoday a spustí datum toofive dříve dnů.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-150">Change hello **Start date time** and hello **End date time** so that hello end date is set tootoday and start date toofive days earlier.</span></span>  
   4. <span data-ttu-id="c9a4c-151">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-151">Click **Next**.</span></span>  
      
      ![Nástroj pro kopírování – stránka Vlastnosti](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. <span data-ttu-id="c9a4c-153">Na hello **zdrojového úložiště dat** klikněte na tlačítko **Azure Blob Storage** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-153">On hello **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="c9a4c-154">Tato stránka toospecify hello zdrojového úložiště dat se používá pro úlohy kopie hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-154">You use this page toospecify hello source data store for hello copy task.</span></span> 
   
    ![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. <span data-ttu-id="c9a4c-156">Na hello **zadejte účet úložiště Azure Blob hello** stránky:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-156">On hello **Specify hello Azure Blob storage account** page:</span></span>
   
   1. <span data-ttu-id="c9a4c-157">Do pole **Název propojené služby** zadejte **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-157">Enter **AzureStorageLinkedService** for **Linked service name**.</span></span>
   2. <span data-ttu-id="c9a4c-158">Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-158">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="c9a4c-159">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-159">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="c9a4c-160">Vyberte **účtu úložiště Azure** z hello účty k dispozici v rámci předplatného hello vybraný seznam úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-160">Select an **Azure storage account** from hello list of Azure storage accounts available in hello selected subscription.</span></span> <span data-ttu-id="c9a4c-161">Můžete také tooenter nastavení účtu úložiště ručně tak, že vyberete **zadat ručně** možnost pro hello **účet metodu výběru**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-161">You can also choose tooenter storage account settings manually by selecting **Enter manually** option for hello **Account selection method**, and then click **Next**.</span></span> 
      
      ![Nástroj pro kopírování – zadání účtu Azure Blob storage hello](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. <span data-ttu-id="c9a4c-163">Na **zvolte hello vstupního souboru nebo složky** stránky:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-163">On **Choose hello input file or folder** page:</span></span>
   
   1. <span data-ttu-id="c9a4c-164">Klikněte dvakrát na **adftutorial** (složka).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-164">Double-click **adftutorial** (folder).</span></span>
   2. <span data-ttu-id="c9a4c-165">Vyberte soubor **emp.txt** a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-165">Select **emp.txt**, and click **Choose**</span></span>
      
      ![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. <span data-ttu-id="c9a4c-167">Na hello **zvolte hello vstupního souboru nebo složky** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-167">On hello **Choose hello input file or folder** page, click **Next**.</span></span> <span data-ttu-id="c9a4c-168">Nezaškrtávejte políčko **Binární kopie**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-168">Do not select **Binary copy**.</span></span> 
   
    ![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. <span data-ttu-id="c9a4c-170">Na hello **nastavení formátu souboru** stránky, zobrazí hello oddělovače a hello schématu, která je automaticky nalezeny průvodcem hello analýzou soubor hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-170">On hello **File format settings** page, you see hello delimiters and hello schema that is auto-detected by hello wizard by parsing hello file.</span></span> <span data-ttu-id="c9a4c-171">Můžete také zadat hello oddělovače ručně hello kopie Průvodce toostop auto zjišťování nebo toooverride.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-171">You can also enter hello delimiters manually for hello copy wizard toostop auto-detecting or toooverride.</span></span> <span data-ttu-id="c9a4c-172">Klikněte na tlačítko **Další** po zkontrolujte hello oddělovače a zobrazte náhled dat.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-172">Click **Next** after you review hello delimiters and preview data.</span></span> 
   
    ![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. <span data-ttu-id="c9a4c-174">Na cílovém data hello uložení stránky, vyberte **Azure SQL Database**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-174">On hello Destination data store page, select **Azure SQL Database**, and click **Next**.</span></span>
   
    ![Nástroj pro kopírování – volba cílového úložiště](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. <span data-ttu-id="c9a4c-176">Na **zadejte hello Azure SQL database** stránky:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-176">On **Specify hello Azure SQL database** page:</span></span>
   
   1. <span data-ttu-id="c9a4c-177">Zadejte **AzureSqlLinkedService** pro hello **název připojení** pole.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-177">Enter **AzureSqlLinkedService** for hello **Connection name** field.</span></span>
   2. <span data-ttu-id="c9a4c-178">Ujistěte se, že je pro položku **Metoda výběru serveru/databáze** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-178">Confirm that **From Azure subscriptions** option is selected for **Server / database selection method**.</span></span>
   3. <span data-ttu-id="c9a4c-179">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-179">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="c9a4c-180">Vyberte možnosti v polích **Název serveru** a **Databáze**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-180">Select **Server name** and **Database**.</span></span>
   5. <span data-ttu-id="c9a4c-181">Zadejte **Uživatelské jméno** a **Heslo**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-181">Enter **User name** and **Password**.</span></span>
   6. <span data-ttu-id="c9a4c-182">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-182">Click **Next**.</span></span>  
      
      ![Nástroj pro kopírování – určení Azure SQL Database](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. <span data-ttu-id="c9a4c-184">Na hello **mapování tabulky** vyberte **emp** pro hello **cílové** pole hello rozevíracího seznamu, klikněte na tlačítko **šipka dolů** (volitelné) toosee hello schéma a toopreview hello data.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-184">On hello **Table mapping** page, select **emp** for hello **Destination** field from hello drop-down list, click **down arrow** (optional) toosee hello schema and toopreview hello data.</span></span>
    
     ![Nástroj pro kopírování – mapování tabulek](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. <span data-ttu-id="c9a4c-186">Na hello **schéma mapování** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-186">On hello **Schema mapping** page, click **Next**.</span></span>
    
    ![Nástroj pro kopírování – mapování schématu](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. <span data-ttu-id="c9a4c-188">Na hello **nastavení výkonu** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-188">On hello **Performance settings** page, click **Next**.</span></span> 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. <span data-ttu-id="c9a4c-190">Zkontrolujte informace v hello **Souhrn** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-190">Review information in hello **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="c9a4c-191">Hello Průvodce vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál v objektu pro vytváření dat hello (z kde spouštěna hello Průvodce kopírováním).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-191">hello wizard creates two linked services, two datasets (input and output), and one pipeline in hello data factory (from where you launched hello Copy Wizard).</span></span> 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a><span data-ttu-id="c9a4c-193">Spuštění aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="c9a4c-193">Launch Monitor and Manage application</span></span>
1. <span data-ttu-id="c9a4c-194">Na hello **nasazení** klikněte na odkaz hello: `Click here toomonitor copy pipeline`.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-194">On hello **Deployment** page, click hello link: `Click here toomonitor copy pipeline`.</span></span>
   
   ![Nástroj pro kopírování – nasazení bylo úspěšné](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. <span data-ttu-id="c9a4c-196">monitorování aplikace Hello se spustí na samostatné kartě ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-196">hello monitoring application is launched in a separate tab in your web browser.</span></span>   
   
   ![Monitorovací aplikace](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. <span data-ttu-id="c9a4c-198">toosee hello poslední stav hodinové řezů, klikněte na tlačítko **aktualizovat** tlačítka na hello **aktivity WINDOWS** seznam v dolní části hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-198">toosee hello latest status of hourly slices, click **Refresh** button in hello **ACTIVITY WINDOWS** list at hello bottom.</span></span> <span data-ttu-id="c9a4c-199">Uvidíte pět aktivity windows pět dní mezi počáteční a koncový čas pro kanál hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-199">You see five activity windows for five days between start and end times for hello pipeline.</span></span> <span data-ttu-id="c9a4c-200">seznam Hello automaticky neobnoví, tak mohou potřebovat tooclick aktualizovat několik doby, než uvidíte všechny systémy windows hello aktivity ve stavu Připraveno hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-200">hello list is not automatically refreshed, so you may need tooclick Refresh a couple of times before you see all hello activity windows in hello Ready state.</span></span> 
4. <span data-ttu-id="c9a4c-201">Okno s aktivity vyberte v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-201">Select an activity window in hello list.</span></span> <span data-ttu-id="c9a4c-202">Zobrazit podrobnosti hello o ho v hello **aktivity okno Průzkumníka** na hello správné.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-202">See hello details about it in hello **Activity Window Explorer** on hello right.</span></span>

    ![Podrobnosti o okně aktivity](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    <span data-ttu-id="c9a4c-204">Všimněte si, že jsou data hello 11, 12, 13, 14 nebo 15 v zelenou barvu, která znamená, že hello denní výstup řezy pro těmito daty už vytvořily.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-204">Notice that hello dates 11, 12, 13, 14, and 15 are in green color, which means that hello daily output slices for these dates have already been produced.</span></span> <span data-ttu-id="c9a4c-205">Můžete také najdete v části barevné kódování na hello kanálu a hello výstupní datovou sadu v zobrazení diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-205">You also see this color coding on hello pipeline and hello output dataset in hello diagram view.</span></span> <span data-ttu-id="c9a4c-206">V předchozím kroku hello Všimněte si, že už vytvořily dva řezy, jeden řez je právě zpracovávána, a hello další dvě čekají toobe zpracovat (podle hello barevné kódování).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-206">In hello previous step, notice that two slices have already been produced, one slice is currently being processed, and hello other two are waiting toobe processed (based on hello color coding).</span></span> 

    <span data-ttu-id="c9a4c-207">Další informace o používání této aplikace najdete v článku [Monitorování a správa kanálu pomocí monitorovací aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="c9a4c-207">For more information on using this application, see [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9a4c-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9a4c-208">Next steps</span></span>
<span data-ttu-id="c9a4c-209">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-209">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="c9a4c-210">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello:</span><span class="sxs-lookup"><span data-stu-id="c9a4c-210">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="c9a4c-211">Podrobnosti o pole nebo vlastnosti, které se zobrazí v Průvodci kopírováním hello pro úložiště dat klikněte na odkaz hello hello úložiště dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="c9a4c-211">For details about fields/properties that you see in hello copy wizard for a data store, click hello link for hello data store in hello table.</span></span> 
