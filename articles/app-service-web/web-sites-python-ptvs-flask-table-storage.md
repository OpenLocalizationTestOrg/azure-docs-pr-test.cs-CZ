---
title: "Flask a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio"
description: "Naučte se používat nástroje Python Tools pro sadu Visual Studio k vytvoření webové aplikace Flask, která ukládá data ve službě Azure Table Storage a nasadíte ho do Azure App Service Web Apps."
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
ms.openlocfilehash: 2e1bc8eebd0b67b965cc70ac4b5dfe03c4720ddf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="flask-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="42beb-103">Flask a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42beb-103">Flask and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="42beb-104">V tomto kurzu použijeme [Python Tools pro Visual Studio] vytvořit jednoduchou hlasovací webové aplikace pomocí jedné z ukázkových šablon PTVS.</span><span class="sxs-lookup"><span data-stu-id="42beb-104">In this tutorial, we'll use [Python Tools for Visual Studio] to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="42beb-105">V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span><span class="sxs-lookup"><span data-stu-id="42beb-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span></span>

<span data-ttu-id="42beb-106">Hlasovací webové aplikace definuje abstrakci pro své úložiště, takže můžete snadno přepínat mezi různými typy úložiště (v paměti, Azure Table Storage, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="42beb-106">The polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="42beb-107">Jsme dozvíte, jak vytvořit účet úložiště Azure, jak nakonfigurovat webovou aplikaci k používání Azure Table Storage a jak publikovat webovou aplikaci do [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="42beb-107">We'll learn how to create an Azure Storage account, how to configure the web app to use Azure Table Storage, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="42beb-108">Najdete v článku [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami MongoDB, Azure Table Storage, MySQL a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="42beb-108">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="42beb-109">Ačkoli se tento článek zaměřuje na službu App Service, postup při vývoji služeb [Azure Cloud Services] je obdobný.</span><span class="sxs-lookup"><span data-stu-id="42beb-109">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42beb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="42beb-110">Prerequisites</span></span>
* <span data-ttu-id="42beb-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="42beb-111">Visual Studio 2015</span></span>
* <span data-ttu-id="42beb-112">[Python Tools 2.2 pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="42beb-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="42beb-113">[Python Tools 2.2 pro Visual Studio – ukázky VSIX]</span><span class="sxs-lookup"><span data-stu-id="42beb-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="42beb-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="42beb-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="42beb-115">[Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]</span><span class="sxs-lookup"><span data-stu-id="42beb-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="42beb-116">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42beb-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="42beb-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="42beb-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-the-project"></a><span data-ttu-id="42beb-118">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="42beb-118">Create the Project</span></span>
<span data-ttu-id="42beb-119">V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="42beb-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="42beb-120">Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="42beb-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="42beb-121">Jsme budete pak spusťte aplikaci místně pomocí výchozí úložiště v paměti.</span><span class="sxs-lookup"><span data-stu-id="42beb-121">Then we'll run the application locally using the default in-memory repository.</span></span>

1. <span data-ttu-id="42beb-122">V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="42beb-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="42beb-123">Šablony projektů z [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou dostupné v části **Python**, **Ukázky**.</span><span class="sxs-lookup"><span data-stu-id="42beb-123">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="42beb-124">Vyberte **webový projekt Flask hlasování** a klikněte na tlačítko OK a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="42beb-124">Select **Polls Flask Web Project** and click OK to create the project.</span></span>
   
     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)
3. <span data-ttu-id="42beb-126">Zobrazí se výzva k instalaci externích balíčků.</span><span class="sxs-lookup"><span data-stu-id="42beb-126">You will be prompted to install external packages.</span></span> <span data-ttu-id="42beb-127">Vyberte možnost **Instalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="42beb-127">Select **Install into a virtual environment**.</span></span>
   
     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)
4. <span data-ttu-id="42beb-129">Jako základní překladač vyberte jazyk **Python 2.7** nebo **Python 3.4**.</span><span class="sxs-lookup"><span data-stu-id="42beb-129">Select **Python 2.7** or **Python 3.4** as the base interpreter.</span></span>
   
     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="42beb-131">Stisknutím klávesy `F5` se ujistěte, zda aplikace pracuje.</span><span class="sxs-lookup"><span data-stu-id="42beb-131">Confirm that the application works by pressing `F5`.</span></span> <span data-ttu-id="42beb-132">Ve výchozím nastavení používá aplikace jako úložiště v paměti, která nevyžaduje žádnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="42beb-132">By default, the application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="42beb-133">Při zastavení webového serveru, dojde ke ztrátě všech dat.</span><span class="sxs-lookup"><span data-stu-id="42beb-133">All data is lost when the web server is stopped.</span></span>
6. <span data-ttu-id="42beb-134">Klikněte na tlačítko **vytvořit ukázková hlasování**, klikněte na hlasování a Hlasujte.</span><span class="sxs-lookup"><span data-stu-id="42beb-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="42beb-136">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="42beb-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="42beb-137">Chcete-li použít operace úložiště, potřebujete účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="42beb-137">To use storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="42beb-138">Pomocí následujícího postupu můžete vytvořit účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="42beb-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="42beb-139">Přihlaste se [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="42beb-139">Log into the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="42beb-140">Klikněte na tlačítko **nový** ikonu na horní pravé portálu, pak klikněte na tlačítko **Data + úložiště** > **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="42beb-140">Click the **New** icon on the top left of the Portal, then click **Data + Storage** > **Storage Account**.</span></span> <span data-ttu-id="42beb-141">Klikněte na **vytvořit**, zadejte jedinečný název účtu úložiště a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.</span><span class="sxs-lookup"><span data-stu-id="42beb-141">Click on **Create**, then give the storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Rychlé vytvoření](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="42beb-143">Po vytvoření účtu úložiště **oznámení** tlačítko bude flash zelená **úspěch** a okno účtu úložiště je otevřená a zobrazit tak, že patří do nové skupiny prostředků, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="42beb-143">When the storage account has been created, the **Notifications** button will flash a green **SUCCESS** and the storage account's blade is open to show that it belongs to the new resource group you created.</span></span>
3. <span data-ttu-id="42beb-144">Klikněte **přístupové klíče** část v okně účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="42beb-144">Click the **Access Keys** part in the storage account's blade.</span></span> <span data-ttu-id="42beb-145">Poznamenejte si název účtu a key1.</span><span class="sxs-lookup"><span data-stu-id="42beb-145">Take note of the account name and the key1.</span></span>
   
      ![Klíče](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="42beb-147">Potřebujeme bude tyto informace ke konfiguraci projektu v další části.</span><span class="sxs-lookup"><span data-stu-id="42beb-147">We will need this information to configure your project in the next section.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="42beb-148">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="42beb-148">Configure the Project</span></span>
<span data-ttu-id="42beb-149">V této části nakonfigurujeme naší aplikaci, aby používala účet úložiště, který jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="42beb-149">In this section, we'll configure our application to use the storage account we just created.</span></span> <span data-ttu-id="42beb-150">Uvidíme získání nastavení připojení z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="42beb-150">We'll see how to obtain connection settings from the Azure Portal.</span></span> <span data-ttu-id="42beb-151">Potom jsme budete místní spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="42beb-151">Then we'll run the application locally.</span></span>

1. <span data-ttu-id="42beb-152">V sadě Visual Studio, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="42beb-152">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="42beb-153">Klikněte na **ladění** kartě.</span><span class="sxs-lookup"><span data-stu-id="42beb-153">Click on the **Debug** tab.</span></span>
   
     ![Nastavení pro ladění projektu](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="42beb-155">Nastavte hodnoty proměnných prostředí, které jsou požadované aplikací v **ladění serveru příkaz**, **prostředí**.</span><span class="sxs-lookup"><span data-stu-id="42beb-155">Set the values of environment variables required by the application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="42beb-156">Nastaví proměnné prostředí při jste **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="42beb-156">This will set the environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="42beb-157">Pokud chcete, aby proměnné, které chcete být nastavený, pokud jste **spustit bez ladění**, nastavte stejné hodnoty v části **spustit příkaz serveru** také.</span><span class="sxs-lookup"><span data-stu-id="42beb-157">If you want the variables to be set when you **Start Without Debugging**, set the same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="42beb-158">Alternativně můžete definovat proměnné prostředí pomocí ovládacího panelu Windows.</span><span class="sxs-lookup"><span data-stu-id="42beb-158">Alternatively, you can define environment variables using the Windows Control Panel.</span></span> <span data-ttu-id="42beb-159">Toto je lepší volbou, pokud se chcete vyhnout ukládání přihlašovacích údajů ve zdrojovém kódu / souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="42beb-159">This is a better option if you want to avoid storing credentials in source code / project file.</span></span> <span data-ttu-id="42beb-160">Všimněte si, že budete muset restartovat Visual Studio pro nové hodnoty prostředí být k dispozici pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42beb-160">Note that you will need to restart Visual Studio for the new environment values to be available to the application.</span></span>
3. <span data-ttu-id="42beb-161">Kód, který implementuje úložiště Azure Table Storage je v **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="42beb-161">The code that implements the Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="42beb-162">Najdete v článku [dokumentace] Další informace o tom, jak používat služby Table z Pythonu.</span><span class="sxs-lookup"><span data-stu-id="42beb-162">See the [documentation] for more information on how to use Table Service from Python.</span></span>
4. <span data-ttu-id="42beb-163">Spusťte aplikaci klávesou `F5`.</span><span class="sxs-lookup"><span data-stu-id="42beb-163">Run the application with `F5`.</span></span> <span data-ttu-id="42beb-164">Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a data odeslaná při hlasování budou serializována v Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="42beb-164">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="42beb-165">Virtuální prostředí Python 2.7 může způsobit přerušení k výjimce v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="42beb-165">The Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="42beb-166">Stiskněte klávesu `F5` Chcete-li pokračovat v načítání webového projektu.</span><span class="sxs-lookup"><span data-stu-id="42beb-166">Press `F5` to continue loading the web project.</span></span>
   > 
   > 
5. <span data-ttu-id="42beb-167">Vyhledejte **o** a ověřte, že je aplikace pomocí **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="42beb-167">Browse to the **About** page to verify that the application is using the **Azure Table Storage** repository.</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## <a name="explore-the-azure-table-storage"></a><span data-ttu-id="42beb-169">Prozkoumejte Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="42beb-169">Explore the Azure Table Storage</span></span>
<span data-ttu-id="42beb-170">Je snadné zobrazení a úprava tabulek úložiště pomocí Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="42beb-170">It's easy to view and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="42beb-171">V této části použijeme v Průzkumníku serveru zobrazit obsah tabulek aplikace hlasování.</span><span class="sxs-lookup"><span data-stu-id="42beb-171">In this section we'll use Server Explorer to view the contents of the polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="42beb-172">To vyžaduje Microsoft Azure nástroje pro instalaci, které jsou k dispozici jako součást [Azure SDK for .NET].</span><span class="sxs-lookup"><span data-stu-id="42beb-172">This requires Microsoft Azure Tools to be installed, which are available as part of the [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="42beb-173">Otevřete **cloudu Explorer**.</span><span class="sxs-lookup"><span data-stu-id="42beb-173">Open **Cloud Explorer**.</span></span> <span data-ttu-id="42beb-174">Rozbalte položku **účty úložiště**, váš účet úložiště, pak **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="42beb-174">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Průzkumník cloudu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="42beb-176">Dvakrát klikněte na **hlasování** nebo **volby** tabulky k zobrazení obsahu tabulky v okně dokumentu a také přidat, odebrat nebo upravit entity.</span><span class="sxs-lookup"><span data-stu-id="42beb-176">Double-click on the **polls** or **choices** table to view the contents of the table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tabulky výsledků dotazu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="42beb-178">Publikování webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="42beb-178">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="42beb-179">Sada Azure .NET SDK poskytuje snadný způsob, jak nasadit webovou aplikaci do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="42beb-179">The Azure .NET SDK provides an easy way to deploy your web app to Azure App Service.</span></span>

1. <span data-ttu-id="42beb-180">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="42beb-180">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>
   
     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="42beb-182">Klikněte na položku **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="42beb-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="42beb-183">Kliknutím na možnost **Nové** vytvořte novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42beb-183">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="42beb-184">Vyplňte následující pole a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="42beb-184">Fill in the following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="42beb-185">**Název webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="42beb-185">**Web App name**</span></span>
   * <span data-ttu-id="42beb-186">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="42beb-186">**App Service plan**</span></span>
   * <span data-ttu-id="42beb-187">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="42beb-187">**Resource group**</span></span>
   * <span data-ttu-id="42beb-188">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="42beb-188">**Region**</span></span>
   * <span data-ttu-id="42beb-189">Položku **Databázový server** ponechte nastavenou na možnost **Bez databáze**</span><span class="sxs-lookup"><span data-stu-id="42beb-189">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="42beb-190">Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="42beb-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="42beb-191">Automaticky se otevře webový prohlížeč s publikovanou webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="42beb-191">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="42beb-192">Pokud přejdete o stránce, uvidíte, že používá **v paměti** úložiště, není **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="42beb-192">If you browse to the about page, you'll see that it uses the **In-Memory** repository, not the **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="42beb-193">Je to způsobeno proměnné prostředí nejsou nastavte u instance webové aplikace v Azure App Service, takže používá výchozí hodnoty zadané v **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="42beb-193">That's because the environment variables are not set on the Web Apps instance in Azure App Service, so it uses the default values specified in **settings.py**.</span></span>

## <a name="configure-the-web-apps-instance"></a><span data-ttu-id="42beb-194">Konfigurace instance webové aplikace</span><span class="sxs-lookup"><span data-stu-id="42beb-194">Configure the Web Apps instance</span></span>
<span data-ttu-id="42beb-195">V této části nakonfigurujeme proměnných prostředí pro instanci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42beb-195">In this section, we'll configure environment variables for the Web Apps instance.</span></span>

1. <span data-ttu-id="42beb-196">V [portálu Azure](https://portal.azure.com), otevřete okno webové aplikace kliknutím **Procházet** > **App Services** > název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42beb-196">In [Azure Portal](https://portal.azure.com), open the web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="42beb-197">V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="42beb-197">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="42beb-198">Přejděte dolů k položce **nastavení aplikace** tématu a nastavte hodnoty pro **úložiště\_název**, **úložiště\_název** a **úložiště\_klíč** jak je popsáno v **konfigurace projektu** část výše.</span><span class="sxs-lookup"><span data-stu-id="42beb-198">Scroll down to the **App settings** section and set the values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in the **Configure the project** section above.</span></span>
   
     ![Nastavení aplikace](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="42beb-200">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="42beb-200">Click on **Save**.</span></span> <span data-ttu-id="42beb-201">Poté, co jste obdrží oznámení, že byly použity změny, klikněte na **Procházet** v okně hlavní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42beb-201">After you've received the notifications that the changes were applied, click on **Browse** from the Web app main blade.</span></span>
5. <span data-ttu-id="42beb-202">Měli byste vidět webové aplikace funguje podle očekávání, pomocí **Azure Table Storage** úložiště.</span><span class="sxs-lookup"><span data-stu-id="42beb-202">You should see the web app working as expected, using the **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="42beb-203">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="42beb-203">Congratulations!</span></span>
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="42beb-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42beb-205">Next steps</span></span>
<span data-ttu-id="42beb-206">Další informace o nástrojích Python Tools pro Visual Studio, Flask a Azure Table Storage na následujících odkazech.</span><span class="sxs-lookup"><span data-stu-id="42beb-206">Follow these links to learn more about Python Tools for Visual Studio, Flask and Azure Table Storage.</span></span>

* <span data-ttu-id="42beb-207">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="42beb-207">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="42beb-208">[Webové projekty]</span><span class="sxs-lookup"><span data-stu-id="42beb-208">[Web Projects]</span></span>
  * <span data-ttu-id="42beb-209">[Projekty cloudových služeb]</span><span class="sxs-lookup"><span data-stu-id="42beb-209">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="42beb-210">[Vzdálené ladění v Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="42beb-210">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="42beb-211">[Dokumentace flask]</span><span class="sxs-lookup"><span data-stu-id="42beb-211">[Flask Documentation]</span></span>
* <span data-ttu-id="42beb-212">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="42beb-212">[Azure Storage]</span></span>
* <span data-ttu-id="42beb-213">[Azure SDK pro Python]</span><span class="sxs-lookup"><span data-stu-id="42beb-213">[Azure SDK for Python]</span></span>
* <span data-ttu-id="42beb-214">[Jak používat služby úložiště Table z Pythonu]</span><span class="sxs-lookup"><span data-stu-id="42beb-214">[How to Use the Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="42beb-215">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="42beb-215">What's changed</span></span>
* <span data-ttu-id="42beb-216">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="42beb-216">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentace]:../cosmos-db/table-storage-how-to-use-python.md
[Jak používat služby úložiště Table z Pythonu]:../cosmos-db/table-storage-how-to-use-python.md

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
