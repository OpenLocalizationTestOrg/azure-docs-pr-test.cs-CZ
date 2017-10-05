---
title: "Kurz: Vytvoření kanálu pomocí průvodce kopírováním | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí průvodce kopírováním podporovaného službou Data Factory"
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
ms.openlocfilehash: 5922c050cc09236ba5fdec885a70d11da20135cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a><span data-ttu-id="16ae6-103">Kurz: Vytvoření kanálu s aktivitou kopírování pomocí průvodce kopírováním služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="16ae6-103">Tutorial: Create a pipeline with Copy Activity using Data Factory Copy Wizard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="16ae6-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="16ae6-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="16ae6-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="16ae6-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="16ae6-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="16ae6-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="16ae6-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16ae6-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="16ae6-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="16ae6-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="16ae6-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="16ae6-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="16ae6-110">REST API</span><span class="sxs-lookup"><span data-stu-id="16ae6-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="16ae6-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="16ae6-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="16ae6-112">V tomto kurzu se dozvíte, jak používat **Průvodce kopírováním** ke zkopírování dat z úložiště objektů blob v Azure do databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="16ae6-112">This tutorial shows you how to use the **Copy Wizard** to copy data from an Azure blob storage to an Azure SQL database.</span></span> 

<span data-ttu-id="16ae6-113">**Průvodce kopírováním**  Azure Data Factory vám umožní rychle vytvořit datové kanály, které kopírují data z podporovaných zdrojů úložišť dat do podporovaných cílů úložišť dat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-113">The Azure Data Factory **Copy Wizard** allows you to quickly create a data pipeline that copies data from a supported source data store to a supported destination data store.</span></span> <span data-ttu-id="16ae6-114">Proto doporučujeme použít průvodce jako první krok k vytvoření ukázkového kanálu pro svůj scénář pohybu dat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-114">Therefore, we recommend that you use the wizard as a first step to create a sample pipeline for your data movement scenario.</span></span> <span data-ttu-id="16ae6-115">Seznam úložišť dat podporovaných jako zdroje a cíle najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="16ae6-115">For a list of data stores supported as sources and as destinations, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>  

