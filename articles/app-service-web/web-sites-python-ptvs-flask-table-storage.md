---
title: "aaaFlask a Azure Table Storage na Azure pomocí nástroje Python Tools 2.2 pro Visual Studio"
description: "Zjistěte, jak toouse hello Python Tools pro Visual Studio toocreate Flask webovou aplikaci, která ukládá data ve službě Azure Table Storage a nasaďte ji tooAzure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: d8e70a29-aca1-4010-95f5-cfe769e3be06
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1a09d4cc78078a00492ba4fe7e2075df96fb0380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="flask-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="6c462-103">Flask a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c462-103">Flask and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="6c462-104">V tomto kurzu použijeme [Python Tools pro Visual Studio] dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="6c462-105">V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span><span class="sxs-lookup"><span data-stu-id="6c462-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span></span>

<span data-ttu-id="6c462-106">Hello hlasovací webové aplikace definuje abstrakci pro své úložiště, takže můžete snadno přepínat mezi různými typy úložiště (v paměti, Azure Table Storage, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="6c462-106">hello polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="6c462-107">Jsme dozvíte, jak toocreate Azure Storage účet, jak tooconfigure hello toouse webové aplikace Azure Table Storage a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="6c462-107">We'll learn how toocreate an Azure Storage account, how tooconfigure hello web app toouse Azure Table Storage, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="6c462-108">V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami MongoDB, Azure Table Storage, MySQL a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6c462-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="6c462-109">Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="6c462-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c462-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6c462-110">Prerequisites</span></span>
* <span data-ttu-id="6c462-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="6c462-111">Visual Studio 2015</span></span>
* <span data-ttu-id="6c462-112">[Python Tools 2.2 pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="6c462-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="6c462-113">[Python Tools 2.2 pro Visual Studio – ukázky VSIX]</span><span class="sxs-lookup"><span data-stu-id="6c462-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="6c462-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="6c462-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="6c462-115">[Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]</span><span class="sxs-lookup"><span data-stu-id="6c462-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="6c462-116">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="6c462-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6c462-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="6c462-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="6c462-118">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="6c462-118">Create hello Project</span></span>
<span data-ttu-id="6c462-119">V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="6c462-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="6c462-120">Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="6c462-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="6c462-121">Potom budete spustíme aplikace hello místně pomocí hello výchozí v paměti úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c462-121">Then we'll run hello application locally using hello default in-memory repository.</span></span>

1. <span data-ttu-id="6c462-122">V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="6c462-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="6c462-123">šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**.</span><span class="sxs-lookup"><span data-stu-id="6c462-123">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="6c462-124">Vyberte **webový projekt Flask hlasování** a klikněte na tlačítko OK toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="6c462-124">Select **Polls Flask Web Project** and click OK toocreate hello project.</span></span>
   
     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)
3. <span data-ttu-id="6c462-126">Bude výzvami tooinstall externí balíčky.</span><span class="sxs-lookup"><span data-stu-id="6c462-126">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="6c462-127">Vyberte možnost **Instalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="6c462-127">Select **Install into a virtual environment**.</span></span>
   
     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)
