---
title: "Sestavení aplikace ASP.NET v Azure SQL Database | Microsoft Docs"
description: "Další informace o získání aplikace ASP.NET, v Azure, funguje s připojením k databázi SQL."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: c22b8ef4866fe2f1ae32c7cb9158fc7866788b26
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a><span data-ttu-id="8f11b-103">Sestavení aplikace ASP.NET v Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f11b-103">Build an ASP.NET app in Azure with SQL Database</span></span>

<span data-ttu-id="8f11b-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="8f11b-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="8f11b-105">V tomto kurzu se dozvíte, jak nasadit datové webové aplikace ASP.NET v Azure a propojte jej s [Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8f11b-105">This tutorial shows you how to deploy a data-driven ASP.NET web app in Azure and connect it to [Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span></span> <span data-ttu-id="8f11b-106">Jakmile budete hotovi, máte aplikaci ASP.NET spuštěná [Azure App Service](../app-service/app-service-value-prop-what-is.md) a připojení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="8f11b-106">When you're finished, you have a ASP.NET app running in [Azure App Service](../app-service/app-service-value-prop-what-is.md) and connected to SQL Database.</span></span>

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="8f11b-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="8f11b-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8f11b-109">Vytvoření databáze SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-109">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="8f11b-110">Připojení k databázi SQL aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8f11b-110">Connect an ASP.NET app to SQL Database</span></span>
> * <span data-ttu-id="8f11b-111">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-111">Deploy the app to Azure</span></span>
> * <span data-ttu-id="8f11b-112">Aktualizovat datový model a aplikaci znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="8f11b-112">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="8f11b-113">Datový proud protokolů z Azure terminálu</span><span class="sxs-lookup"><span data-stu-id="8f11b-113">Stream logs from Azure to your terminal</span></span>
> * <span data-ttu-id="8f11b-114">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-114">Manage the app in the Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f11b-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8f11b-115">Prerequisites</span></span>

<span data-ttu-id="8f11b-116">K provedení kroků v tomto kurzu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="8f11b-116">To complete this tutorial:</span></span>

* <span data-ttu-id="8f11b-117">Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:</span><span class="sxs-lookup"><span data-stu-id="8f11b-117">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with the following workloads:</span></span>
  - <span data-ttu-id="8f11b-118">**Vývoj pro ASP.NET a web**</span><span class="sxs-lookup"><span data-stu-id="8f11b-118">**ASP.NET and web development**</span></span>
  - <span data-ttu-id="8f11b-119">**Azure – vývoj**</span><span class="sxs-lookup"><span data-stu-id="8f11b-119">**Azure development**</span></span>

  ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="8f11b-121">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="8f11b-121">Download the sample</span></span>

<span data-ttu-id="8f11b-122">[Stáhněte si ukázkový projekt](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="8f11b-122">[Download the sample project](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span></span>

<span data-ttu-id="8f11b-123">Extrahování (rozbalte) *dotnet-sqldb kurzu master.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="8f11b-123">Extract (unzip) the  *dotnet-sqldb-tutorial-master.zip* file.</span></span>

<span data-ttu-id="8f11b-124">Ukázkový projekt obsahuje základní [ASP.NET MVC](https://www.asp.net/mvc) CRUD (vytvoření čtení aktualizace odstranění) aplikace pomocí [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="8f11b-124">The sample project contains a basic [ASP.NET MVC](https://www.asp.net/mvc) CRUD (create-read-update-delete) app using [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="run-the-app"></a><span data-ttu-id="8f11b-125">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="8f11b-125">Run the app</span></span>

<span data-ttu-id="8f11b-126">Otevřete *dotnet-sqldb – kurz – hlavní/DotNetAppSqlDb.sln* souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f11b-126">Open the *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* file in Visual Studio.</span></span> 

<span data-ttu-id="8f11b-127">Typ `Ctrl+F5` a spusťte aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="8f11b-127">Type `Ctrl+F5` to run the app without debugging.</span></span> <span data-ttu-id="8f11b-128">Aplikace se zobrazí ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="8f11b-128">The app is displayed in your default browser.</span></span> <span data-ttu-id="8f11b-129">Vyberte **vytvořit nový** propojit a vytvořit pár *úkolů* položky.</span><span class="sxs-lookup"><span data-stu-id="8f11b-129">Select the **Create New** link and create a couple *to-do* items.</span></span> 

![Dialogové okno Nový projekt ASP.NET](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

<span data-ttu-id="8f11b-131">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="8f11b-131">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="8f11b-132">Aplikace používá pro připojení k databázi kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="8f11b-132">The app uses a database context to connect with the database.</span></span> <span data-ttu-id="8f11b-133">V této ukázce kontext databáze používá připojovací řetězec s názvem `MyDbConnection`.</span><span class="sxs-lookup"><span data-stu-id="8f11b-133">In this sample, the database context uses a connection string named `MyDbConnection`.</span></span> <span data-ttu-id="8f11b-134">Připojovací řetězec je nastavena v *Web.config* souborů a v odkazuje *Models/MyDatabaseContext.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="8f11b-134">The connection string is set in the *Web.config* file and referenced in the *Models/MyDatabaseContext.cs* file.</span></span> <span data-ttu-id="8f11b-135">Název připojovacího řetězce se později v tomto kurzu používá pro připojení k databázi SQL Azure webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-135">The connection string name is used later in the tutorial to connect the Azure web app to an Azure SQL Database.</span></span> 

## <a name="publish-to-azure-with-sql-database"></a><span data-ttu-id="8f11b-136">Publikování v Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f11b-136">Publish to Azure with SQL Database</span></span>

<span data-ttu-id="8f11b-137">V **Průzkumníku řešení**, klikněte pravým tlačítkem na vaše **DotNetAppSqlDb** projektu a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-137">In the **Solution Explorer**, right-click your **DotNetAppSqlDb** project and select **Publish**.</span></span>

![Publikování z Průzkumníka řešení](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

<span data-ttu-id="8f11b-139">Zkontrolujte, že je vybraná možnost **Microsoft Azure App Service** a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-139">Make sure that **Microsoft Azure App Service** is selected and click **Publish**.</span></span>

![Publikování ze stránky přehledu projektu](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

<span data-ttu-id="8f11b-141">Publikování se otevře **vytvořit službu App Service** dialog, který vám pomůže vytvořit všechny prostředky Azure budete muset spustit webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-141">Publishing opens the **Create App Service** dialog, which helps you create all the Azure resources you need to run your ASP.NET web app in Azure.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="8f11b-142">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-142">Sign in to Azure</span></span>

<span data-ttu-id="8f11b-143">V dialogovém okně **Vytvoření služby App Service** klikněte na **Přidat účet** a přihlaste se ke svému předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-143">In the **Create App Service** dialog, click **Add an account**, and then sign in to your Azure subscription.</span></span> <span data-ttu-id="8f11b-144">Pokud jste již přihlášení k účtu Microsoft, ujistěte se, že odpovídá vašemu předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-144">If you're already signed into a Microsoft account, make sure that account holds your Azure subscription.</span></span> <span data-ttu-id="8f11b-145">Pokud jste přihlášeni k účtu Microsoft, který nemá přiřazené předplatné Azure, kliknutím na něj přidejte správný účet.</span><span class="sxs-lookup"><span data-stu-id="8f11b-145">If the signed-in Microsoft account doesn't have your Azure subscription, click it to add the correct account.</span></span>
   
![Přihlášení k Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

<span data-ttu-id="8f11b-147">Až se přihlásíte, můžete v tomto okně vytvořit všechny prostředky, které potřebujete pro vaši webovou aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-147">Once signed in, you're ready to create all the resources you need for your Azure web app in this dialog.</span></span>

### <a name="configure-the-web-app-name"></a><span data-ttu-id="8f11b-148">Nakonfigurujte název webové aplikace</span><span class="sxs-lookup"><span data-stu-id="8f11b-148">Configure the web app name</span></span>

<span data-ttu-id="8f11b-149">Můžete ponechat název vygenerovaný webové aplikace, nebo ho změnit na jiný jedinečný název (platnými znaky jsou `a-z`, `0-9`, a `-`).</span><span class="sxs-lookup"><span data-stu-id="8f11b-149">You can keep the generated web app name, or change it to another unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="8f11b-150">Název webové aplikace se používá jako součást výchozí adresa URL pro aplikaci (`<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="8f11b-150">The web app name is used as part of the default URL for your app (`<app_name>.azurewebsites.net`, where `<app_name>` is your web app name).</span></span> <span data-ttu-id="8f11b-151">Název webové aplikace musí být jedinečný v rámci všech aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-151">The web app name needs to be unique across all apps in Azure.</span></span> 

![Vytvoření dialogovém okně app service](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a><span data-ttu-id="8f11b-153">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8f11b-153">Create a resource group</span></span>

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="8f11b-154">Vedle pole **Skupina prostředků** klikněte na tlačítko **Nová**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-154">Next to **Resource Group**, click **New**.</span></span>

![Vedle skupinu prostředků klikněte na tlačítko Nový.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

<span data-ttu-id="8f11b-156">Název skupiny prostředků **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-156">Name the resource group **myResourceGroup**.</span></span>

> [!NOTE]
> <span data-ttu-id="8f11b-157">Neklikejte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-157">Do not click **Create**.</span></span> <span data-ttu-id="8f11b-158">Nejprve je třeba nastavit vytvářet databáze SQL v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="8f11b-158">You first need to set up a SQL Database in a later step.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="8f11b-159">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="8f11b-159">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="8f11b-160">Vedle pole **Plán služby App Service** klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-160">Next to **App Service Plan**, click **New**.</span></span> 

<span data-ttu-id="8f11b-161">V dialogovém okně **Konfigurace plánu služby App Service** nastavte nový plán takto:</span><span class="sxs-lookup"><span data-stu-id="8f11b-161">In the **Configure App Service Plan** dialog, configure the new App Service plan with the following settings:</span></span>

![Vytvoření plánu služby App Service](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| <span data-ttu-id="8f11b-163">Nastavení</span><span class="sxs-lookup"><span data-stu-id="8f11b-163">Setting</span></span>  | <span data-ttu-id="8f11b-164">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="8f11b-164">Suggested value</span></span> | <span data-ttu-id="8f11b-165">Další informace</span><span class="sxs-lookup"><span data-stu-id="8f11b-165">For more information</span></span> |
| ----------------- | ------------ | ----|
|<span data-ttu-id="8f11b-166">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="8f11b-166">**App Service Plan**</span></span>| <span data-ttu-id="8f11b-167">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="8f11b-167">myAppServicePlan</span></span> | [<span data-ttu-id="8f11b-168">Plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="8f11b-168">App Service plans</span></span>](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|<span data-ttu-id="8f11b-169">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="8f11b-169">**Location**</span></span>| <span data-ttu-id="8f11b-170">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="8f11b-170">West Europe</span></span> | [<span data-ttu-id="8f11b-171">Oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-171">Azure regions</span></span>](https://azure.microsoft.com/regions/) |
|<span data-ttu-id="8f11b-172">**Velikost**</span><span class="sxs-lookup"><span data-stu-id="8f11b-172">**Size**</span></span>| <span data-ttu-id="8f11b-173">Free</span><span class="sxs-lookup"><span data-stu-id="8f11b-173">Free</span></span> | [<span data-ttu-id="8f11b-174">Cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="8f11b-174">Pricing tiers</span></span>](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a><span data-ttu-id="8f11b-175">Vytvoření instance systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="8f11b-175">Create a SQL Server instance</span></span>

<span data-ttu-id="8f11b-176">Před vytvořením databázi, budete potřebovat [logického serveru Azure SQL Database](../sql-database/sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="8f11b-176">Before creating a database, you need an [Azure SQL Database logical server](../sql-database/sql-database-features.md).</span></span> <span data-ttu-id="8f11b-177">Logický server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="8f11b-177">A logical server contains a group of databases managed as a group.</span></span>

<span data-ttu-id="8f11b-178">Vyberte **Objevte další služby Azure**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-178">Select **Explore additional Azure services**.</span></span>

![Nastavení názvu webové aplikace](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

<span data-ttu-id="8f11b-180">V **služby** , klikněte na  **+**  ikonu vedle **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-180">In the **Services** tab, click the **+** icon next to **SQL Database**.</span></span> 

![Na kartě služby klikněte + Ikona vedle databáze SQL.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

<span data-ttu-id="8f11b-182">V **nakonfigurovat databázi SQL** dialogové okno, klikněte na tlačítko **nový** vedle **systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-182">In the **Configure SQL Database** dialog, click **New** next to **SQL Server**.</span></span> 

<span data-ttu-id="8f11b-183">Generuje se název jedinečný serveru.</span><span class="sxs-lookup"><span data-stu-id="8f11b-183">A unique server name is generated.</span></span> <span data-ttu-id="8f11b-184">Tento název se používá jako součást výchozí adresa URL logického serveru, `<server_name>.database.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="8f11b-184">This name is used as part of the default URL for your logical server, `<server_name>.database.windows.net`.</span></span> <span data-ttu-id="8f11b-185">Musí být ve všech instancích logický server v Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8f11b-185">It must be unique across all logical server instances in Azure.</span></span> <span data-ttu-id="8f11b-186">Můžete změnit název serveru, ale pro účely tohoto kurzu zachovat generované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f11b-186">You can change the server name, but for this tutorial, keep the generated value.</span></span>

<span data-ttu-id="8f11b-187">Přidat správce uživatelské jméno a heslo a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-187">Add an administrator username and password, and then select **OK**.</span></span> <span data-ttu-id="8f11b-188">Požadavky na složitost hesla, najdete v části [zásady hesel](/sql/relational-databases/security/password-policy).</span><span class="sxs-lookup"><span data-stu-id="8f11b-188">For password complexity requirements, see [Password Policy](/sql/relational-databases/security/password-policy).</span></span>

<span data-ttu-id="8f11b-189">Mějte na paměti Toto uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8f11b-189">Remember this username and password.</span></span> <span data-ttu-id="8f11b-190">Je potřebujete spravovat instanci logického serveru později.</span><span class="sxs-lookup"><span data-stu-id="8f11b-190">You need them to manage the logical server instance later.</span></span>

![Vytvoření instance systému SQL Server](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a><span data-ttu-id="8f11b-192">Vytvoření databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8f11b-192">Create a SQL Database</span></span>

<span data-ttu-id="8f11b-193">V **nakonfigurovat databázi SQL** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="8f11b-193">In the **Configure SQL Database** dialog:</span></span> 

* <span data-ttu-id="8f11b-194">Ponechat výchozí generovaný **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-194">Keep the default generated **Database Name**.</span></span>
* <span data-ttu-id="8f11b-195">V **název připojovacího řetězce**, typ *MyDbConnection*.</span><span class="sxs-lookup"><span data-stu-id="8f11b-195">In **Connection String Name**, type *MyDbConnection*.</span></span> <span data-ttu-id="8f11b-196">Tento název musí odpovídat připojovací řetězec, který se odkazuje v *Models/MyDatabaseContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="8f11b-196">This name must match the connection string that is referenced in *Models/MyDatabaseContext.cs*.</span></span>
* <span data-ttu-id="8f11b-197">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-197">Select **OK**.</span></span>

![Konfigurace databáze SQL](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

<span data-ttu-id="8f11b-199">**Vytvořit službu App Service** dialog zobrazuje prostředky jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8f11b-199">The **Create App Service** dialog shows the resources you've created.</span></span> <span data-ttu-id="8f11b-200">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-200">Click **Create**.</span></span> 

![prostředky, které jste vytvořili](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

<span data-ttu-id="8f11b-202">Až průvodce dokončí vytváření prostředků Azure, publikuje aplikace ASP.NET do Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-202">Once the wizard finishes creating the Azure resources, it  publishes your ASP.NET app to Azure.</span></span> <span data-ttu-id="8f11b-203">Spuštění výchozího prohlížeče s adresou URL nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f11b-203">Your default browser is launched with the URL to the deployed app.</span></span> 

<span data-ttu-id="8f11b-204">Přidejte několik položek úkolů.</span><span class="sxs-lookup"><span data-stu-id="8f11b-204">Add a few to-do items.</span></span>

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="8f11b-206">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="8f11b-206">Congratulations!</span></span> <span data-ttu-id="8f11b-207">Vaše aplikace ASP.NET řízené daty je spuštěna za provozu v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="8f11b-207">Your data-driven ASP.NET application is running live in Azure App Service.</span></span>

## <a name="access-the-sql-database-locally"></a><span data-ttu-id="8f11b-208">Přístup k databázi SQL místně</span><span class="sxs-lookup"><span data-stu-id="8f11b-208">Access the SQL Database locally</span></span>

<span data-ttu-id="8f11b-209">Visual Studio umožňuje prozkoumat a spravovat snadno v nové databáze SQL **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-209">Visual Studio lets you explore and manage your new SQL Database easily in the **SQL Server Object Explorer**.</span></span>

### <a name="create-a-database-connection"></a><span data-ttu-id="8f11b-210">Vytvoření připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="8f11b-210">Create a database connection</span></span>

<span data-ttu-id="8f11b-211">Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-211">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="8f11b-212">V horní části **Průzkumník objektů systému SQL Server**, klikněte **přidat SQL Server** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f11b-212">At the top of **SQL Server Object Explorer**, click the **Add SQL Server** button.</span></span>

### <a name="configure-the-database-connection"></a><span data-ttu-id="8f11b-213">Konfigurace připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="8f11b-213">Configure the database connection</span></span>

<span data-ttu-id="8f11b-214">V **připojit** dialogové okno, rozbalte **Azure** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8f11b-214">In the **Connect** dialog, expand the **Azure** node.</span></span> <span data-ttu-id="8f11b-215">Všechny instance databáze SQL v Azure jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="8f11b-215">All your SQL Database instances in Azure are listed here.</span></span>

<span data-ttu-id="8f11b-216">Vyberte `DotNetAppSqlDb` databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="8f11b-216">Select the `DotNetAppSqlDb` SQL Database.</span></span> <span data-ttu-id="8f11b-217">V dolní části se automaticky vyplní připojení, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="8f11b-217">The connection you created earlier is automatically filled at the bottom.</span></span>

<span data-ttu-id="8f11b-218">Zadejte heslo správce databáze, který jste dříve vytvořili a klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-218">Type the database administrator password you created earlier and click **Connect**.</span></span>

![Konfigurace připojení k databázi ze sady Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a><span data-ttu-id="8f11b-220">Povolit připojení klienta z vašeho počítače</span><span class="sxs-lookup"><span data-stu-id="8f11b-220">Allow client connection from your computer</span></span>

<span data-ttu-id="8f11b-221">**Vytvořit nové pravidlo brány firewall** se otevře dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8f11b-221">The **Create a new firewall rule** dialog is opened.</span></span> <span data-ttu-id="8f11b-222">Ve výchozím nastavení umožňuje vaší instanci služby SQL Database pouze připojení ze služby Azure, jako je například Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f11b-222">By default, your SQL Database instance only allows connections from Azure services, such as your Azure web app.</span></span> <span data-ttu-id="8f11b-223">Pro připojení k databázi, vytvořte pravidlo brány firewall v instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="8f11b-223">To connect to your database, create a firewall rule in the SQL Database instance.</span></span> <span data-ttu-id="8f11b-224">Pravidlo brány firewall umožňuje veřejnou IP adresu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="8f11b-224">The firewall rule allows the public IP address of your local computer.</span></span>

<span data-ttu-id="8f11b-225">Dialogové okno je již vyplněn veřejná IP adresa vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="8f11b-225">The dialog is already filled with your computer's public IP address.</span></span>

<span data-ttu-id="8f11b-226">Ujistěte se, že **přidat Moje IP adresa klienta** je vybrána a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-226">Make sure that **Add my client IP** is selected and click **OK**.</span></span> 

![Nastavení brány firewall pro instanci databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

<span data-ttu-id="8f11b-228">Jakmile sady Visual Studio dokončí vytváření nastavení brány firewall pro instanci SQL databáze, připojení se zobrazí v **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-228">Once Visual Studio finishes creating the firewall setting for your SQL Database instance, your connection shows up in **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="8f11b-229">Zde můžete provádět nejběžnější databázových operací, jako je například spouštění dotazů, vytvořit zobrazení a uložených procedur a další.</span><span class="sxs-lookup"><span data-stu-id="8f11b-229">Here, you can perform the most common database operations, such as run queries, create views and stored procedures, and more.</span></span> 

<span data-ttu-id="8f11b-230">Klikněte pravým tlačítkem na `Todoes` tabulky a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-230">Right-click on the `Todoes` table and select **View Data**.</span></span> 

![Prozkoumejte objekty databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a><span data-ttu-id="8f11b-232">Aktualizovat aplikaci migrace Code First</span><span class="sxs-lookup"><span data-stu-id="8f11b-232">Update app with Code First Migrations</span></span>

<span data-ttu-id="8f11b-233">Známých nástrojů v sadě Visual Studio slouží k aktualizaci databáze a webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-233">You can use the familiar tools in Visual Studio to update your database and web app in Azure.</span></span> <span data-ttu-id="8f11b-234">V tomto kroku použijete migrace Code First v Entity Framework k provedení změny svého schématu databáze a publikujete ho v Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-234">In this step, you use Code First Migrations in Entity Framework to make a change to your database schema and publish it to Azure.</span></span>

<span data-ttu-id="8f11b-235">Další informace o použití migrace Entity Framework Code First najdete v tématu [Začínáme s Entity Framework 6 Code First pomocí MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="8f11b-235">For more information about using Entity Framework Code First Migrations, see [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="update-your-data-model"></a><span data-ttu-id="8f11b-236">Aktualizovat data modelu</span><span class="sxs-lookup"><span data-stu-id="8f11b-236">Update your data model</span></span>

<span data-ttu-id="8f11b-237">Otevřete _Models\Todo.cs_ v editoru kódu.</span><span class="sxs-lookup"><span data-stu-id="8f11b-237">Open _Models\Todo.cs_ in the code editor.</span></span> <span data-ttu-id="8f11b-238">Přidejte následující vlastnosti, která má `ToDo` třídy:</span><span class="sxs-lookup"><span data-stu-id="8f11b-238">Add the following property to the `ToDo` class:</span></span>

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a><span data-ttu-id="8f11b-239">Spustit migrace Code First místně</span><span class="sxs-lookup"><span data-stu-id="8f11b-239">Run Code First Migrations locally</span></span>

<span data-ttu-id="8f11b-240">Spusťte několik příkazy provádění aktualizací místní databázi.</span><span class="sxs-lookup"><span data-stu-id="8f11b-240">Run a few commands to make updates to your local database.</span></span> 

<span data-ttu-id="8f11b-241">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-241">From the **Tools** menu, click **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="8f11b-242">V okně konzoly Správce balíčků povolení migrace Code First:</span><span class="sxs-lookup"><span data-stu-id="8f11b-242">In the Package Manager Console window, enable Code First Migrations:</span></span>

```PowerShell
Enable-Migrations
```

<span data-ttu-id="8f11b-243">Přidejte migrace:</span><span class="sxs-lookup"><span data-stu-id="8f11b-243">Add a migration:</span></span>

```PowerShell
Add-Migration AddProperty
```

<span data-ttu-id="8f11b-244">Aktualizace místní databázi:</span><span class="sxs-lookup"><span data-stu-id="8f11b-244">Update the local database:</span></span>

```PowerShell
Update-Database
```

<span data-ttu-id="8f11b-245">Typ `Ctrl+F5` a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f11b-245">Type `Ctrl+F5` to run the app.</span></span> <span data-ttu-id="8f11b-246">Upravit podrobnosti, otestovat a vytvořit odkazy.</span><span class="sxs-lookup"><span data-stu-id="8f11b-246">Test the edit, details, and create links.</span></span>

<span data-ttu-id="8f11b-247">Pokud aplikace načte bez chyb, proběhla úspěšně migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="8f11b-247">If the application loads without errors, then Code First Migrations has succeeded.</span></span> <span data-ttu-id="8f11b-248">Ale stránku stále vypadá stejně protože aplikační logiku ještě nepoužívá tuto novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8f11b-248">However, your page still looks the same because your application logic is not using this new property yet.</span></span> 

### <a name="use-the-new-property"></a><span data-ttu-id="8f11b-249">Použít nové vlastnosti</span><span class="sxs-lookup"><span data-stu-id="8f11b-249">Use the new property</span></span>

<span data-ttu-id="8f11b-250">Některé změny v kódu k použití `Done` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8f11b-250">Make some changes in your code to use the `Done` property.</span></span> <span data-ttu-id="8f11b-251">Pro zjednodušení v tomto kurzu se pouze chystáte změnit `Index` a `Create` zobrazení naleznete ve vlastnosti v akci.</span><span class="sxs-lookup"><span data-stu-id="8f11b-251">For simplicity in this tutorial, you're only going to change the `Index` and `Create` views to see the property in action.</span></span>

<span data-ttu-id="8f11b-252">Otevřete _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="8f11b-252">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="8f11b-253">Najít `Create()` metoda a přidejte `Done` do seznamu vlastností v `Bind` atribut.</span><span class="sxs-lookup"><span data-stu-id="8f11b-253">Find the `Create()` method and add `Done` to the list of properties in the `Bind` attribute.</span></span> <span data-ttu-id="8f11b-254">Když jste hotovi, vaše `Create()` podpis metody vypadá jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="8f11b-254">When you're done, your `Create()` method signature looks like the following code:</span></span>

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

<span data-ttu-id="8f11b-255">Otevřete _Views\Todos\Create.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="8f11b-255">Open _Views\Todos\Create.cshtml_.</span></span>

<span data-ttu-id="8f11b-256">V kódu Razor, měli byste vidět `<div class="form-group">` element, který používá `model.Description`a pak další `<div class="form-group">` element, který používá `model.CreatedDate`.</span><span class="sxs-lookup"><span data-stu-id="8f11b-256">In the Razor code, you should see a `<div class="form-group">` element that uses `model.Description`, and then another `<div class="form-group">` element that uses `model.CreatedDate`.</span></span> <span data-ttu-id="8f11b-257">Hned za tyto dva prvky, přidejte další `<div class="form-group">` element, který používá `model.Done`:</span><span class="sxs-lookup"><span data-stu-id="8f11b-257">Immediately following these two elements, add another `<div class="form-group">` element that uses `model.Done`:</span></span>

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

<span data-ttu-id="8f11b-258">Otevřete _Views\Todos\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="8f11b-258">Open _Views\Todos\Index.cshtml_.</span></span>

<span data-ttu-id="8f11b-259">Vyhledejte prázdné `<th></th>` elementu.</span><span class="sxs-lookup"><span data-stu-id="8f11b-259">Search for the empty `<th></th>` element.</span></span> <span data-ttu-id="8f11b-260">Nad tento element přidejte následující kód Razor:</span><span class="sxs-lookup"><span data-stu-id="8f11b-260">Just above this element, add the following Razor code:</span></span>

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

<span data-ttu-id="8f11b-261">Najít `<td>` elementu, který obsahuje `Html.ActionLink()` pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="8f11b-261">Find the `<td>` element that contains the `Html.ActionLink()` helper methods.</span></span> <span data-ttu-id="8f11b-262">Nad tento element přidejte následující kód Razor:</span><span class="sxs-lookup"><span data-stu-id="8f11b-262">Just above this element, add the following Razor code:</span></span>

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

<span data-ttu-id="8f11b-263">To je všechno potřebujete zobrazit změny v `Index` a `Create` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8f11b-263">That's all you need to see the changes in the `Index` and `Create` views.</span></span> 

<span data-ttu-id="8f11b-264">Typ `Ctrl+F5` a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f11b-264">Type `Ctrl+F5` to run the app.</span></span>

<span data-ttu-id="8f11b-265">Teď můžete přidat položku seznamu úkolů a zkontrolujte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-265">You can now add a to-do item and check **Done**.</span></span> <span data-ttu-id="8f11b-266">Potom ho měli zobrazí v domovské stránce jako dokončené položka.</span><span class="sxs-lookup"><span data-stu-id="8f11b-266">Then it should show up in your homepage as a completed item.</span></span> <span data-ttu-id="8f11b-267">Nezapomeňte, že `Edit` zobrazení nezobrazí `Done` pole, protože nebyla změní `Edit` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8f11b-267">Remember that the `Edit` view doesn't show the `Done` field, because you didn't change the `Edit` view.</span></span>

### <a name="enable-code-first-migrations-in-azure"></a><span data-ttu-id="8f11b-268">Povolení migrace Code First v Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-268">Enable Code First Migrations in Azure</span></span>

<span data-ttu-id="8f11b-269">Teď, když kód změnit funguje, včetně databáze migrace jej publikujte do vaší webové aplikace Azure a příliš aktualizace databáze SQL se migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="8f11b-269">Now that your code change works, including database migration, you publish it to your Azure web app and update your SQL Database with Code First Migrations too.</span></span>

<span data-ttu-id="8f11b-270">Podobně jako před, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-270">Just like before, right-click your project and select **Publish**.</span></span>

<span data-ttu-id="8f11b-271">Klikněte na tlačítko **nastavení** otevřete Průvodce Publikovat.</span><span class="sxs-lookup"><span data-stu-id="8f11b-271">Click **Settings** to open the publish wizard.</span></span>

![Otevřete nastavení publikování](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

<span data-ttu-id="8f11b-273">V průvodci klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-273">In the wizard, click **Next**.</span></span>

<span data-ttu-id="8f11b-274">Ujistěte se, že je připojovací řetězec pro vaši databázi SQL vložené do **MyDatabaseContext (MyDbConnection)**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-274">Make sure that the connection string for your SQL Database is populated in **MyDatabaseContext (MyDbConnection)**.</span></span> <span data-ttu-id="8f11b-275">Je nutné vybrat **myToDoAppDb** databáze z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="8f11b-275">You may need to select the **myToDoAppDb** database from the dropdown.</span></span> 

<span data-ttu-id="8f11b-276">Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**, pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-276">Select **Execute Code First Migrations (runs on application start)**, then click **Save**.</span></span>

![Povolení migrace Code First ve webové aplikace Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a><span data-ttu-id="8f11b-278">Publikování změn</span><span class="sxs-lookup"><span data-stu-id="8f11b-278">Publish your changes</span></span>

<span data-ttu-id="8f11b-279">Teď, když jste povolili migrace Code First ve vaší webové aplikace Azure, publikování změn kódu.</span><span class="sxs-lookup"><span data-stu-id="8f11b-279">Now that you enabled Code First Migrations in your Azure web app, publish your code changes.</span></span>

<span data-ttu-id="8f11b-280">Na stránce publikování klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-280">In the publish page, click **Publish**.</span></span>

<span data-ttu-id="8f11b-281">Přidejte položkami seznamu úkolů znovu a vyberte **provádí**, a měli objeví v domovské stránce jako dokončené položka.</span><span class="sxs-lookup"><span data-stu-id="8f11b-281">Try adding to-do items again and select **Done**, and they should show up in your homepage as a completed item.</span></span>

![Webové aplikace Azure po migraci první kódu](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

<span data-ttu-id="8f11b-283">Všechny vaše existující položky seznamu úkolů se pořád zobrazí.</span><span class="sxs-lookup"><span data-stu-id="8f11b-283">All your existing to-do items are still displayed.</span></span> <span data-ttu-id="8f11b-284">Při opětovném aplikace ASP.NET, není stávající data v databázi SQL ztraceny.</span><span class="sxs-lookup"><span data-stu-id="8f11b-284">When you republish your ASP.NET application, existing data in your SQL Database is not lost.</span></span> <span data-ttu-id="8f11b-285">Migrace Code First také, pouze změní schéma dat a svoje existující data zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="8f11b-285">Also, Code First Migrations only changes the data schema and leaves your existing data intact.</span></span>


## <a name="stream-application-logs"></a><span data-ttu-id="8f11b-286">Protokoly aplikací datového proudu</span><span class="sxs-lookup"><span data-stu-id="8f11b-286">Stream application logs</span></span>

<span data-ttu-id="8f11b-287">Trasování zprávy můžete přímo z vaší webové aplikace Azure datového proudu k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f11b-287">You can stream tracing messages directly from your Azure web app to Visual Studio.</span></span>

<span data-ttu-id="8f11b-288">Otevřete _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="8f11b-288">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="8f11b-289">Každá akce začíná `Trace.WriteLine()` metoda.</span><span class="sxs-lookup"><span data-stu-id="8f11b-289">Each action starts with a `Trace.WriteLine()` method.</span></span> <span data-ttu-id="8f11b-290">Tento kód se přidá do ukazují, jak přidat trasování zprávy do vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-290">This code is added to show you how to add trace messages to your Azure web app.</span></span>

### <a name="open-server-explorer"></a><span data-ttu-id="8f11b-291">V Průzkumníku serveru otevřete</span><span class="sxs-lookup"><span data-stu-id="8f11b-291">Open Server Explorer</span></span>

<span data-ttu-id="8f11b-292">Z **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-292">From the **View** menu, select **Server Explorer**.</span></span> <span data-ttu-id="8f11b-293">Můžete nakonfigurovat protokolování pro Azure webové aplikace v **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-293">You can configure logging for your Azure web app in **Server Explorer**.</span></span> 

### <a name="enable-log-streaming"></a><span data-ttu-id="8f11b-294">Povolení protokolu streamování</span><span class="sxs-lookup"><span data-stu-id="8f11b-294">Enable log streaming</span></span>

<span data-ttu-id="8f11b-295">V **Průzkumníka serveru**, rozbalte položku **Azure** > **služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-295">In **Server Explorer**, expand **Azure** > **App Service**.</span></span>

<span data-ttu-id="8f11b-296">Rozbalte **myResourceGroup** skupinu prostředků, jste vytvořili při prvním vytvoření webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-296">Expand the **myResourceGroup** resource group, you created when you first created the Azure web app.</span></span>

<span data-ttu-id="8f11b-297">Klikněte pravým tlačítkem na vaší webové aplikace Azure a vyberte **zobrazit protokoly streamování**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-297">Right-click your Azure web app and select **View Streaming Logs**.</span></span>

![Povolení protokolu streamování](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

<span data-ttu-id="8f11b-299">Protokoly jsou nyní streamování do **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="8f11b-299">The logs are now streamed into the **Output** window.</span></span> 

![V okně výstupu datový proud protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

<span data-ttu-id="8f11b-301">Ale nezobrazí žádná trasování zpráv ještě.</span><span class="sxs-lookup"><span data-stu-id="8f11b-301">However, you don't see any of the trace messages yet.</span></span> <span data-ttu-id="8f11b-302">To je vzhledem k tomu, že pokud nejprve vybrat **zobrazit protokoly streamování**, vaší webové aplikace Azure nastaví na úrovni trasování `Error`, který jenom protokoluje události chyby (s `Trace.TraceError()` metoda).</span><span class="sxs-lookup"><span data-stu-id="8f11b-302">That's because when you first select **View Streaming Logs**, your Azure web app sets the trace level to `Error`, which only logs error events (with the `Trace.TraceError()` method).</span></span>

### <a name="change-trace-levels"></a><span data-ttu-id="8f11b-303">Změna úrovně trasování</span><span class="sxs-lookup"><span data-stu-id="8f11b-303">Change trace levels</span></span>

<span data-ttu-id="8f11b-304">Pokud chcete změnit úrovně trasování do výstupního další zprávy trasování, přejděte zpět na **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-304">To change the trace levels to output other trace messages, go back to **Server Explorer**.</span></span>

<span data-ttu-id="8f11b-305">Znovu klikněte pravým tlačítkem Azure webové aplikace a vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-305">Right-click your Azure web app again and select **Settings**.</span></span>

<span data-ttu-id="8f11b-306">V **protokolování aplikace (systém souborů)** rozevíracího seznamu vyberte **podrobné**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-306">In the **Application Logging (File System)** dropdown, select **Verbose**.</span></span> <span data-ttu-id="8f11b-307">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8f11b-307">Click **Save**.</span></span>

![Změňte úroveň trasování Verbose](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> <span data-ttu-id="8f11b-309">Můžete experimentovat s úrovněmi různých trasování chcete zobrazit, jaké typy zprávy se zobrazují pro každou úroveň.</span><span class="sxs-lookup"><span data-stu-id="8f11b-309">You can experiment with different trace levels to see what types of messages are displayed for each level.</span></span> <span data-ttu-id="8f11b-310">Například **informace** úroveň zahrnuje všechny protokoly, které jsou vytvořené `Trace.TraceInformation()`, `Trace.TraceWarning()`, a `Trace.TraceError()`, ale není protokoly vytvořené `Trace.WriteLine()`.</span><span class="sxs-lookup"><span data-stu-id="8f11b-310">For example, the **Information** level includes all logs created by `Trace.TraceInformation()`, `Trace.TraceWarning()`, and `Trace.TraceError()`, but not logs created by `Trace.WriteLine()`.</span></span>
>
>

<span data-ttu-id="8f11b-311">V prohlížeči zkuste klepnout kolem aplikaci seznamu úkolů v Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-311">In your browser, try clicking around the to-do list application in Azure.</span></span> <span data-ttu-id="8f11b-312">Trasovací zprávy jsou nyní streamování na **výstup** oken v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f11b-312">The trace messages are now streamed to the **Output** window in Visual Studio.</span></span>

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a><span data-ttu-id="8f11b-313">Zastavit streamování protokolu</span><span class="sxs-lookup"><span data-stu-id="8f11b-313">Stop log streaming</span></span>

<span data-ttu-id="8f11b-314">Zastavit službu protokolu streamování, klikněte na tlačítko **zastavit monitorování** v tlačítko **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="8f11b-314">To stop the log-streaming service, click the **Stop monitoring** button in the **Output** window.</span></span>

![Zastavit streamování protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="8f11b-316">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="8f11b-316">Manage your Azure web app</span></span>

<span data-ttu-id="8f11b-317">Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8f11b-317">Go to the [Azure portal](https://portal.azure.com) to see the web app you created.</span></span> 



<span data-ttu-id="8f11b-318">V levé nabídce klikněte na **App Service** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="8f11b-318">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

<span data-ttu-id="8f11b-320">Dostali jste na stránce vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f11b-320">You have landed in your web app's page.</span></span> 

<span data-ttu-id="8f11b-321">Ve výchozím nastavení, zobrazí na portálu **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="8f11b-321">By default, the portal shows the **Overview** page.</span></span> <span data-ttu-id="8f11b-322">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="8f11b-322">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="8f11b-323">Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="8f11b-323">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="8f11b-324">Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="8f11b-324">The tabs on the left side of the page show the different configuration pages you can open.</span></span> 

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="8f11b-326">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f11b-326">Next steps</span></span>

<span data-ttu-id="8f11b-327">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="8f11b-327">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8f11b-328">Vytvoření databáze SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-328">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="8f11b-329">Připojení k databázi SQL aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8f11b-329">Connect an ASP.NET app to SQL Database</span></span>
> * <span data-ttu-id="8f11b-330">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-330">Deploy the app to Azure</span></span>
> * <span data-ttu-id="8f11b-331">Aktualizovat datový model a aplikaci znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="8f11b-331">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="8f11b-332">Datový proud protokolů z Azure terminálu</span><span class="sxs-lookup"><span data-stu-id="8f11b-332">Stream logs from Azure to your terminal</span></span>
> * <span data-ttu-id="8f11b-333">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8f11b-333">Manage the app in the Azure portal</span></span>

<span data-ttu-id="8f11b-334">Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f11b-334">Advance to the next tutorial to learn how to map a custom DNS name to the web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8f11b-335">Mapování existujícího vlastního názvu DNS na Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="8f11b-335">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