<span data-ttu-id="16ae6-116">Tento návod ukazuje, jak vytvořit objekt pro vytváření dat Azure, spustit Průvodce kopírováním a projít posloupností kroků poskytnutí podrobností o vašem scénáři příjmu/pohybu dat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-116">This tutorial shows you how to create an Azure data factory, launch the Copy Wizard, go through a series of steps to provide details about your data ingestion/movement scenario.</span></span> <span data-ttu-id="16ae6-117">Po dokončení kroků v průvodci se automaticky vytvoří kanál s aktivitou kopírování pro kopírování dat z úložiště Azure Blob Storage do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="16ae6-117">When you finish steps in the wizard, the wizard automatically creates a pipeline with a Copy Activity to copy data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="16ae6-118">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-118">For more information about Copy Activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16ae6-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="16ae6-119">Prerequisites</span></span>
<span data-ttu-id="16ae6-120">Než se pustíte do tohoto kurzu, dokončete požadované kroky uvedené v článku [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-120">Complete prerequisites listed in the [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article before performing this tutorial.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="16ae6-121">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="16ae6-121">Create data factory</span></span>
<span data-ttu-id="16ae6-122">V tomto kroku vytvoříte pomocí webu Azure Portal objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-122">In this step, you use the Azure portal to create an Azure data factory named **ADFTutorialDataFactory**.</span></span>

1. <span data-ttu-id="16ae6-123">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="16ae6-123">Log in to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="16ae6-124">V nabídce v levém horním rohu klikněte na **+ NOVÝ**, potom na **Data + Analýza** a poté klikněte na **Datová továrna**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-124">Click **+ NEW** from the top-left corner, click **Data + analytics**, and click **Data Factory**.</span></span> 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. <span data-ttu-id="16ae6-126">V okně **Nový objekt pro vytváření dat**:</span><span class="sxs-lookup"><span data-stu-id="16ae6-126">In the **New data factory** blade:</span></span>
   
   1. <span data-ttu-id="16ae6-127">Do pole **Název** zadejte **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-127">Enter **ADFTutorialDataFactory** for the **name**.</span></span>
       <span data-ttu-id="16ae6-128">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="16ae6-128">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="16ae6-129">Pokud se zobrazí chyba: `Data factory name “ADFTutorialDataFactory” is not available`, změňte název datové továrny (třeba na váš_název_ADFTutorialDataFactoryDDMMYYYY) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="16ae6-129">If you receive the error: `Data factory name “ADFTutorialDataFactory” is not available`, change the name of the data factory (for example, yournameADFTutorialDataFactoryYYYYMMDD) and try creating again.</span></span> <span data-ttu-id="16ae6-130">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="16ae6-130">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. <span data-ttu-id="16ae6-132">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="16ae6-132">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="16ae6-133">Pro skupinu prostředků proveďte jeden z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="16ae6-133">For Resource Group, do one of the following steps:</span></span> 
      
      - <span data-ttu-id="16ae6-134">Vyberte možnost **Použít existující** a vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="16ae6-134">Select **Use existing** to select an existing resource group.</span></span>
      - <span data-ttu-id="16ae6-135">Vyberte možnost **Vytvořit nový** a zadejte název pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="16ae6-135">Select **Create new** to enter a name for a resource group.</span></span>
          
        <span data-ttu-id="16ae6-136">Některé kroky v tomto kurzu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-136">Some of the steps in this tutorial assume that you use the name: **ADFTutorialResourceGroup** for the resource group.</span></span> <span data-ttu-id="16ae6-137">Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-137">To learn about resource groups, see [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md).</span></span>
   4. <span data-ttu-id="16ae6-138">Vyberte **umístění** pro příslušný objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-138">Select a **location** for the data factory.</span></span>
   5. <span data-ttu-id="16ae6-139">Zaškrtněte políčko **Připnout na řídicí panel** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="16ae6-139">Select **Pin to dashboard** check box at the bottom of the blade.</span></span>  
   6. <span data-ttu-id="16ae6-140">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-140">Click **Create**.</span></span>
      
       ![Okno Nový objekt pro vytváření dat](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. <span data-ttu-id="16ae6-142">Po vytvoření se zobrazí okno **Objekt pro vytváření dat**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="16ae6-142">After the creation is complete, you see the **Data Factory** blade as shown in the following image:</span></span>
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a><span data-ttu-id="16ae6-144">Spuštění průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="16ae6-144">Launch Copy Wizard</span></span>
1. <span data-ttu-id="16ae6-145">V okně Data Factory klikněte na **Kopírovat data [PREVIEW]**. Spustí se **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-145">On the Data Factory blade, click **Copy data [PREVIEW]** to launch the **Copy Wizard**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="16ae6-146">Pokud zjistíte, že se webový prohlížeč zasekl ve fázi „Autorizace…“, zakažte / zrušte zaškrtnutí políčka **Block third party cookies and site data** (Blokovat data souborů cookie a webů třetích stran) v nastavení prohlížeče nebo nechte toto nastavení povolené a vytvořte výjimku pro **login.microsoftonline.com**. Potom zkuste průvodce znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="16ae6-146">If you see that the web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting in the browser settings (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching the wizard again.</span></span>
2. <span data-ttu-id="16ae6-147">Na stránce **Vlastnosti**:</span><span class="sxs-lookup"><span data-stu-id="16ae6-147">In the **Properties** page:</span></span>
   
   1. <span data-ttu-id="16ae6-148">Jako **Název úlohy** zadejte **CopyFromBlobToAzureSql**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-148">Enter **CopyFromBlobToAzureSql** for **Task name**</span></span>
   2. <span data-ttu-id="16ae6-149">Zadejte **popis** (volitelné).</span><span class="sxs-lookup"><span data-stu-id="16ae6-149">Enter **description** (optional).</span></span>
   3. <span data-ttu-id="16ae6-150">Změňte položky **Datum a čas zahájení** a **Datum a čas ukončení** tak, aby datum ukončení bylo nastaveno na dnešek a datum zahájení bylo o pět dnů dříve.</span><span class="sxs-lookup"><span data-stu-id="16ae6-150">Change the **Start date time** and the **End date time** so that the end date is set to today and start date to five days earlier.</span></span>  
   4. <span data-ttu-id="16ae6-151">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-151">Click **Next**.</span></span>  
      
      ![Nástroj pro kopírování – stránka Vlastnosti](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. <span data-ttu-id="16ae6-153">Na stránce **Source data store** (Zdrojové úložiště dat) klikněte na dlaždici **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-153">On the **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="16ae6-154">Tato stránka slouží k zadání zdrojového úložiště dat pro úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="16ae6-154">You use this page to specify the source data store for the copy task.</span></span> 
   
    ![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. <span data-ttu-id="16ae6-156">Na stránce **Specify the Azure Blob storage account** (Zadejte účet Azure Blob Storage):</span><span class="sxs-lookup"><span data-stu-id="16ae6-156">On the **Specify the Azure Blob storage account** page:</span></span>
   
   1. <span data-ttu-id="16ae6-157">Do pole **Název propojené služby** zadejte **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-157">Enter **AzureStorageLinkedService** for **Linked service name**.</span></span>
   2. <span data-ttu-id="16ae6-158">Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-158">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="16ae6-159">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="16ae6-159">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="16ae6-160">V seznamu účtů úložiště Azure dostupných ve zvoleném předplatném vyberte požadovaný **účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-160">Select an **Azure storage account** from the list of Azure storage accounts available in the selected subscription.</span></span> <span data-ttu-id="16ae6-161">Také si můžete zvolit, že chcete zadat nastavení účtu úložiště ručně. Stačí u položky **Account selection method** (Metoda výběru účtu) vybrat možnost **Enter manually** (Zadat ručně) a potom kliknout na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-161">You can also choose to enter storage account settings manually by selecting **Enter manually** option for the **Account selection method**, and then click **Next**.</span></span> 
      
      ![Nástroj pro kopírování – zadání účtu Azure Blob Storage](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. <span data-ttu-id="16ae6-163">Na stránce **Choose the input file or folder** (Zvolte vstupní soubor nebo složku):</span><span class="sxs-lookup"><span data-stu-id="16ae6-163">On **Choose the input file or folder** page:</span></span>
   
   1. <span data-ttu-id="16ae6-164">Klikněte dvakrát na **adftutorial** (složka).</span><span class="sxs-lookup"><span data-stu-id="16ae6-164">Double-click **adftutorial** (folder).</span></span>
   2. <span data-ttu-id="16ae6-165">Vyberte soubor **emp.txt** a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-165">Select **emp.txt**, and click **Choose**</span></span>
      
      ![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. <span data-ttu-id="16ae6-167">Na stránce **Zvolte vstupní soubor nebo složku** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-167">On the **Choose the input file or folder** page, click **Next**.</span></span> <span data-ttu-id="16ae6-168">Nezaškrtávejte políčko **Binární kopie**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-168">Do not select **Binary copy**.</span></span> 
   
    ![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. <span data-ttu-id="16ae6-170">Na stránce **Nastavení formátu souboru** jsou uvedeny oddělovače a schéma, které je automaticky zjištěno průvodcem při analýze souboru.</span><span class="sxs-lookup"><span data-stu-id="16ae6-170">On the **File format settings** page, you see the delimiters and the schema that is auto-detected by the wizard by parsing the file.</span></span> <span data-ttu-id="16ae6-171">Můžete také zadat oddělovače ručně, pokud chcete, aby Průvodce kopírováním zastavil automatické zjišťování, nebo pokud je chcete přepsat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-171">You can also enter the delimiters manually for the copy wizard to stop auto-detecting or to override.</span></span> <span data-ttu-id="16ae6-172">Po zkontrolování oddělovačů a náhledu dat klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-172">Click **Next** after you review the delimiters and preview data.</span></span> 
   
    ![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. <span data-ttu-id="16ae6-174">Na stránce Cílové úložiště dat vyberte možnost **Azure SQL Database** a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-174">On the Destination data store page, select **Azure SQL Database**, and click **Next**.</span></span>
   
    ![Nástroj pro kopírování – volba cílového úložiště](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. <span data-ttu-id="16ae6-176">Na stránce **Specify the Azure SQL database** (Určete databázi SQL Azure):</span><span class="sxs-lookup"><span data-stu-id="16ae6-176">On **Specify the Azure SQL database** page:</span></span>
   
   1. <span data-ttu-id="16ae6-177">Do pole **Název připojení** zadejte hodnotu **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-177">Enter **AzureSqlLinkedService** for the **Connection name** field.</span></span>
   2. <span data-ttu-id="16ae6-178">Ujistěte se, že je pro položku **Metoda výběru serveru/databáze** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-178">Confirm that **From Azure subscriptions** option is selected for **Server / database selection method**.</span></span>
   3. <span data-ttu-id="16ae6-179">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="16ae6-179">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="16ae6-180">Vyberte možnosti v polích **Název serveru** a **Databáze**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-180">Select **Server name** and **Database**.</span></span>
   5. <span data-ttu-id="16ae6-181">Zadejte **Uživatelské jméno** a **Heslo**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-181">Enter **User name** and **Password**.</span></span>
   6. <span data-ttu-id="16ae6-182">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-182">Click **Next**.</span></span>  
      
      ![Nástroj pro kopírování – určení Azure SQL Database](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. <span data-ttu-id="16ae6-184">Na stránce **Mapování tabulek** vyberte v rozevíracím seznamu poli **Cíl** možnost **emp** a potom klikněte na **šipku dolů** (volitelné). Tím zobrazíte schéma a náhled dat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-184">On the **Table mapping** page, select **emp** for the **Destination** field from the drop-down list, click **down arrow** (optional) to see the schema and to preview the data.</span></span>
    
     ![Nástroj pro kopírování – mapování tabulek](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. <span data-ttu-id="16ae6-186">Na stránce **Schema mapping** (Mapování schématu) klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-186">On the **Schema mapping** page, click **Next**.</span></span>
    
    ![Nástroj pro kopírování – mapování schématu](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. <span data-ttu-id="16ae6-188">Na stránce **Nastavení výkonu** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-188">On the **Performance settings** page, click **Next**.</span></span> 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. <span data-ttu-id="16ae6-190">Na stránce **Souhrn** zkontrolujte informace a klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-190">Review information in the **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="16ae6-191">Průvodce v objektu pro vytváření dat (ze kterého jste průvodce kopírováním spustili) vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál.</span><span class="sxs-lookup"><span data-stu-id="16ae6-191">The wizard creates two linked services, two datasets (input and output), and one pipeline in the data factory (from where you launched the Copy Wizard).</span></span> 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a><span data-ttu-id="16ae6-193">Spuštění aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="16ae6-193">Launch Monitor and Manage application</span></span>
1. <span data-ttu-id="16ae6-194">Na stránce **Nasazení** klikněte na následující odkaz: `Click here to monitor copy pipeline`.</span><span class="sxs-lookup"><span data-stu-id="16ae6-194">On the **Deployment** page, click the link: `Click here to monitor copy pipeline`.</span></span>
   
   ![Nástroj pro kopírování – nasazení bylo úspěšné](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. <span data-ttu-id="16ae6-196">Monitorovací aplikace se spouští na samostatné kartě ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="16ae6-196">The monitoring application is launched in a separate tab in your web browser.</span></span>   
   
   ![Monitorovací aplikace](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. <span data-ttu-id="16ae6-198">Kliknutím na tlačítko **Aktualizovat** v seznamu **OKNA AKTIVITY** ve spodní části zobrazíte nejnovější stav hodinových řezů.</span><span class="sxs-lookup"><span data-stu-id="16ae6-198">To see the latest status of hourly slices, click **Refresh** button in the **ACTIVITY WINDOWS** list at the bottom.</span></span> <span data-ttu-id="16ae6-199">Uvidíte pět oken aktivity, každé pro jeden den mezi počátečním a koncovým časem pro kanál.</span><span class="sxs-lookup"><span data-stu-id="16ae6-199">You see five activity windows for five days between start and end times for the pipeline.</span></span> <span data-ttu-id="16ae6-200">Seznam se automaticky neobnovuje, takže možná budete muset několikrát kliknout na tlačítko Aktualizovat, než uvidíte všechna okna aktivity ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="16ae6-200">The list is not automatically refreshed, so you may need to click Refresh a couple of times before you see all the activity windows in the Ready state.</span></span> 
4. <span data-ttu-id="16ae6-201">Vyberte ze seznamu okno aktivity.</span><span class="sxs-lookup"><span data-stu-id="16ae6-201">Select an activity window in the list.</span></span> <span data-ttu-id="16ae6-202">Jeho podrobnosti si zobrazíte napravo v **Průzkumníku okna aktivity**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-202">See the details about it in the **Activity Window Explorer** on the right.</span></span>

    ![Podrobnosti o okně aktivity](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    <span data-ttu-id="16ae6-204">Všimněte si, že data 11., 12., 13., 14. a 15. jsou zelená, což znamená, že denní výstup řezů byl u těchto dnů už vytvořen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-204">Notice that the dates 11, 12, 13, 14, and 15 are in green color, which means that the daily output slices for these dates have already been produced.</span></span> <span data-ttu-id="16ae6-205">Toto barevné kódování můžete také vidět u kanálu a výstupu datové sady v zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="16ae6-205">You also see this color coding on the pipeline and the output dataset in the diagram view.</span></span> <span data-ttu-id="16ae6-206">Všimněte si, že v předchozím kroku byly už vytvořeny dva řezy, jeden řez se právě zpracovává a ty další dva na zpracování čekají (podle barevného kódování).</span><span class="sxs-lookup"><span data-stu-id="16ae6-206">In the previous step, notice that two slices have already been produced, one slice is currently being processed, and the other two are waiting to be processed (based on the color coding).</span></span> 

    <span data-ttu-id="16ae6-207">Další informace o používání této aplikace najdete v článku [Monitorování a správa kanálu pomocí monitorovací aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-207">For more information on using this application, see [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16ae6-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16ae6-208">Next steps</span></span>
<span data-ttu-id="16ae6-209">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="16ae6-209">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="16ae6-210">Následující tabulka obsahuje seznam úložišť dat podporovaných jako zdroje a cíle aktivitou kopírování:</span><span class="sxs-lookup"><span data-stu-id="16ae6-210">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="16ae6-211">Podrobnosti o polích nebo vlastnostech, které vidíte v průvodci kopírováním pro úložiště dat získáte po kliknutí na odkaz úložiště dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="16ae6-211">For details about fields/properties that you see in the copy wizard for a data store, click the link for the data store in the table.</span></span> 