4. <span data-ttu-id="6c462-129">Vyberte **Python 2.7** nebo **Python 3.4** jako základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-129">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="6c462-131">Potvrďte, že hello aplikace funguje tak, že stisknete `F5`.</span><span class="sxs-lookup"><span data-stu-id="6c462-131">Confirm that hello application works by pressing `F5`.</span></span> <span data-ttu-id="6c462-132">Ve výchozím nastavení používá aplikace hello jako úložiště v paměti, která nevyžaduje žádnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6c462-132">By default, hello application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="6c462-133">Všechna data bude ztracena, jakmile je zastavena hello webový server.</span><span class="sxs-lookup"><span data-stu-id="6c462-133">All data is lost when hello web server is stopped.</span></span>
6. <span data-ttu-id="6c462-134">Klikněte na tlačítko **vytvořit ukázková hlasování**, klikněte na hlasování a Hlasujte.</span><span class="sxs-lookup"><span data-stu-id="6c462-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="6c462-136">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6c462-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="6c462-137">operace úložiště toouse, potřebujete účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6c462-137">toouse storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="6c462-138">Pomocí následujícího postupu můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c462-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="6c462-139">Přihlaste se k hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6c462-139">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6c462-140">Klikněte na tlačítko hello **nový** ikonu na hello horní pravé hello portál, pak klikněte na tlačítko **Data + úložiště** > **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6c462-140">Click hello **New** icon on hello top left of hello Portal, then click **Data + Storage** > **Storage Account**.</span></span> <span data-ttu-id="6c462-141">Klikněte na **vytvořit**, dejte účtu úložiště hello jedinečný název a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.</span><span class="sxs-lookup"><span data-stu-id="6c462-141">Click on **Create**, then give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Rychlé vytvoření](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="6c462-143">Po vytvoření účtu úložiště hello hello **oznámení** tlačítko bude flash zelená **úspěch** a se otevře okno účtu úložiště hello tooshow patří toohello nový prostředek skupiny vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6c462-143">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="6c462-144">Klikněte na tlačítko hello **přístupové klíče** část v okně účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-144">Click hello **Access Keys** part in hello storage account's blade.</span></span> <span data-ttu-id="6c462-145">Poznamenejte si název účtu hello a hello key1.</span><span class="sxs-lookup"><span data-stu-id="6c462-145">Take note of hello account name and hello key1.</span></span>
   
      ![Klíče](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="6c462-147">Potřebujeme bude tato informace tooconfigure projektu v další části hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-147">We will need this information tooconfigure your project in hello next section.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="6c462-148">Konfigurace hello projektu</span><span class="sxs-lookup"><span data-stu-id="6c462-148">Configure hello Project</span></span>
<span data-ttu-id="6c462-149">V této části nakonfigurujeme naše aplikace toouse hello účet úložiště, který jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6c462-149">In this section, we'll configure our application toouse hello storage account we just created.</span></span> <span data-ttu-id="6c462-150">Ukážeme, jak hello tooobtain nastavení připojení z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c462-150">We'll see how tooobtain connection settings from hello Azure Portal.</span></span> <span data-ttu-id="6c462-151">Potom jsme budete místní spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-151">Then we'll run hello application locally.</span></span>

1. <span data-ttu-id="6c462-152">V sadě Visual Studio, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6c462-152">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="6c462-153">Klikněte na hello **ladění** kartě.</span><span class="sxs-lookup"><span data-stu-id="6c462-153">Click on hello **Debug** tab.</span></span>
   
     ![Nastavení pro ladění projektu](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="6c462-155">Nastavení hello hodnot proměnných prostředí vyžaduje aplikace hello v **ladění serveru příkaz**, **prostředí**.</span><span class="sxs-lookup"><span data-stu-id="6c462-155">Set hello values of environment variables required by hello application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="6c462-156">To bude nastavení proměnných prostředí hello když jste **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="6c462-156">This will set hello environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="6c462-157">Chcete-li hello proměnné toobe nastavený, pokud jste **spustit bez ladění**, sada hello stejné hodnoty v části **spustit příkaz serveru** také.</span><span class="sxs-lookup"><span data-stu-id="6c462-157">If you want hello variables toobe set when you **Start Without Debugging**, set hello same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="6c462-158">Alternativně můžete definovat proměnné prostředí pomocí ovládacích panelů Windows hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-158">Alternatively, you can define environment variables using hello Windows Control Panel.</span></span> <span data-ttu-id="6c462-159">Toto je lepší volbou, pokud chcete tooavoid ukládání přihlašovacích údajů ve zdrojovém kódu / souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="6c462-159">This is a better option if you want tooavoid storing credentials in source code / project file.</span></span> <span data-ttu-id="6c462-160">Všimněte si, že budete potřebovat toorestart Visual Studio pro hello nové prostředí hodnoty toobe k dispozici toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c462-160">Note that you will need toorestart Visual Studio for hello new environment values toobe available toohello application.</span></span>
3. <span data-ttu-id="6c462-161">Hello kód, který implementuje úložiště Azure Table Storage hello je v **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="6c462-161">hello code that implements hello Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="6c462-162">V tématu hello [dokumentace] Další informace o tom, toouse služby Table z Pythonu.</span><span class="sxs-lookup"><span data-stu-id="6c462-162">See hello [documentation] for more information on how toouse Table Service from Python.</span></span>
4. <span data-ttu-id="6c462-163">Spuštění aplikace hello s `F5`.</span><span class="sxs-lookup"><span data-stu-id="6c462-163">Run hello application with `F5`.</span></span> <span data-ttu-id="6c462-164">Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="6c462-164">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6c462-165">Hello virtuální prostředí Python 2.7 může způsobit přerušení k výjimce v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c462-165">hello Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="6c462-166">Stiskněte klávesu `F5` toocontinue načítání hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="6c462-166">Press `F5` toocontinue loading hello web project.</span></span>
   > 
   > 
5. <span data-ttu-id="6c462-167">Procházet toohello **o** tooverify stránky, které aplikace hello používá hello **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c462-167">Browse toohello **About** page tooverify that hello application is using hello **Azure Table Storage** repository.</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a><span data-ttu-id="6c462-169">Prozkoumejte hello Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="6c462-169">Explore hello Azure Table Storage</span></span>
<span data-ttu-id="6c462-170">Je snadno tooview a upravit tabulky úložiště pomocí Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c462-170">It's easy tooview and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="6c462-171">V této části použijeme Průzkumníka serveru tooview hello obsahu tabulek aplikace hello hlasování.</span><span class="sxs-lookup"><span data-stu-id="6c462-171">In this section we'll use Server Explorer tooview hello contents of hello polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="6c462-172">To vyžaduje toobe nástroje Microsoft Azure nainstalovaná, které jsou k dispozici jako součást hello [Azure SDK for .NET].</span><span class="sxs-lookup"><span data-stu-id="6c462-172">This requires Microsoft Azure Tools toobe installed, which are available as part of hello [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="6c462-173">Otevřete **cloudu Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6c462-173">Open **Cloud Explorer**.</span></span> <span data-ttu-id="6c462-174">Rozbalte položku **účty úložiště**, váš účet úložiště, pak **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="6c462-174">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Průzkumník cloudu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="6c462-176">Dvakrát klikněte na hello **hlasování** nebo **volby** tabulky tooview hello obsah hello tabulky v okně dokumentu a také přidat, odebrat nebo upravit entity.</span><span class="sxs-lookup"><span data-stu-id="6c462-176">Double-click on hello **polls** or **choices** table tooview hello contents of hello table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tabulky výsledků dotazu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="6c462-178">Publikování hello webové aplikace tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="6c462-178">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="6c462-179">Hello .NET SDK služby Azure poskytuje snadno toodeploy tooAzure vaší webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="6c462-179">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="6c462-180">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6c462-180">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="6c462-182">Klikněte na položku **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="6c462-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="6c462-183">Klikněte na **nový** toocreate novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c462-183">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="6c462-184">Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6c462-184">Fill in hello following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="6c462-185">**Název webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="6c462-185">**Web App name**</span></span>
   * <span data-ttu-id="6c462-186">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="6c462-186">**App Service plan**</span></span>
   * <span data-ttu-id="6c462-187">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="6c462-187">**Resource group**</span></span>
   * <span data-ttu-id="6c462-188">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="6c462-188">**Region**</span></span>
   * <span data-ttu-id="6c462-189">Nechte **databázový server** nastavit příliš**žádná databáze.**</span><span class="sxs-lookup"><span data-stu-id="6c462-189">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="6c462-190">Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6c462-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="6c462-191">Webový prohlížeč se automaticky otevře toohello publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c462-191">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="6c462-192">Pokud toohello o stránce, se zobrazí, že používá hello **v paměti** úložiště, není hello **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c462-192">If you browse toohello about page, you'll see that it uses hello **In-Memory** repository, not hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="6c462-193">Je to způsobeno proměnné prostředí hello nejsou nastaveny na hello instanci webové aplikace v Azure App Service, takže používá hello výchozí hodnoty zadané v **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="6c462-193">That's because hello environment variables are not set on hello Web Apps instance in Azure App Service, so it uses hello default values specified in **settings.py**.</span></span>

## <a name="configure-hello-web-apps-instance"></a><span data-ttu-id="6c462-194">Konfigurace instance webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6c462-194">Configure hello Web Apps instance</span></span>
<span data-ttu-id="6c462-195">V této části nakonfigurujeme proměnných prostředí pro instanci webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6c462-195">In this section, we'll configure environment variables for hello Web Apps instance.</span></span>

1. <span data-ttu-id="6c462-196">V [portálu Azure](https://portal.azure.com), otevřete okno hello webovou aplikaci kliknutím **Procházet** > **App Services** > název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c462-196">In [Azure Portal](https://portal.azure.com), open hello web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="6c462-197">V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6c462-197">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="6c462-198">Projděte dolů toohello **nastavení aplikace** tématu a nastavte hodnoty hello **úložiště\_název**, **úložiště\_název** a  **ÚLOŽIŠTĚ\_klíč** jak je popsáno v hello **konfigurovat hello projektu** část výše.</span><span class="sxs-lookup"><span data-stu-id="6c462-198">Scroll down toohello **App settings** section and set hello values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in hello **Configure hello project** section above.</span></span>
   
     ![Nastavení aplikace](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="6c462-200">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c462-200">Click on **Save**.</span></span> <span data-ttu-id="6c462-201">Poté, co jste přijali hello oznámení, že byly použity změny hello, klikněte na **Procházet** z hlavní okně hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c462-201">After you've received hello notifications that hello changes were applied, click on **Browse** from hello Web app main blade.</span></span>
5. <span data-ttu-id="6c462-202">Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c462-202">You should see hello web app working as expected, using hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="6c462-203">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="6c462-203">Congratulations!</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="6c462-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c462-205">Next steps</span></span>
<span data-ttu-id="6c462-206">Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, Flask a úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="6c462-206">Follow these links toolearn more about Python Tools for Visual Studio, Flask and Azure Table Storage.</span></span>

* <span data-ttu-id="6c462-207">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="6c462-207">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="6c462-208">[Webové projekty]</span><span class="sxs-lookup"><span data-stu-id="6c462-208">[Web Projects]</span></span>
  * <span data-ttu-id="6c462-209">[Projekty cloudových služeb]</span><span class="sxs-lookup"><span data-stu-id="6c462-209">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="6c462-210">[Vzdálené ladění v Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="6c462-210">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="6c462-211">[Dokumentace flask]</span><span class="sxs-lookup"><span data-stu-id="6c462-211">[Flask Documentation]</span></span>
* <span data-ttu-id="6c462-212">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="6c462-212">[Azure Storage]</span></span>
* <span data-ttu-id="6c462-213">[Azure SDK pro Python]</span><span class="sxs-lookup"><span data-stu-id="6c462-213">[Azure SDK for Python]</span></span>
* <span data-ttu-id="6c462-214">[Jak tooUse hello služby úložiště Table z Pythonu]</span><span class="sxs-lookup"><span data-stu-id="6c462-214">[How tooUse hello Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="6c462-215">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="6c462-215">What's changed</span></span>
* <span data-ttu-id="6c462-216">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6c462-216">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentace]:../cosmos-db/table-storage-how-to-use-python.md
[Jak tooUse hello služby úložiště Table z Pythonu]:../cosmos-db/table-storage-how-to-use-python.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK for .NET]: http://azure.microsoft.com/downloads/
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 pro Visual Studio – ukázky VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?linkid=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentace flask]: http://flask.pocoo.org/
[Vzdálené ladění v Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webové projekty]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK pro Python]: https://github.com/Azure/azure-sdk-for-python
