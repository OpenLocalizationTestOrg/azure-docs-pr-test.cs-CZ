---
title: "aaaBottle a Azure Table Storage na Azure pomocí nástroje Python Tools 2.2 pro Visual Studio"
description: "Zjistěte, jak toouse hello Python Tools pro Visual Studio toocreate Bottle aplikace, která ukládá data ve službě Azure Table Storage a nasadit hello webové aplikace tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: f075124b-db79-4e51-b394-09187dd6c634
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 25b9eb002b8748483d5b9458b7b5860a958b4bb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="b204a-103">Bottle a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b204a-103">Bottle and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="b204a-104">V tomto kurzu použijeme [Python Tools pro Visual Studio] dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="b204a-105">V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=GJXDGaEPy94).</span><span class="sxs-lookup"><span data-stu-id="b204a-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=GJXDGaEPy94).</span></span>

<span data-ttu-id="b204a-106">Hello hlasovací webové aplikace definuje abstrakci pro své úložiště, takže můžete snadno přepínat mezi různými typy úložiště (v paměti, Azure Table Storage, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="b204a-106">hello polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="b204a-107">Jsme dozvíte, jak toocreate Azure Storage účet, jak tooconfigure hello toouse webové aplikace Azure Table Storage a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="b204a-107">We'll learn how toocreate an Azure Storage account, how tooconfigure hello web app toouse Azure Table Storage, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="b204a-108">V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami MongoDB, Azure Table Storage, MySQL a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b204a-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="b204a-109">Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="b204a-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b204a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b204a-110">Prerequisites</span></span>
* <span data-ttu-id="b204a-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="b204a-111">Visual Studio 2015</span></span>
* <span data-ttu-id="b204a-112">[Python Tools 2.2 pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="b204a-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="b204a-113">[Python Tools 2.2 pro Visual Studio – ukázky VSIX]</span><span class="sxs-lookup"><span data-stu-id="b204a-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="b204a-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="b204a-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="b204a-115">[Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]</span><span class="sxs-lookup"><span data-stu-id="b204a-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="b204a-116">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="b204a-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b204a-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="b204a-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="b204a-118">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="b204a-118">Create hello Project</span></span>
<span data-ttu-id="b204a-119">V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="b204a-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="b204a-120">Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="b204a-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="b204a-121">Potom budete spustíme aplikace hello místně pomocí hello výchozí v paměti úložiště.</span><span class="sxs-lookup"><span data-stu-id="b204a-121">Then we'll run hello application locally using hello default in-memory repository.</span></span>

1. <span data-ttu-id="b204a-122">V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="b204a-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="b204a-123">šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**.</span><span class="sxs-lookup"><span data-stu-id="b204a-123">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="b204a-124">Vyberte **hlasování Bottle webového projektu** a klikněte na tlačítko OK toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="b204a-124">Select **Polls Bottle Web Project** and click OK toocreate hello project.</span></span>
   
     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. <span data-ttu-id="b204a-126">Bude výzvami tooinstall externí balíčky.</span><span class="sxs-lookup"><span data-stu-id="b204a-126">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="b204a-127">Vyberte možnost **Instalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="b204a-127">Select **Install into a virtual environment**.</span></span>
   
     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. <span data-ttu-id="b204a-129">Vyberte **Python 2.7** nebo **Python 3.4** jako základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-129">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="b204a-131">Potvrďte, že hello aplikace funguje tak, že stisknete `F5`.</span><span class="sxs-lookup"><span data-stu-id="b204a-131">Confirm that hello application works by pressing `F5`.</span></span> <span data-ttu-id="b204a-132">Ve výchozím nastavení používá aplikace hello jako úložiště v paměti, která nevyžaduje žádnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b204a-132">By default, hello application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="b204a-133">Všechna data bude ztracena, jakmile je zastavena hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b204a-133">All data is lost when hello web server is stopped.</span></span>
6. <span data-ttu-id="b204a-134">Klikněte na tlačítko **vytvořit ukázková hlasování**, klikněte na hlasování a Hlasujte.</span><span class="sxs-lookup"><span data-stu-id="b204a-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="b204a-136">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="b204a-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="b204a-137">operace úložiště toouse, potřebujete účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b204a-137">toouse storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="b204a-138">Pomocí následujícího postupu můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b204a-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="b204a-139">Přihlaste se k hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b204a-139">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b204a-140">Klikněte na tlačítko hello **nový** ikonu na hello horní pravé hello portál, pak klikněte na tlačítko **Data + úložiště** > **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="b204a-140">Click hello **New** icon on hello top left of hello Portal, then click **Data + Storage** > **Storage Account**.</span></span>  <span data-ttu-id="b204a-141">Klikněte na tlačítko hello **vytvořit** tlačítko, pak zadejte účet úložiště hello jedinečný název a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.</span><span class="sxs-lookup"><span data-stu-id="b204a-141">Click hello **Create** button, then give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Rychlé vytvoření](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="b204a-143">Po vytvoření účtu úložiště hello hello **oznámení** tlačítko bude flash zelená **úspěch** a se otevře okno účtu úložiště hello tooshow patří toohello nový prostředek skupiny vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b204a-143">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="b204a-144">Klikněte na tlačítko hello **přístupové klíče** část v okně účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-144">Click hello **Access keys** part in hello storage account's blade.</span></span> <span data-ttu-id="b204a-145">Poznamenejte si název účtu hello a key1.</span><span class="sxs-lookup"><span data-stu-id="b204a-145">Take note of hello account name and key1.</span></span>
   
      ![Klíče](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="b204a-147">Potřebujeme bude tato informace tooconfigure projektu v další části hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-147">We will need this information tooconfigure your project in hello next section.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="b204a-148">Konfigurace hello projektu</span><span class="sxs-lookup"><span data-stu-id="b204a-148">Configure hello Project</span></span>
<span data-ttu-id="b204a-149">V této části nakonfigurujeme naše aplikace toouse hello účet úložiště, který jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b204a-149">In this section, we'll configure our application toouse hello storage account we just created.</span></span> <span data-ttu-id="b204a-150">Potom jsme budete místní spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-150">Then we'll run hello application locally.</span></span>

1. <span data-ttu-id="b204a-151">V sadě Visual Studio, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="b204a-151">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="b204a-152">Klikněte na hello **ladění** kartě.</span><span class="sxs-lookup"><span data-stu-id="b204a-152">Click on hello **Debug** tab.</span></span>
   
     ![Nastavení pro ladění projektu](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="b204a-154">Nastavení hello hodnot proměnných prostředí vyžaduje aplikace hello v **ladění serveru příkaz**, **prostředí**.</span><span class="sxs-lookup"><span data-stu-id="b204a-154">Set hello values of environment variables required by hello application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="b204a-155">To bude nastavení proměnných prostředí hello když jste **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="b204a-155">This will set hello environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="b204a-156">Chcete-li hello proměnné toobe nastavený, pokud jste **spustit bez ladění**, sada hello stejné hodnoty v části **spustit příkaz serveru** také.</span><span class="sxs-lookup"><span data-stu-id="b204a-156">If you want hello variables toobe set when you **Start Without Debugging**, set hello same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="b204a-157">Alternativně můžete definovat proměnné prostředí pomocí ovládacích panelů Windows hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-157">Alternatively, you can define environment variables using hello Windows Control Panel.</span></span> <span data-ttu-id="b204a-158">Toto je lepší volbou, pokud chcete tooavoid ukládání přihlašovacích údajů ve zdrojovém kódu / souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="b204a-158">This is a better option if you want tooavoid storing credentials in source code / project file.</span></span> <span data-ttu-id="b204a-159">Všimněte si, že budete potřebovat toorestart Visual Studio pro hello nové prostředí hodnoty toobe k dispozici toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b204a-159">Note that you will need toorestart Visual Studio for hello new environment values toobe available toohello application.</span></span>
3. <span data-ttu-id="b204a-160">Hello kód, který implementuje úložiště Azure Table Storage hello je v **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="b204a-160">hello code that implements hello Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="b204a-161">V tématu hello [dokumentace] Další informace o tom, toouse služby Table z Pythonu.</span><span class="sxs-lookup"><span data-stu-id="b204a-161">See hello [documentation] for more information on how toouse Table Service from Python.</span></span>
4. <span data-ttu-id="b204a-162">Spuštění aplikace hello s `F5`.</span><span class="sxs-lookup"><span data-stu-id="b204a-162">Run hello application with `F5`.</span></span> <span data-ttu-id="b204a-163">Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="b204a-163">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b204a-164">Hello virtuální prostředí Python 2.7 může způsobit přerušení k výjimce v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b204a-164">hello Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="b204a-165">Stiskněte klávesu `F5` toocontinue načítání hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="b204a-165">Press `F5` toocontinue loading hello web project.</span></span>
   > 
   > 
5. <span data-ttu-id="b204a-166">Procházet toohello **o** tooverify stránky, které aplikace hello používá hello **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="b204a-166">Browse toohello **About** page tooverify that hello application is using hello **Azure Table Storage** repository.</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a><span data-ttu-id="b204a-168">Prozkoumejte hello Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="b204a-168">Explore hello Azure Table Storage</span></span>
<span data-ttu-id="b204a-169">Je snadno tooview a upravit tabulky úložiště pomocí Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b204a-169">It's easy tooview and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="b204a-170">V této části použijeme Průzkumníka serveru tooview hello obsahu tabulek aplikace hello hlasování.</span><span class="sxs-lookup"><span data-stu-id="b204a-170">In this section we'll use Server Explorer tooview hello contents of hello polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="b204a-171">To vyžaduje toobe nástroje Microsoft Azure nainstalovaná, které jsou k dispozici jako součást hello [Azure SDK for .NET].</span><span class="sxs-lookup"><span data-stu-id="b204a-171">This requires Microsoft Azure Tools toobe installed, which are available as part of hello [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="b204a-172">Otevřete **cloudu Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b204a-172">Open **Cloud Explorer**.</span></span> <span data-ttu-id="b204a-173">Rozbalte položku **účty úložiště**, váš účet úložiště, pak **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="b204a-173">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Průzkumník cloudu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="b204a-175">Dvakrát klikněte na hello **hlasování** nebo **volby** tabulky tooview hello obsah hello tabulky v okně dokumentu a také přidat, odebrat nebo upravit entity.</span><span class="sxs-lookup"><span data-stu-id="b204a-175">Double-click on hello **polls** or **choices** table tooview hello contents of hello table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tabulky výsledků dotazu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="b204a-177">Publikování hello webové aplikace tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="b204a-177">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="b204a-178">Hello .NET SDK služby Azure poskytuje snadno toodeploy tooAzure vaší webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b204a-178">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="b204a-179">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b204a-179">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="b204a-181">Klikněte na položku **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="b204a-181">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="b204a-182">Klikněte na **nový** toocreate novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b204a-182">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="b204a-183">Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b204a-183">Fill in hello following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="b204a-184">**Název webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="b204a-184">**Web App name**</span></span>
   * <span data-ttu-id="b204a-185">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="b204a-185">**App Service plan**</span></span>
   * <span data-ttu-id="b204a-186">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="b204a-186">**Resource group**</span></span>
   * <span data-ttu-id="b204a-187">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="b204a-187">**Region**</span></span>
   * <span data-ttu-id="b204a-188">Nechte **databázový server** nastavit příliš**žádná databáze.**</span><span class="sxs-lookup"><span data-stu-id="b204a-188">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="b204a-189">Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b204a-189">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="b204a-190">Webový prohlížeč se automaticky otevře toohello publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b204a-190">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="b204a-191">Pokud toohello o stránce, se zobrazí, že používá hello **v paměti** úložiště, není hello **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="b204a-191">If you browse toohello about page, you'll see that it uses hello **In-Memory** repository, not hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="b204a-192">Je to způsobeno proměnné prostředí hello nejsou nastaveny na hello instanci webové aplikace v Azure App Service, takže používá hello výchozí hodnoty zadané v **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="b204a-192">That's because hello environment variables are not set on hello Web Apps instance in Azure App Service, so it uses hello default values specified in **settings.py**.</span></span>

## <a name="configure-hello-web-apps-instance"></a><span data-ttu-id="b204a-193">Konfigurace instance webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b204a-193">Configure hello Web Apps instance</span></span>
<span data-ttu-id="b204a-194">V této části nakonfigurujeme proměnných prostředí pro instanci webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b204a-194">In this section, we'll configure environment variables for hello Web Apps instance.</span></span>

1. <span data-ttu-id="b204a-195">V [portálu Azure], otevřete okno hello webovou aplikaci kliknutím **Procházet** > **App Services** > název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b204a-195">In [Azure Portal], open hello web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="b204a-196">V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b204a-196">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="b204a-197">Projděte dolů toohello **nastavení aplikace** tématu a nastavte hodnoty hello **úložiště\_název**, **úložiště\_název** a  **ÚLOŽIŠTĚ\_klíč** jak je popsáno v hello **konfigurovat hello projektu** část výše.</span><span class="sxs-lookup"><span data-stu-id="b204a-197">Scroll down toohello **App settings** section and set hello values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in hello **Configure hello project** section above.</span></span>
   
     ![Nastavení aplikace](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="b204a-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b204a-199">Click on **Save**.</span></span> <span data-ttu-id="b204a-200">Poté, co jste přijali hello oznámení, že byly použity změny hello, klikněte na **Procházet** z hlavní okně hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b204a-200">After you've received hello notifications that hello changes were applied, click on **Browse** from hello Web app main blade.</span></span>
5. <span data-ttu-id="b204a-201">Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="b204a-201">You should see hello web app working as expected, using hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="b204a-202">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="b204a-202">Congratulations!</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="b204a-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b204a-204">Next steps</span></span>
<span data-ttu-id="b204a-205">Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, Bottle a úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="b204a-205">Follow these links toolearn more about Python Tools for Visual Studio, Bottle and Azure Table Storage.</span></span>

* <span data-ttu-id="b204a-206">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="b204a-206">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="b204a-207">[Webové projekty]</span><span class="sxs-lookup"><span data-stu-id="b204a-207">[Web Projects]</span></span>
  * <span data-ttu-id="b204a-208">[Projekty cloudových služeb]</span><span class="sxs-lookup"><span data-stu-id="b204a-208">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="b204a-209">[Vzdálené ladění v Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="b204a-209">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="b204a-210">[Bottle dokumentace]</span><span class="sxs-lookup"><span data-stu-id="b204a-210">[Bottle Documentation]</span></span>
* <span data-ttu-id="b204a-211">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="b204a-211">[Azure Storage]</span></span>
* <span data-ttu-id="b204a-212">[Azure SDK pro Python]</span><span class="sxs-lookup"><span data-stu-id="b204a-212">[Azure SDK for Python]</span></span>
* <span data-ttu-id="b204a-213">[Jak tooUse hello služby úložiště Table z Pythonu]</span><span class="sxs-lookup"><span data-stu-id="b204a-213">[How tooUse hello Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="b204a-214">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="b204a-214">What's changed</span></span>
* <span data-ttu-id="b204a-215">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b204a-215">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentace]:../cosmos-db/table-storage-how-to-use-python.md
[Jak tooUse hello služby úložiště Table z Pythonu]:../cosmos-db/table-storage-how-to-use-python.md


<!--External Link references-->
[portálu Azure]: https://portal.azure.com
[Azure SDK for .NET]: http://azure.microsoft.com/downloads/
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=624025
[Python Tools 2.2 pro Visual Studio – ukázky VSIX]: http://go.microsoft.com/fwlink/?LinkId=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Bottle dokumentace]: http://bottlepy.org/docs/dev/index.html
[Vzdálené ladění v Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webové projekty]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK pro Python]: https://github.com/Azure/azure-sdk-for-python
