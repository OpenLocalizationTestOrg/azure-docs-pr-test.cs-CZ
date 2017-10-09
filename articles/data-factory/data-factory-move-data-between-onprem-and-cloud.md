---
title: "data aaaMove – Brána pro správu dat | Microsoft Docs"
description: "Nastavení dat brány toomove dat mezi místními a hello cloudu. Použijte Brána pro správu dat v Azure Data Factory toomove data."
keywords: "Brána dat, integraci dat. přesun dat, přihlašovací údaje brány"
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 314341c142d5260c785b7e82081774f044450e81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-between-on-premises-sources-and-hello-cloud-with-data-management-gateway"></a><span data-ttu-id="567d4-105">Přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat</span><span class="sxs-lookup"><span data-stu-id="567d4-105">Move data between on-premises sources and hello cloud with Data Management Gateway</span></span>
<span data-ttu-id="567d4-106">Tento článek obsahuje přehled dat integrace mezi místním úložištím dat a cloudové úložiště dat pomocí služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="567d4-106">This article provides an overview of data integration between on-premises data stores and cloud data stores using Data Factory.</span></span> <span data-ttu-id="567d4-107">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek a další data factory základní koncepty články: [datové sady](data-factory-create-datasets.md) a [kanály](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="567d4-107">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article and other data factory core concepts articles: [datasets](data-factory-create-datasets.md) and [pipelines](data-factory-create-pipelines.md).</span></span>

## <a name="data-management-gateway"></a><span data-ttu-id="567d4-108">Brána správy dat</span><span class="sxs-lookup"><span data-stu-id="567d4-108">Data Management Gateway</span></span>
<span data-ttu-id="567d4-109">Brána pro správu dat je nutné nainstalovat na vaše místní počítač tooenable přesun dat z úložiště služby místní data.</span><span class="sxs-lookup"><span data-stu-id="567d4-109">You must install Data Management Gateway on your on-premises machine tooenable moving data to/from an on-premises data store.</span></span> <span data-ttu-id="567d4-110">Hello gateway se dá nainstalovat na stejný počítač jako úložiště dat hello nebo na jiném počítači, dokud hello gateway bude moct připojit úložiště dat toohello hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-110">hello gateway can be installed on hello same machine as hello data store or on a different machine as long as hello gateway can connect toohello data store.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="567d4-111">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="567d4-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> 

<span data-ttu-id="567d4-112">Hello následující postup vám ukáže, jak toocreate objekt pro vytváření dat s kanál, který přesouvá data z místního **systému SQL Server** databáze tooan úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="567d4-112">hello following walkthrough shows you how toocreate a data factory with a pipeline that moves data from an on-premises **SQL Server** database tooan Azure blob storage.</span></span> <span data-ttu-id="567d4-113">Jako součást hello návod nainstalujte a nakonfigurujte hello Brána pro správu dat na počítači.</span><span class="sxs-lookup"><span data-stu-id="567d4-113">As part of hello walkthrough, you install and configure hello Data Management Gateway on your machine.</span></span>

## <a name="walkthrough-copy-on-premises-data-toocloud"></a><span data-ttu-id="567d4-114">Návod: zkopírování toocloud místní data</span><span class="sxs-lookup"><span data-stu-id="567d4-114">Walkthrough: copy on-premises data toocloud</span></span>
<span data-ttu-id="567d4-115">V tomto návodu hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="567d4-115">In this walkthrough you do hello following steps:</span></span> 

1. <span data-ttu-id="567d4-116">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="567d4-116">Create a data factory.</span></span>
2. <span data-ttu-id="567d4-117">Brána pro správu dat vytvořte.</span><span class="sxs-lookup"><span data-stu-id="567d4-117">Create a data management gateway.</span></span> 
3. <span data-ttu-id="567d4-118">Vytvoření propojených služeb pro úložiště dat zdroj a jímka.</span><span class="sxs-lookup"><span data-stu-id="567d4-118">Create linked services for source and sink data stores.</span></span>
4. <span data-ttu-id="567d4-119">Vytvoření datových sad toorepresent vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="567d4-119">Create datasets toorepresent input and output data.</span></span>
5. <span data-ttu-id="567d4-120">Vytvoření kanálu s kopírovat data hello toomove aktivity.</span><span class="sxs-lookup"><span data-stu-id="567d4-120">Create a pipeline with a copy activity toomove hello data.</span></span>

## <a name="prerequisites-for-hello-tutorial"></a><span data-ttu-id="567d4-121">Předpoklady pro kurz hello</span><span class="sxs-lookup"><span data-stu-id="567d4-121">Prerequisites for hello tutorial</span></span>
<span data-ttu-id="567d4-122">Než začnete Tento názorný postup, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="567d4-122">Before you begin this walkthrough, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="567d4-123">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="567d4-123">**Azure subscription**.</span></span>  <span data-ttu-id="567d4-124">Pokud nemáte předplatné, můžete si během několika minut bezplatně vytvořit zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="567d4-124">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="567d4-125">V tématu hello [bezplatné zkušební verze](http://azure.microsoft.com/pricing/free-trial/) článku.</span><span class="sxs-lookup"><span data-stu-id="567d4-125">See hello [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="567d4-126">**Účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="567d4-126">**Azure Storage Account**.</span></span> <span data-ttu-id="567d4-127">Použít úložiště objektů blob hello jako **cílové/jímka** úložiště dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="567d4-127">You use hello blob storage as a **destination/sink** data store in this tutorial.</span></span> <span data-ttu-id="567d4-128">Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="567d4-128">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
* <span data-ttu-id="567d4-129">**SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="567d4-129">**SQL Server**.</span></span> <span data-ttu-id="567d4-130">V tomto kurzu použijete místní databázi SQL Serveru jako **zdrojové** úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="567d4-130">You use an on-premises SQL Server database as a **source** data store in this tutorial.</span></span> 

## <a name="create-data-factory"></a><span data-ttu-id="567d4-131">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="567d4-131">Create data factory</span></span>
<span data-ttu-id="567d4-132">V tomto kroku použijete hello Azure portálu toocreate instanci objektu pro vytváření dat Azure s názvem **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="567d4-132">In this step, you use hello Azure portal toocreate an Azure Data Factory instance named **ADFTutorialOnPremDF**.</span></span>

1. <span data-ttu-id="567d4-133">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="567d4-133">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="567d4-134">Klikněte na tlačítko **+ nový**, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="567d4-134">Click **+ NEW**, click **Intelligence + analytics**, and click **Data Factory**.</span></span>

   ![Nový -> Objekt pro vytváření dat](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. <span data-ttu-id="567d4-136">V hello **nový objekt pro vytváření dat** zadejte **ADFTutorialOnPremDF** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="567d4-136">In hello **New data factory** page, enter **ADFTutorialOnPremDF** for hello Name.</span></span>

    ![Přidat tooStartboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > <span data-ttu-id="567d4-138">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="567d4-138">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="567d4-139">Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "ADFTutorialOnPremDF" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například yournameADFTutorialOnPremDF) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="567d4-139">If you receive hello error: **Data factory name “ADFTutorialOnPremDF” is not available**, change hello name of hello data factory (for example, yournameADFTutorialOnPremDF) and try creating again.</span></span> <span data-ttu-id="567d4-140">Tento název používejte místo ADFTutorialOnPremDF při provádění zbývající kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="567d4-140">Use this name in place of ADFTutorialOnPremDF while performing remaining steps in this tutorial.</span></span>
   >
   > <span data-ttu-id="567d4-141">Hello název objektu pro vytváření dat hello může být registrován jako **DNS** název v budoucnosti hello a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="567d4-141">hello name of hello data factory may be registered as a **DNS** name in hello future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="567d4-142">Vyberte hello **předplatného Azure** místo hello data factory toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="567d4-142">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="567d4-143">Vyberte existující **skupinu prostředků** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="567d4-143">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="567d4-144">Hello kurzu vytvořte skupinu prostředků s názvem: **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="567d4-144">For hello tutorial, create a resource group named: **ADFTutorialResourceGroup**.</span></span>
6. <span data-ttu-id="567d4-145">Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** stránky.</span><span class="sxs-lookup"><span data-stu-id="567d4-145">Click **Create** on hello **New data factory** page.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="567d4-146">instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-146">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="567d4-147">Po dokončení vytvoření se zobrazí hello **Data Factory** stránky, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="567d4-147">After creation is complete, you see hello **Data Factory** page as shown in hello following image:</span></span>

   ![Data Factory domovské stránky](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a><span data-ttu-id="567d4-149">Vytvoření brány</span><span class="sxs-lookup"><span data-stu-id="567d4-149">Create gateway</span></span>
1. <span data-ttu-id="567d4-150">V hello **Data Factory** klikněte na tlačítko **autora a nasazení** dlaždice toolaunch hello **Editor** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="567d4-150">In hello **Data Factory** page, click **Author and deploy** tile toolaunch hello **Editor** for hello data factory.</span></span>

    ![Dlaždice Vytvořit a nasadit](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. <span data-ttu-id="567d4-152">V editoru služby Data Factory hello, klikněte na **... Další** hello panelu nástrojů a pak klikněte na tlačítko **novou bránu dat**.</span><span class="sxs-lookup"><span data-stu-id="567d4-152">In hello Data Factory Editor, click **... More** on hello toolbar and then click **New data gateway**.</span></span> <span data-ttu-id="567d4-153">Alternativně můžete můžete kliknout pravým tlačítkem **brány Data Gateways** v hello stromové zobrazení a klikněte na tlačítko **novou bránu dat**.</span><span class="sxs-lookup"><span data-stu-id="567d4-153">Alternatively, you can right-click **Data Gateways** in hello tree view, and click **New data gateway**.</span></span>

   ![Nová brána dat na panelu nástrojů](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. <span data-ttu-id="567d4-155">V hello **vytvořit** zadejte **adftutorialgateway** pro hello **název**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="567d4-155">In hello **Create** page, enter **adftutorialgateway** for hello **name**, and click **OK**.</span></span>     

    ![Vytvoření brány stránky](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > <span data-ttu-id="567d4-157">V tomto návodu vytvoříte logické brány hello jenom jeden uzel (místní počítač Windows).</span><span class="sxs-lookup"><span data-stu-id="567d4-157">In this walkthrough, you create hello logical gateway with only one node (on-premises Windows machine).</span></span> <span data-ttu-id="567d4-158">Brána pro správu dat můžete škálovat tím, že přidružíte více místní počítače s bránou hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-158">You can scale out a data management gateway by associating multiple on-premises machines with hello gateway.</span></span> <span data-ttu-id="567d4-159">Je možné škálovat až zvýšením počtu úloh přesun dat, které můžou běžet souběžně na uzlu.</span><span class="sxs-lookup"><span data-stu-id="567d4-159">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="567d4-160">Tato funkce je také k dispozici pro logické brány se jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="567d4-160">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="567d4-161">V tématu [škálování Brána pro správu dat v Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) článku.</span><span class="sxs-lookup"><span data-stu-id="567d4-161">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>  
4. <span data-ttu-id="567d4-162">V hello **konfigurace** klikněte na tlačítko **nainstalovat přímo na tomto počítači**.</span><span class="sxs-lookup"><span data-stu-id="567d4-162">In hello **Configure** page, click **Install directly on this computer**.</span></span> <span data-ttu-id="567d4-163">Tato akce stáhne hello instalační balíček brány hello, nainstaluje, konfiguruje a zaregistruje hello bránu v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-163">This action downloads hello installation package for hello gateway, installs, configures, and registers hello gateway on hello computer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="567d4-164">Použijte Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="567d4-164">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>
   >
   > <span data-ttu-id="567d4-165">Pokud používáte Chrome, přejděte toohello [internetového obchodu Chrome](https://chrome.google.com/webstore/), hledání with – klíčové slovo "ClickOnce", vyberte jedno z rozšíření hello ClickOnce a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="567d4-165">If you are using Chrome, go toohello [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>
   >
   > <span data-ttu-id="567d4-166">Hello stejné pro Firefox (instalace doplňku).</span><span class="sxs-lookup"><span data-stu-id="567d4-166">Do hello same for Firefox (install add-in).</span></span> <span data-ttu-id="567d4-167">Klikněte na tlačítko **otevřít nabídku** tlačítka na panelu nástrojů hello (**tři vodorovné čáry** v pravém horním rohu hello), klikněte na tlačítko **doplňky**, hledání with – klíčové slovo "ClickOnce", vyberte jednu z hello ClickOnce rozšíření a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="567d4-167">Click **Open Menu** button on hello toolbar (**three horizontal lines** in hello top-right corner), click **Add-ons**, search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>    
   >
   >

    ![Brána - konfigurace stránky](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    <span data-ttu-id="567d4-169">Tímto způsobem je nejjednodušší způsob (jedním kliknutím) toodownload hello, nainstalovat, nakonfigurovat a zaregistrovat bránu hello v jednom kroku jeden.</span><span class="sxs-lookup"><span data-stu-id="567d4-169">This way is hello easiest way (one-click) toodownload, install, configure, and register hello gateway in one single step.</span></span> <span data-ttu-id="567d4-170">Můžete zobrazit hello **Správce konfigurace brány pro správu dat společnosti Microsoft** je aplikace nainstalována na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="567d4-170">You can see hello **Microsoft Data Management Gateway Configuration Manager** application is installed on your computer.</span></span> <span data-ttu-id="567d4-171">Můžete také vyhledat hello spustitelné **ConfigManager.exe** ve složce hello: **C:\Program Files\Microsoft Data správy Gateway\2.0\Shared**.</span><span class="sxs-lookup"><span data-stu-id="567d4-171">You can also find hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.</span></span>

    <span data-ttu-id="567d4-172">Můžete také stáhnout a nainstalovat bránu ručně pomocí hello odkazů na této stránce a zaregistrovat ji pomocí klíče hello ukazuje hello **nový klíč** textové pole.</span><span class="sxs-lookup"><span data-stu-id="567d4-172">You can also download and install gateway manually by using hello links in this page and register it using hello key shown in hello **NEW KEY** text box.</span></span>

    <span data-ttu-id="567d4-173">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článek pro všechny hello podrobnosti o hello brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-173">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all hello details about hello gateway.</span></span>

   > [!NOTE]
   > <span data-ttu-id="567d4-174">Musíte mít oprávnění správce na místním počítači tooinstall hello a nakonfigurovat hello Brána pro správu dat úspěšně.</span><span class="sxs-lookup"><span data-stu-id="567d4-174">You must be an administrator on hello local computer tooinstall and configure hello Data Management Gateway successfully.</span></span> <span data-ttu-id="567d4-175">Můžete přidat další uživatele toohello **Data Management Gateway Users** místní skupiny Windows.</span><span class="sxs-lookup"><span data-stu-id="567d4-175">You can add additional users toohello **Data Management Gateway Users** local Windows group.</span></span> <span data-ttu-id="567d4-176">Hello členy této skupiny můžete použít hello Správce konfigurace brány pro správu dat nástroj tooconfigure hello brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-176">hello members of this group can use hello Data Management Gateway Configuration Manager tool tooconfigure hello gateway.</span></span>
   >
   >
5. <span data-ttu-id="567d4-177">Počkejte několik minut, nebo počkejte, až se zobrazí následující zprávu oznámení hello:</span><span class="sxs-lookup"><span data-stu-id="567d4-177">Wait for a couple of minutes or wait until you see hello following notification message:</span></span>

    ![Úspěšná instalace brány](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. <span data-ttu-id="567d4-179">Spusťte **Správce konfigurace brány pro správu dat** aplikace ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="567d4-179">Launch **Data Management Gateway Configuration Manager** application on your computer.</span></span> <span data-ttu-id="567d4-180">V hello **vyhledávání** zadejte **Brána pro správu dat** tooaccess tento nástroj.</span><span class="sxs-lookup"><span data-stu-id="567d4-180">In hello **Search** window, type **Data Management Gateway** tooaccess this utility.</span></span> <span data-ttu-id="567d4-181">Můžete také vyhledat hello spustitelné **ConfigManager.exe** ve složce hello: **C:\Program Files\Microsoft Data správy Gateway\2.0\Shared**</span><span class="sxs-lookup"><span data-stu-id="567d4-181">You can also find hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**</span></span>

    ![Správce konfigurace brány](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. <span data-ttu-id="567d4-183">Zkontrolujte, jestli `adftutorialgateway is connected toohello cloud service` zprávy.</span><span class="sxs-lookup"><span data-stu-id="567d4-183">Confirm that you see `adftutorialgateway is connected toohello cloud service` message.</span></span> <span data-ttu-id="567d4-184">Hello stavovém řádku zobrazuje dolní hello **připojené toohello Cloudová služba** spolu s **zelená značka zaškrtnutí**.</span><span class="sxs-lookup"><span data-stu-id="567d4-184">hello status bar hello bottom displays **Connected toohello cloud service** along with a **green check mark**.</span></span>

    <span data-ttu-id="567d4-185">Na hello **Domů** kartě, můžete provést také hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="567d4-185">On hello **Home** tab, you can also do hello following operations:</span></span>

   * <span data-ttu-id="567d4-186">**Zaregistrovat** bránu pomocí klíče z portálu Azure pomocí tlačítko Registrovat hello hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-186">**Register** a gateway with a key from hello Azure portal by using hello Register button.</span></span>
   * <span data-ttu-id="567d4-187">**Zastavit** hello Data Management Gateway hostitele Service spuštěná na počítači brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-187">**Stop** hello Data Management Gateway Host Service running on your gateway machine.</span></span>
   * <span data-ttu-id="567d4-188">**Naplánovat aktualizace** toobe nainstalovaný v určitém čase hello dne.</span><span class="sxs-lookup"><span data-stu-id="567d4-188">**Schedule updates** toobe installed at a specific time of hello day.</span></span>
   * <span data-ttu-id="567d4-189">Zobrazení při hello brány **poslední aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="567d4-189">View when hello gateway was **last updated**.</span></span>
   * <span data-ttu-id="567d4-190">Zadejte čas, kdy se dá nainstalovat bránu toohello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="567d4-190">Specify time at which an update toohello gateway can be installed.</span></span>
8. <span data-ttu-id="567d4-191">Přepínač toohello **nastavení** kartě hello certifikát uvedený v hello **certifikát** část se přihlašovací údaje použité tooencrypt/dešifrování pro hello místní datové úložiště, které zadáte na portál hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-191">Switch toohello **Settings** tab. hello certificate specified in hello **Certificate** section is used tooencrypt/decrypt credentials for hello on-premises data store that you specify on hello portal.</span></span> <span data-ttu-id="567d4-192">(volitelné) Klikněte na tlačítko **změnu** toouse svůj vlastní certifikát místo.</span><span class="sxs-lookup"><span data-stu-id="567d4-192">(optional) Click **Change** toouse your own certificate instead.</span></span> <span data-ttu-id="567d4-193">Ve výchozím nastavení používá brána hello hello certifikát, který je automaticky vygenerována třídou hello služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="567d4-193">By default, hello gateway uses hello certificate that is auto-generated by hello Data Factory service.</span></span>

    ![Konfigurace certifikátu brány](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    <span data-ttu-id="567d4-195">Můžete také provést následující akce v hello hello **nastavení** karty:</span><span class="sxs-lookup"><span data-stu-id="567d4-195">You can also do hello following actions on hello **Settings** tab:</span></span>

   * <span data-ttu-id="567d4-196">Zobrazení nebo export hello certifikátu používanému službou hello brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-196">View or export hello certificate being used by hello gateway.</span></span>
   * <span data-ttu-id="567d4-197">Změnit koncový bod HTTPS hello používá hello brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-197">Change hello HTTPS endpoint used by hello gateway.</span></span>    
   * <span data-ttu-id="567d4-198">Nastavte toobe proxy serveru HTTP používaný hello brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-198">Set an HTTP proxy toobe used by hello gateway.</span></span>     
9. <span data-ttu-id="567d4-199">(volitelné) Přepínač toohello **diagnostiky** kartě, zkontrolujte hello **zapnout podrobné protokolování** možnost, pokud chcete tooenable podrobné protokolování, které můžete použít tootroubleshoot všechny problémy s bránou hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-199">(optional) Switch toohello **Diagnostics** tab, check hello **Enable verbose logging** option if you want tooenable verbose logging that you can use tootroubleshoot any issues with hello gateway.</span></span> <span data-ttu-id="567d4-200">Hello informace o protokolování naleznete v **Prohlížeč událostí** pod **protokoly aplikací a služeb** -> **Brána pro správu dat** uzlu.</span><span class="sxs-lookup"><span data-stu-id="567d4-200">hello logging information can be found in **Event Viewer** under **Applications and Services Logs** -> **Data Management Gateway** node.</span></span>

    ![Karta Diagnostika](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    <span data-ttu-id="567d4-202">Můžete také provést následující akce v hello hello **diagnostiky** karty:</span><span class="sxs-lookup"><span data-stu-id="567d4-202">You can also perform hello following actions in hello **Diagnostics** tab:</span></span>

   * <span data-ttu-id="567d4-203">Použití **Test připojení** části tooan místní zdroje dat pomocí hello brány.</span><span class="sxs-lookup"><span data-stu-id="567d4-203">Use **Test Connection** section tooan on-premises data source using hello gateway.</span></span>
   * <span data-ttu-id="567d4-204">Klikněte na tlačítko **zobrazit protokoly** toosee hello Brána pro správu dat protokolu v okně prohlížeče událostí.</span><span class="sxs-lookup"><span data-stu-id="567d4-204">Click **View Logs** toosee hello Data Management Gateway log in an Event Viewer window.</span></span>
   * <span data-ttu-id="567d4-205">Klikněte na tlačítko **odeslat protokoly** tooupload soubor zip s protokoly posledních sedmi dnech tooMicrosoft toofacilitate problémů v vaše.</span><span class="sxs-lookup"><span data-stu-id="567d4-205">Click **Send Logs** tooupload a zip file with logs of last seven days tooMicrosoft toofacilitate troubleshooting of your issues.</span></span>
10. <span data-ttu-id="567d4-206">Na hello **diagnostiky** na kartě hello **Test připojení** vyberte **SqlServer** hello hello datový typ úložiště, zadejte název hello hello databázového serveru, název Hello databázi, zadejte typ ověřování, zadejte uživatelské jméno a heslo a klikněte na tlačítko **Test** tootest jestli hello gateway bude moct připojit toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="567d4-206">On hello **Diagnostics** tab, in hello **Test Connection** section, select **SqlServer** for hello type of hello data store, enter hello name of hello database server, name of hello database, specify authentication type, enter user name, and password, and click **Test** tootest whether hello gateway can connect toohello database.</span></span>
11. <span data-ttu-id="567d4-207">Přepínač toohello webového prohlížeče a v hello **portál Azure**, klikněte na tlačítko **OK** na hello **konfigurace** stránky a pak na hello **novou bránu dat** stránka.</span><span class="sxs-lookup"><span data-stu-id="567d4-207">Switch toohello web browser, and in hello **Azure portal**, click **OK** on hello **Configure** page and then on hello **New data gateway** page.</span></span>
12. <span data-ttu-id="567d4-208">Měli byste vidět **adftutorialgateway** pod **brány Data Gateways** hello stromové zobrazení na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-208">You should see **adftutorialgateway** under **Data Gateways** in hello tree view on hello left.</span></span>  <span data-ttu-id="567d4-209">Pokud kliknete na tlačítko ji, měli byste vidět spojenou hello JSON.</span><span class="sxs-lookup"><span data-stu-id="567d4-209">If you click it, you should see hello associated JSON.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="567d4-210">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="567d4-210">Create linked services</span></span>
<span data-ttu-id="567d4-211">V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **SqlServerLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="567d4-211">In this step, you create two linked services: **AzureStorageLinkedService** and **SqlServerLinkedService**.</span></span> <span data-ttu-id="567d4-212">Hello **SqlServerLinkedService** odkazy místní databázi systému SQL Server a hello **AzureStorageLinkedService** propojená služba odkazuje služby data factory toohello úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="567d4-212">hello **SqlServerLinkedService** links an on-premises SQL Server database and hello **AzureStorageLinkedService** linked service links an Azure blob store toohello data factory.</span></span> <span data-ttu-id="567d4-213">Vytvoření kanálu dále v tomto návodu, který kopíruje data z úložiště objektů blob v Azure toohello databáze serveru SQL Server pro místní hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-213">You create a pipeline later in this walkthrough that copies data from hello on-premises SQL Server database toohello Azure blob store.</span></span>

#### <a name="add-a-linked-service-tooan-on-premises-sql-server-database"></a><span data-ttu-id="567d4-214">Přidejte databázi SQL serveru propojená služba tooan na místě</span><span class="sxs-lookup"><span data-stu-id="567d4-214">Add a linked service tooan on-premises SQL Server database</span></span>
1. <span data-ttu-id="567d4-215">V hello **editoru služby Data Factory**, klikněte na tlačítko **nové datové úložiště** na panelu nástrojů hello a vyberte **systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="567d4-215">In hello **Data Factory Editor**, click **New data store** on hello toolbar and select **SQL Server**.</span></span>

   ![Nová SQL serveru propojená služba](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. <span data-ttu-id="567d4-217">V hello **JSON editor** na hello správné, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="567d4-217">In hello **JSON editor** on hello right, do hello following steps:</span></span>

   1. <span data-ttu-id="567d4-218">Pro hello **gatewayName**, zadejte **adftutorialgateway**.</span><span class="sxs-lookup"><span data-stu-id="567d4-218">For hello **gatewayName**, specify **adftutorialgateway**.</span></span>    
   2. <span data-ttu-id="567d4-219">V hello **connectionString**, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="567d4-219">In hello **connectionString**, do hello following steps:</span></span>    

      1. <span data-ttu-id="567d4-220">Pro **servername**, zadejte název hello hello serveru, který je hostitelem databáze systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-220">For **servername**, enter hello name of hello server that hosts hello SQL Server database.</span></span>
      2. <span data-ttu-id="567d4-221">Pro **databasename**, zadejte název hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="567d4-221">For **databasename**, enter hello name of hello database.</span></span>
      3. <span data-ttu-id="567d4-222">Klikněte na tlačítko **šifrovat** tlačítka na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-222">Click **Encrypt** button on hello toolbar.</span></span> <span data-ttu-id="567d4-223">Zobrazí aplikace hello správce přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="567d4-223">You see hello Credentials Manager application.</span></span>

         ![Pověření správce aplikace](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. <span data-ttu-id="567d4-225">V hello **nastavení pověření** dialogové okno, zadejte typ ověřování, uživatelské jméno a heslo a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="567d4-225">In hello **Setting Credentials** dialog box, specify authentication type, user name, and password, and click **OK**.</span></span> <span data-ttu-id="567d4-226">Pokud hello připojení úspěšné, hello šifrovat přihlašovací údaje uloží do hello JSON a zavře dialogové okno hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-226">If hello connection is successful, hello encrypted credentials are stored in hello JSON and hello dialog box closes.</span></span>
      5. <span data-ttu-id="567d4-227">Zavřete kartu hello prázdný prohlížeče, který spustit dialogové okno hello, pokud není uzavřený automaticky a získat zpátky toohello karta s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="567d4-227">Close hello empty browser tab that launched hello dialog box if it is not automatically closed and get back toohello tab with hello Azure portal.</span></span>

         <span data-ttu-id="567d4-228">Na počítači gateway hello tyto přihlašovací údaje jsou **šifrované** pomocí certifikátu, který hello Data Factory služby vlastní.</span><span class="sxs-lookup"><span data-stu-id="567d4-228">On hello gateway machine, these credentials are **encrypted** by using a certificate that hello Data Factory service owns.</span></span> <span data-ttu-id="567d4-229">Pokud chcete toouse hello certifikát, který je přidružen hello Brána pro správu dat na místo, najdete v části [nastavit pověření bezpečně](#set-credentials-and-security).</span><span class="sxs-lookup"><span data-stu-id="567d4-229">If you want toouse hello certificate that is associated with hello Data Management Gateway instead, see [Set credentials securely](#set-credentials-and-security).</span></span>    
   3. <span data-ttu-id="567d4-230">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello systému SQL Server propojené služby.</span><span class="sxs-lookup"><span data-stu-id="567d4-230">Click **Deploy** on hello command bar toodeploy hello SQL Server linked service.</span></span> <span data-ttu-id="567d4-231">Měli byste vidět hello propojené služby v zobrazení stromu hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-231">You should see hello linked service in hello tree view.</span></span>

      ![Služba SQL Server propojené v zobrazení stromu hello](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="567d4-233">Přidat propojené služby pro účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="567d4-233">Add a linked service for an Azure storage account</span></span>
1. <span data-ttu-id="567d4-234">V hello **editoru služby Data Factory**, klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a klikněte na tlačítko **úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="567d4-234">In hello **Data Factory Editor**, click **New data store** on hello command bar and click **Azure storage**.</span></span>
2. <span data-ttu-id="567d4-235">Zadejte hello název účtu úložiště Azure pro hello **název účtu**.</span><span class="sxs-lookup"><span data-stu-id="567d4-235">Enter hello name of your Azure storage account for hello **Account name**.</span></span>
3. <span data-ttu-id="567d4-236">Zadejte hello klíč pro účet úložiště Azure pro hello **klíč účtu**.</span><span class="sxs-lookup"><span data-stu-id="567d4-236">Enter hello key for your Azure storage account for hello **Account key**.</span></span>
4. <span data-ttu-id="567d4-237">Klikněte na tlačítko **nasadit** toodeploy hello **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="567d4-237">Click **Deploy** toodeploy hello **AzureStorageLinkedService**.</span></span>

## <a name="create-datasets"></a><span data-ttu-id="567d4-238">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="567d4-238">Create datasets</span></span>
<span data-ttu-id="567d4-239">V tomto kroku vytvoření vstupní a výstupní datové sady, které představují vstupní a výstupní data pro operace kopírování hello (databáze systému SQL Server pro místní = > úložiště objektů blob v Azure).</span><span class="sxs-lookup"><span data-stu-id="567d4-239">In this step, you create input and output datasets that represent input and output data for hello copy operation (On-premises SQL Server database => Azure blob storage).</span></span> <span data-ttu-id="567d4-240">Před vytvořením datové sady, hello následující kroky (podrobný postup odpovídá seznamu hello):</span><span class="sxs-lookup"><span data-stu-id="567d4-240">Before creating datasets, do hello following steps (detailed steps follows hello list):</span></span>

* <span data-ttu-id="567d4-241">Vytvoří tabulku s názvem **emp** v hello jste přidali jako objekt propojené služby pro vytváření dat toohello databáze systému SQL Server a vložte několik ukázkových položky do tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-241">Create a table named **emp** in hello SQL Server Database you added as a linked service toohello data factory and insert a couple of sample entries into hello table.</span></span>
* <span data-ttu-id="567d4-242">Vytvořte kontejner objektů blob s názvem **adftutorial** v hello Azure blob, které jste přidali jako objekt propojené služby pro vytváření dat toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="567d4-242">Create a blob container named **adftutorial** in hello Azure blob storage account you added as a linked service toohello data factory.</span></span>

### <a name="prepare-on-premises-sql-server-for-hello-tutorial"></a><span data-ttu-id="567d4-243">Příprava na místním serveru SQL pro kurz hello</span><span class="sxs-lookup"><span data-stu-id="567d4-243">Prepare On-premises SQL Server for hello tutorial</span></span>
1. <span data-ttu-id="567d4-244">V databázi hello jste určili pro hello místního SQL serveru propojená služba (**SqlServerLinkedService**), použijte následující SQL skriptu toocreate hello hello **emp** tabulky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-244">In hello database you specified for hello on-premises SQL Server linked service (**SqlServerLinkedService**), use hello following SQL script toocreate hello **emp** table in hello database.</span></span>

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. <span data-ttu-id="567d4-245">Některé ukázkové vložte do tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="567d4-245">Insert some sample into hello table:</span></span>

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a><span data-ttu-id="567d4-246">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="567d4-246">Create input dataset</span></span>

1. <span data-ttu-id="567d4-247">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další**, klikněte na tlačítko **nová datová sada** na panelu příkazů text hello a klikněte na tlačítko **tabulky serveru SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="567d4-247">In hello **Data Factory Editor**, click **... More**, click **New dataset** on hello command bar, and click **SQL Server table**.</span></span>
2. <span data-ttu-id="567d4-248">Nahraďte hello JSON v pravém podokně hello hello následující text:</span><span class="sxs-lookup"><span data-stu-id="567d4-248">Replace hello JSON in hello right pane with hello following text:</span></span>

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   <span data-ttu-id="567d4-249">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="567d4-249">Note hello following points:</span></span>

   * <span data-ttu-id="567d4-250">**typ** je nastaven příliš**SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="567d4-250">**type** is set too**SqlServerTable**.</span></span>
   * <span data-ttu-id="567d4-251">**Název tabulky** je nastaven příliš**emp**.</span><span class="sxs-lookup"><span data-stu-id="567d4-251">**tableName** is set too**emp**.</span></span>
   * <span data-ttu-id="567d4-252">**linkedServiceName** je nastaven příliš**SqlServerLinkedService** (jste vytvořili tuto propojenou službu dříve v tomto návodu.).</span><span class="sxs-lookup"><span data-stu-id="567d4-252">**linkedServiceName** is set too**SqlServerLinkedService** (you had created this linked service earlier in this walkthrough.).</span></span>
   * <span data-ttu-id="567d4-253">Pro vstupní datovou sadu, která není generované jiný kanál v Azure Data Factory, je potřeba nastavit **externí** příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="567d4-253">For an input dataset that is not generated by another pipeline in Azure Data Factory, you must set **external** too**true**.</span></span> <span data-ttu-id="567d4-254">Ji označuje, že jsou vstupní data hello vytvořené externí toohello služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="567d4-254">It denotes hello input data is produced external toohello Azure Data Factory service.</span></span> <span data-ttu-id="567d4-255">Volitelně můžete zadat všechny zásady externí data pomocí hello **externalData** element v hello **zásad** části.</span><span class="sxs-lookup"><span data-stu-id="567d4-255">You can optionally specify any external data policies using hello **externalData** element in hello **Policy** section.</span></span>    

   <span data-ttu-id="567d4-256">V tématu [přesun dat do nebo ze systému SQL Server](data-factory-sqlserver-connector.md) podrobnosti o vlastnostech formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="567d4-256">See [Move data to/from SQL Server](data-factory-sqlserver-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="567d4-257">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="567d4-257">Click **Deploy** on hello command bar toodeploy hello dataset.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="567d4-258">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="567d4-258">Create output dataset</span></span>

1. <span data-ttu-id="567d4-259">V hello **editoru služby Data Factory**, klikněte na tlačítko **nová datová sada** na panelu příkazů text hello a klikněte na tlačítko **úložiště objektů Azure Blob**.</span><span class="sxs-lookup"><span data-stu-id="567d4-259">In hello **Data Factory Editor**, click **New dataset** on hello command bar, and click **Azure Blob storage**.</span></span>
2. <span data-ttu-id="567d4-260">Nahraďte hello JSON v pravém podokně hello hello následující text:</span><span class="sxs-lookup"><span data-stu-id="567d4-260">Replace hello JSON in hello right pane with hello following text:</span></span>

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   <span data-ttu-id="567d4-261">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="567d4-261">Note hello following points:</span></span>

   * <span data-ttu-id="567d4-262">**typ** je nastaven příliš**AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="567d4-262">**type** is set too**AzureBlob**.</span></span>
   * <span data-ttu-id="567d4-263">**linkedServiceName** je nastaven příliš**AzureStorageLinkedService** (tuto propojenou službu měl vytvořili v kroku 2).</span><span class="sxs-lookup"><span data-stu-id="567d4-263">**linkedServiceName** is set too**AzureStorageLinkedService** (you had created this linked service in Step 2).</span></span>
   * <span data-ttu-id="567d4-264">**folderPath** je nastaven příliš**adftutorial/outfromonpremdf** kde outfromonpremdf je složka hello v kontejneru adftutorial hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-264">**folderPath** is set too**adftutorial/outfromonpremdf** where outfromonpremdf is hello folder in hello adftutorial container.</span></span> <span data-ttu-id="567d4-265">Vytvoření hello **adftutorial** kontejner, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="567d4-265">Create hello **adftutorial** container if it does not already exist.</span></span>
   * <span data-ttu-id="567d4-266">Hello **dostupnosti** je nastaven příliš**každou hodinu** (**frekvence** nastavit příliš**hodinu** a **interval** nastavit také **1**).</span><span class="sxs-lookup"><span data-stu-id="567d4-266">hello **availability** is set too**hourly** (**frequency** set too**hour** and **interval** set too**1**).</span></span>  <span data-ttu-id="567d4-267">Služba Data Factory Hello generuje řez výstupních dat každou hodinu v hello **emp** tabulky v hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="567d4-267">hello Data Factory service generates an output data slice every hour in hello **emp** table in hello Azure SQL Database.</span></span>

   <span data-ttu-id="567d4-268">Pokud nezadáte **fileName** pro **výstupní tabulku**, hello generované soubory v hello **folderPath** se pojmenují podle hello formátu: Data.<Guid>. TXT (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span><span class="sxs-lookup"><span data-stu-id="567d4-268">If you do not specify a **fileName** for an **output table**, hello generated files in hello **folderPath** are named in hello following format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span></span>

   <span data-ttu-id="567d4-269">tooset **folderPath** a **fileName** dynamicky podle hello **SliceStart** čas, použijte vlastnost partitionedBy hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-269">tooset **folderPath** and **fileName** dynamically based on hello **SliceStart** time, use hello partitionedBy property.</span></span> <span data-ttu-id="567d4-270">V následujícím příkladu hello folderPath používá rok, měsíc a den z hello SliceStart (čas zahájení zpracování řezu hello) a fileName používá hodinu z hello SliceStart.</span><span class="sxs-lookup"><span data-stu-id="567d4-270">In hello following example, folderPath uses Year, Month, and Day from hello SliceStart (start time of hello slice being processed) and fileName uses Hour from hello SliceStart.</span></span> <span data-ttu-id="567d4-271">Například, pokud je řez vytvářen v 2014-10-20T08:00:00, hello folderName je nastavená toowikidatagateway/wikisampledataout/2014/10/20 a hello fileName je nastavená too08.csv.</span><span class="sxs-lookup"><span data-stu-id="567d4-271">For example, if a slice is being produced for 2014-10-20T08:00:00, hello folderName is set toowikidatagateway/wikisampledataout/2014/10/20 and hello fileName is set too08.csv.</span></span>

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    <span data-ttu-id="567d4-272">V tématu [přesun dat do/z Azure Blob Storage](data-factory-azure-blob-connector.md) podrobnosti o vlastnostech formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="567d4-272">See [Move data to/from Azure Blob Storage](data-factory-azure-blob-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="567d4-273">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="567d4-273">Click **Deploy** on hello command bar toodeploy hello dataset.</span></span> <span data-ttu-id="567d4-274">Zkontrolujte, jestli obou hello datové sady v zobrazení stromu hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-274">Confirm that you see both hello datasets in hello tree view.</span></span>  

## <a name="create-pipeline"></a><span data-ttu-id="567d4-275">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="567d4-275">Create pipeline</span></span>
<span data-ttu-id="567d4-276">V tomto kroku vytvoříte **kanálu** s jedním **aktivity kopírování** používající **EmpOnPremSQLTable** jako vstup a **OutputBlobTable** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="567d4-276">In this step, you create a **pipeline** with one **Copy Activity** that uses **EmpOnPremSQLTable** as input and **OutputBlobTable** as output.</span></span>

1. <span data-ttu-id="567d4-277">V editoru služby Data Factory, klikněte na tlačítko **... Další** a poté na **Nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="567d4-277">In Data Factory Editor, click **... More**, and click **New pipeline**.</span></span>
2. <span data-ttu-id="567d4-278">Nahraďte hello JSON v pravém podokně hello hello následující text:</span><span class="sxs-lookup"><span data-stu-id="567d4-278">Replace hello JSON in hello right pane with hello following text:</span></span>    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL tooAzure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server tooblob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > <span data-ttu-id="567d4-279">Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den.</span><span class="sxs-lookup"><span data-stu-id="567d4-279">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span>
   >
   >

   <span data-ttu-id="567d4-280">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="567d4-280">Note hello following points:</span></span>

   * <span data-ttu-id="567d4-281">V části aktivity hello se pouze aktivity jejichž **typ** je nastaven příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="567d4-281">In hello activities section, there is only activity whose **type** is set too**Copy**.</span></span>
   * <span data-ttu-id="567d4-282">**Vstup** hello aktivity je nastavený příliš**EmpOnPremSQLTable** a **výstup** hello aktivity je nastavený příliš**OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="567d4-282">**Input** for hello activity is set too**EmpOnPremSQLTable** and **output** for hello activity is set too**OutputBlobTable**.</span></span>
   * <span data-ttu-id="567d4-283">V hello **rámci typeProperties** části **SqlSource** je zadán jako hello **typ zdroje** a ** BlobSink ** je zadán jako hello **jímky typ**.</span><span class="sxs-lookup"><span data-stu-id="567d4-283">In hello **typeProperties** section, **SqlSource** is specified as hello **source type** and **BlobSink **is specified as hello **sink type**.</span></span>
   * <span data-ttu-id="567d4-284">Dotaz SQL `select * from emp` je zadán pro hello **sqlReaderQuery** vlastnost **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="567d4-284">SQL query `select * from emp` is specified for hello **sqlReaderQuery** property of **SqlSource**.</span></span>

   <span data-ttu-id="567d4-285">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="567d4-285">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="567d4-286">Například: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="567d4-286">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="567d4-287">Hello **end** čas je nepovinný, ale používáme v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="567d4-287">hello **end** time is optional, but we use it in this tutorial.</span></span>

   <span data-ttu-id="567d4-288">Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**".</span><span class="sxs-lookup"><span data-stu-id="567d4-288">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="567d4-289">bez omezení, zadejte toorun hello kanálu **9/9/9999** hello hodnotu hello **end** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="567d4-289">toorun hello pipeline indefinitely, specify **9/9/9999** as hello value for hello **end** property.</span></span>

   <span data-ttu-id="567d4-290">Definování hello doby trvání v které hello se zpracovávají datové řezy podle hello **dostupnosti** vlastnosti, které byly definovány pro každé datové sady Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="567d4-290">You are defining hello time duration in which hello data slices are processed based on hello **Availability** properties that were defined for each Azure Data Factory dataset.</span></span>

   <span data-ttu-id="567d4-291">V příkladu hello je 24 datových řezů jak se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="567d4-291">In hello example, there are 24 data slices as each data slice is produced hourly.</span></span>        
3. <span data-ttu-id="567d4-292">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello datovou sadu (tabulka je obdélníková datová sada).</span><span class="sxs-lookup"><span data-stu-id="567d4-292">Click **Deploy** on hello command bar toodeploy hello dataset (table is a rectangular dataset).</span></span> <span data-ttu-id="567d4-293">Potvrďte, že hello kanálu se zobrazí v zobrazení stromu hello pod **kanály** uzlu.</span><span class="sxs-lookup"><span data-stu-id="567d4-293">Confirm that hello pipeline shows up in hello tree view under **Pipelines** node.</span></span>  
4. <span data-ttu-id="567d4-294">Nyní, klikněte na tlačítko **X** dvakrát tooclose hello stránky tooget back toohello **Data Factory** stránku hello **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="567d4-294">Now, click **X** twice tooclose hello page tooget back toohello **Data Factory** page for hello **ADFTutorialOnPremDF**.</span></span>

<span data-ttu-id="567d4-295">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="567d4-295">**Congratulations!**</span></span> <span data-ttu-id="567d4-296">Úspěšně jste vytvořili objekt pro vytváření dat Azure, propojené služby, počet datových sad a kanálu a naplánované hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="567d4-296">You have successfully created an Azure data factory, linked services, datasets, and a pipeline and scheduled hello pipeline.</span></span>

#### <a name="view-hello-data-factory-in-a-diagram-view"></a><span data-ttu-id="567d4-297">Zobrazení hello objekt pro vytváření dat v zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="567d4-297">View hello data factory in a Diagram View</span></span>
1. <span data-ttu-id="567d4-298">V hello **portál Azure**, klikněte na tlačítko **Diagram** na domovské stránce hello hello dlaždici **ADFTutorialOnPremDF** služby data factory.</span><span class="sxs-lookup"><span data-stu-id="567d4-298">In hello **Azure portal**, click **Diagram** tile on hello home page for hello **ADFTutorialOnPremDF** data factory.</span></span> <span data-ttu-id="567d4-299">:</span><span class="sxs-lookup"><span data-stu-id="567d4-299">:</span></span>

    ![Diagram odkaz](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. <span data-ttu-id="567d4-301">Měli byste vidět hello diagram podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="567d4-301">You should see hello diagram similar toohello following image:</span></span>

    ![Zobrazení diagramu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    <span data-ttu-id="567d4-303">Přiblížení, oddálení, zvětšení too100 %, toofit přiblížení, automaticky pozice kanálů a datové sady a zobrazení informací o rodokmenu (zvýrazní nadřazené a podřízené položky vybraných položek).</span><span class="sxs-lookup"><span data-stu-id="567d4-303">You can zoom in, zoom out, zoom too100%, zoom toofit, automatically position pipelines and datasets, and show lineage information (highlights upstream and downstream items of selected items).</span></span>  <span data-ttu-id="567d4-304">Poklepáním na vlastnosti toosee objektu (vstupní a výstupní datové sady nebo kanál) pro ni.</span><span class="sxs-lookup"><span data-stu-id="567d4-304">You can double-click an object (input/output dataset or pipeline) toosee properties for it.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="567d4-305">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="567d4-305">Monitor pipeline</span></span>
<span data-ttu-id="567d4-306">V tomto kroku použijete hello Azure portálu toomonitor, co se děje v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="567d4-306">In this step, you use hello Azure portal toomonitor what’s going on in an Azure data factory.</span></span> <span data-ttu-id="567d4-307">Můžete také použít PowerShell rutiny toomonitor datových sad a kanálů.</span><span class="sxs-lookup"><span data-stu-id="567d4-307">You can also use PowerShell cmdlets toomonitor datasets and pipelines.</span></span> <span data-ttu-id="567d4-308">Podrobnosti o monitorování najdete v tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="567d4-308">For details about monitoring, see [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md).</span></span>

1. <span data-ttu-id="567d4-309">V diagramu hello, klikněte dvakrát na **EmpOnPremSQLTable**.</span><span class="sxs-lookup"><span data-stu-id="567d4-309">In hello diagram, double-click **EmpOnPremSQLTable**.</span></span>  

    ![EmpOnPremSQLTable řezy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. <span data-ttu-id="567d4-311">Všimněte si, že všechny hello datové řezy až jsou v **připraven** stavu, protože doba trvání kanálu hello (čas spuštění doba tooend) je v minulosti hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-311">Notice that all hello data slices up are in **Ready** state because hello pipeline duration (start time tooend time) is in hello past.</span></span> <span data-ttu-id="567d4-312">Je také protože vložení hello dat v databázi systému SQL Server hello a existuje celou dobu hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-312">It is also because you have inserted hello data in hello SQL Server database and it is there all hello time.</span></span> <span data-ttu-id="567d4-313">Potvrďte, že žádné řezy nezobrazují v hello **problémové řezy** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-313">Confirm that no slices show up in hello **Problem slices** section at hello bottom.</span></span> <span data-ttu-id="567d4-314">Klikněte na všech řezech hello tooview **najdete v části více** dole hello hello seznam řezů.</span><span class="sxs-lookup"><span data-stu-id="567d4-314">tooview all hello slices, click **See More** at hello bottom of hello list of slices.</span></span>
3. <span data-ttu-id="567d4-315">Nyní v hello **datové sady** klikněte na tlačítko **OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="567d4-315">Now, In hello **Datasets** page, click **OutputBlobTable**.</span></span>

    ![OputputBlobTable řezy](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. <span data-ttu-id="567d4-317">Klikněte na libovolný datový řez ze seznamu hello a měli byste vidět hello **datový řez** stránky.</span><span class="sxs-lookup"><span data-stu-id="567d4-317">Click any data slice from hello list and you should see hello **Data Slice** page.</span></span> <span data-ttu-id="567d4-318">Uvidíte, že aktivita spuštěna hello řez.</span><span class="sxs-lookup"><span data-stu-id="567d4-318">You see activity runs for hello slice.</span></span> <span data-ttu-id="567d4-319">Zobrazí jenom jedna aktivita obvykle spustit.</span><span class="sxs-lookup"><span data-stu-id="567d4-319">You see only one activity run usually.</span></span>  

    ![Okno datový řez](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    <span data-ttu-id="567d4-321">Pokud není hello řez v hello **připraven** stavu, můžete zobrazit upstreamové datové řezy hello, které nejsou připravené a které blokují hello aktuálního řezu v hello **Upstreamové datové řezy, které nejsou připraveny** seznamu.</span><span class="sxs-lookup"><span data-stu-id="567d4-321">If hello slice is not in hello **Ready** state, you can see hello upstream slices that are not Ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span>
5. <span data-ttu-id="567d4-322">Klikněte na tlačítko hello **aktivity při spuštění** hello seznamu v dolním toosee hello **podrobnosti o spuštění aktivit**.</span><span class="sxs-lookup"><span data-stu-id="567d4-322">Click hello **activity run** from hello list at hello bottom toosee **activity run details**.</span></span>

   ![Stránka Podrobnosti o spuštění aktivity](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   <span data-ttu-id="567d4-324">Zobrazí se informace a, jako je propustnost, doba trvání a brány hello používá tootransfer hello data.</span><span class="sxs-lookup"><span data-stu-id="567d4-324">You would see information such as throughput, duration, and hello gateway used tootransfer hello data.</span></span>
6. <span data-ttu-id="567d4-325">Klikněte na tlačítko **X** tooclose všechny dokud hello stránky</span><span class="sxs-lookup"><span data-stu-id="567d4-325">Click **X** tooclose all hello pages until you</span></span>
7. <span data-ttu-id="567d4-326">získat zpět toohello domovskou stránku pro hello **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="567d4-326">get back toohello home page for hello **ADFTutorialOnPremDF**.</span></span>
8. <span data-ttu-id="567d4-327">(volitelné) Klikněte na tlačítko **kanály**, klikněte na tlačítko **ADFTutorialOnPremDF**a procházení vstupní tabulky (**Potřebováno**) nebo výstupní datové sady (**vyprodukováno**).</span><span class="sxs-lookup"><span data-stu-id="567d4-327">(optional) Click **Pipelines**, click **ADFTutorialOnPremDF**, and drill through input tables (**Consumed**) or output datasets (**Produced**).</span></span>
9. <span data-ttu-id="567d4-328">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) tooverify vytvořený objekt blob nebo soubor pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="567d4-328">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) tooverify that a blob/file is created for each hour.</span></span>

   ![Azure Storage Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a><span data-ttu-id="567d4-330">Další kroky</span><span class="sxs-lookup"><span data-stu-id="567d4-330">Next steps</span></span>
* <span data-ttu-id="567d4-331">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článek pro všechny podrobnosti o hello Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="567d4-331">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all hello details about hello Data Management Gateway.</span></span>
* <span data-ttu-id="567d4-332">V tématu [kopírování dat z Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn o jak toouse aktivity kopírování toomove data ze zdrojových dat úložiště tooa podřízený data.</span><span class="sxs-lookup"><span data-stu-id="567d4-332">See [Copy data from Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn about how toouse Copy Activity toomove data from a source data store tooa sink data store.</span></span>
