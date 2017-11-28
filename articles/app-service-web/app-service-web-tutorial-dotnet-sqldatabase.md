---
title: aaaBuild aplikace ASP.NET v Azure SQL Database | Microsoft Docs
description: "Zjistěte, jak tooget ASP.NET aplikace v Azure, funguje s tooa připojení databáze SQL."
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
ms.openlocfilehash: d21c2bc404bfe038608c17e5a94d96847153002c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a><span data-ttu-id="ac391-103">Sestavení aplikace ASP.NET v Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="ac391-103">Build an ASP.NET app in Azure with SQL Database</span></span>

<span data-ttu-id="ac391-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="ac391-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="ac391-105">Tento kurz ukazuje, jak toodeploy ASP.NET se datové webové aplikace v Azure a připojte ho příliš[Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ac391-105">This tutorial shows you how toodeploy a data-driven ASP.NET web app in Azure and connect it too[Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span></span> <span data-ttu-id="ac391-106">Jakmile budete hotovi, máte aplikaci ASP.NET spuštěná [Azure App Service](../app-service/app-service-value-prop-what-is.md) a připojené tooSQL databáze.</span><span class="sxs-lookup"><span data-stu-id="ac391-106">When you're finished, you have a ASP.NET app running in [Azure App Service](../app-service/app-service-value-prop-what-is.md) and connected tooSQL Database.</span></span>

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="ac391-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="ac391-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ac391-109">Vytvoření databáze SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="ac391-109">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="ac391-110">Připojit tooSQL aplikace ASP.NET databáze</span><span class="sxs-lookup"><span data-stu-id="ac391-110">Connect an ASP.NET app tooSQL Database</span></span>
> * <span data-ttu-id="ac391-111">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="ac391-111">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="ac391-112">Aktualizovat hello datový model a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="ac391-112">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="ac391-113">Datový proud protokolů z Azure tooyour terminálu</span><span class="sxs-lookup"><span data-stu-id="ac391-113">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="ac391-114">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ac391-114">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac391-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ac391-115">Prerequisites</span></span>

<span data-ttu-id="ac391-116">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="ac391-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="ac391-117">Nainstalujte [Visual Studio 2017](https://www.visualstudio.com/downloads/) s hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="ac391-117">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello following workloads:</span></span>
  - <span data-ttu-id="ac391-118">**Vývoj pro ASP.NET a web**</span><span class="sxs-lookup"><span data-stu-id="ac391-118">**ASP.NET and web development**</span></span>
  - <span data-ttu-id="ac391-119">**Azure – vývoj**</span><span class="sxs-lookup"><span data-stu-id="ac391-119">**Azure development**</span></span>

  ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a><span data-ttu-id="ac391-121">Stažení ukázky hello</span><span class="sxs-lookup"><span data-stu-id="ac391-121">Download hello sample</span></span>

<span data-ttu-id="ac391-122">[Stáhněte si ukázkový projekt hello](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="ac391-122">[Download hello sample project](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span></span>

<span data-ttu-id="ac391-123">Extrahování (rozbalte) hello *dotnet-sqldb kurzu master.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="ac391-123">Extract (unzip) hello  *dotnet-sqldb-tutorial-master.zip* file.</span></span>

<span data-ttu-id="ac391-124">Hello ukázkový projekt obsahuje základní [ASP.NET MVC](https://www.asp.net/mvc) CRUD (vytvoření čtení aktualizace odstranění) aplikace pomocí [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="ac391-124">hello sample project contains a basic [ASP.NET MVC](https://www.asp.net/mvc) CRUD (create-read-update-delete) app using [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="run-hello-app"></a><span data-ttu-id="ac391-125">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="ac391-125">Run hello app</span></span>

<span data-ttu-id="ac391-126">Otevřete hello *dotnet-sqldb – kurz – hlavní/DotNetAppSqlDb.sln* souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac391-126">Open hello *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* file in Visual Studio.</span></span> 

<span data-ttu-id="ac391-127">Typ `Ctrl+F5` toorun hello aplikace bez ladění.</span><span class="sxs-lookup"><span data-stu-id="ac391-127">Type `Ctrl+F5` toorun hello app without debugging.</span></span> <span data-ttu-id="ac391-128">Hello aplikace se zobrazí ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ac391-128">hello app is displayed in your default browser.</span></span> <span data-ttu-id="ac391-129">Vyberte hello **vytvořit nový** propojit a vytvořit pár *úkolů* položky.</span><span class="sxs-lookup"><span data-stu-id="ac391-129">Select hello **Create New** link and create a couple *to-do* items.</span></span> 

![Dialogové okno Nový projekt ASP.NET](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

<span data-ttu-id="ac391-131">Test hello **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="ac391-131">Test hello **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ac391-132">aplikace Hello používá tooconnect kontextu databáze s databází hello.</span><span class="sxs-lookup"><span data-stu-id="ac391-132">hello app uses a database context tooconnect with hello database.</span></span> <span data-ttu-id="ac391-133">V této ukázce kontext databáze hello používá připojovací řetězec s názvem `MyDbConnection`.</span><span class="sxs-lookup"><span data-stu-id="ac391-133">In this sample, hello database context uses a connection string named `MyDbConnection`.</span></span> <span data-ttu-id="ac391-134">Hello připojovací řetězec je nastavena v hello *Web.config* souborové služby a odkazovaná v hello *Models/MyDatabaseContext.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="ac391-134">hello connection string is set in hello *Web.config* file and referenced in hello *Models/MyDatabaseContext.cs* file.</span></span> <span data-ttu-id="ac391-135">název připojovacího řetězce Hello se používá novější v hello kurz tooconnect hello službě Azure web app tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ac391-135">hello connection string name is used later in hello tutorial tooconnect hello Azure web app tooan Azure SQL Database.</span></span> 

## <a name="publish-tooazure-with-sql-database"></a><span data-ttu-id="ac391-136">Publikování tooAzure s databází SQL</span><span class="sxs-lookup"><span data-stu-id="ac391-136">Publish tooAzure with SQL Database</span></span>

<span data-ttu-id="ac391-137">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na vaše **DotNetAppSqlDb** projektu a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ac391-137">In hello **Solution Explorer**, right-click your **DotNetAppSqlDb** project and select **Publish**.</span></span>

![Publikování z Průzkumníka řešení](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

<span data-ttu-id="ac391-139">Zkontrolujte, že je vybraná možnost **Microsoft Azure App Service** a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ac391-139">Make sure that **Microsoft Azure App Service** is selected and click **Publish**.</span></span>

![Publikování ze stránky přehledu projektu](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

<span data-ttu-id="ac391-141">Publikování otevře hello **vytvořit službu App Service** dialogové okno, které vám pomůže vytvořit všechny hello prostředky Azure, budete potřebovat toorun webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-141">Publishing opens hello **Create App Service** dialog, which helps you create all hello Azure resources you need toorun your ASP.NET web app in Azure.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="ac391-142">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="ac391-142">Sign in tooAzure</span></span>

<span data-ttu-id="ac391-143">V hello **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **přidat účet**a potom se přihlaste tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-143">In hello **Create App Service** dialog, click **Add an account**, and then sign in tooyour Azure subscription.</span></span> <span data-ttu-id="ac391-144">Pokud jste již přihlášení k účtu Microsoft, ujistěte se, že odpovídá vašemu předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-144">If you're already signed into a Microsoft account, make sure that account holds your Azure subscription.</span></span> <span data-ttu-id="ac391-145">Pokud hello přihlášeného účtu Microsoft nemá vašeho předplatného Azure, klikněte na něj tooadd hello správný účet.</span><span class="sxs-lookup"><span data-stu-id="ac391-145">If hello signed-in Microsoft account doesn't have your Azure subscription, click it tooadd hello correct account.</span></span>
   
![Přihlaste se tooAzure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

<span data-ttu-id="ac391-147">Jakmile se přihlásíte, jste připravené toocreate všechny prostředky, které potřebujete k Azure webové aplikace v tomto dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="ac391-147">Once signed in, you're ready toocreate all hello resources you need for your Azure web app in this dialog.</span></span>

### <a name="configure-hello-web-app-name"></a><span data-ttu-id="ac391-148">Nakonfigurujte název webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="ac391-148">Configure hello web app name</span></span>

<span data-ttu-id="ac391-149">Můžete ponechat hello vygeneruje název webové aplikace, nebo ho změnit jedinečný název tooanother (platnými znaky jsou `a-z`, `0-9`, a `-`).</span><span class="sxs-lookup"><span data-stu-id="ac391-149">You can keep hello generated web app name, or change it tooanother unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="ac391-150">Název webové aplikace Hello se používá jako součást hello výchozí adresa URL pro aplikaci (`<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="ac391-150">hello web app name is used as part of hello default URL for your app (`<app_name>.azurewebsites.net`, where `<app_name>` is your web app name).</span></span> <span data-ttu-id="ac391-151">Název webové aplikace Hello musí toobe jedinečný mezi všechny aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-151">hello web app name needs toobe unique across all apps in Azure.</span></span> 

![Vytvoření dialogovém okně app service](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a><span data-ttu-id="ac391-153">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ac391-153">Create a resource group</span></span>

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="ac391-154">Další příliš**skupiny prostředků**, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="ac391-154">Next too**Resource Group**, click **New**.</span></span>

![Další tooResource skupiny, klepněte na tlačítko Nový.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

<span data-ttu-id="ac391-156">Název skupiny prostředků hello **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="ac391-156">Name hello resource group **myResourceGroup**.</span></span>

> [!NOTE]
> <span data-ttu-id="ac391-157">Neklikejte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ac391-157">Do not click **Create**.</span></span> <span data-ttu-id="ac391-158">Je nutné nejprve tooset vytvářet databáze SQL v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="ac391-158">You first need tooset up a SQL Database in a later step.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="ac391-159">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="ac391-159">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="ac391-160">Další příliš**plán služby App Service**, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="ac391-160">Next too**App Service Plan**, click **New**.</span></span> 

<span data-ttu-id="ac391-161">V hello **nakonfigurovat plán služby App Service** dialogu Nový plán aplikační služby hello nakonfigurovat hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ac391-161">In hello **Configure App Service Plan** dialog, configure hello new App Service plan with hello following settings:</span></span>

![Vytvoření plánu služby App Service](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| <span data-ttu-id="ac391-163">Nastavení</span><span class="sxs-lookup"><span data-stu-id="ac391-163">Setting</span></span>  | <span data-ttu-id="ac391-164">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="ac391-164">Suggested value</span></span> | <span data-ttu-id="ac391-165">Další informace</span><span class="sxs-lookup"><span data-stu-id="ac391-165">For more information</span></span> |
| ----------------- | ------------ | ----|
|<span data-ttu-id="ac391-166">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="ac391-166">**App Service Plan**</span></span>| <span data-ttu-id="ac391-167">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="ac391-167">myAppServicePlan</span></span> | [<span data-ttu-id="ac391-168">Plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="ac391-168">App Service plans</span></span>](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|<span data-ttu-id="ac391-169">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="ac391-169">**Location**</span></span>| <span data-ttu-id="ac391-170">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="ac391-170">West Europe</span></span> | [<span data-ttu-id="ac391-171">Oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="ac391-171">Azure regions</span></span>](https://azure.microsoft.com/regions/) |
|<span data-ttu-id="ac391-172">**Velikost**</span><span class="sxs-lookup"><span data-stu-id="ac391-172">**Size**</span></span>| <span data-ttu-id="ac391-173">Free</span><span class="sxs-lookup"><span data-stu-id="ac391-173">Free</span></span> | [<span data-ttu-id="ac391-174">Cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="ac391-174">Pricing tiers</span></span>](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a><span data-ttu-id="ac391-175">Vytvoření instance systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="ac391-175">Create a SQL Server instance</span></span>

<span data-ttu-id="ac391-176">Před vytvořením databázi, budete potřebovat [logického serveru Azure SQL Database](../sql-database/sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="ac391-176">Before creating a database, you need an [Azure SQL Database logical server](../sql-database/sql-database-features.md).</span></span> <span data-ttu-id="ac391-177">Logický server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="ac391-177">A logical server contains a group of databases managed as a group.</span></span>

<span data-ttu-id="ac391-178">Vyberte **Objevte další služby Azure**.</span><span class="sxs-lookup"><span data-stu-id="ac391-178">Select **Explore additional Azure services**.</span></span>

![Nastavení názvu webové aplikace](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

<span data-ttu-id="ac391-180">V hello **služby** , klikněte na hello  **+**  ikonu další příliš**SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="ac391-180">In hello **Services** tab, click hello **+** icon next too**SQL Database**.</span></span> 

![V kartě hello služeb, klikněte na ikonu + hello další tooSQL databáze.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

<span data-ttu-id="ac391-182">V hello **nakonfigurovat databázi SQL** dialogové okno, klikněte na tlačítko **nový** další příliš**systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ac391-182">In hello **Configure SQL Database** dialog, click **New** next too**SQL Server**.</span></span> 

<span data-ttu-id="ac391-183">Generuje se název jedinečný serveru.</span><span class="sxs-lookup"><span data-stu-id="ac391-183">A unique server name is generated.</span></span> <span data-ttu-id="ac391-184">Tento název se používá jako součást výchozí adresa URL hello logického serveru, `<server_name>.database.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="ac391-184">This name is used as part of hello default URL for your logical server, `<server_name>.database.windows.net`.</span></span> <span data-ttu-id="ac391-185">Musí být ve všech instancích logický server v Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ac391-185">It must be unique across all logical server instances in Azure.</span></span> <span data-ttu-id="ac391-186">Můžete změnit název serveru hello, ale pro účely tohoto kurzu, ponechte hodnotu hello vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="ac391-186">You can change hello server name, but for this tutorial, keep hello generated value.</span></span>

<span data-ttu-id="ac391-187">Přidat správce uživatelské jméno a heslo a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac391-187">Add an administrator username and password, and then select **OK**.</span></span> <span data-ttu-id="ac391-188">Požadavky na složitost hesla, najdete v části [zásady hesel](/sql/relational-databases/security/password-policy).</span><span class="sxs-lookup"><span data-stu-id="ac391-188">For password complexity requirements, see [Password Policy](/sql/relational-databases/security/password-policy).</span></span>

<span data-ttu-id="ac391-189">Mějte na paměti Toto uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="ac391-189">Remember this username and password.</span></span> <span data-ttu-id="ac391-190">Musíte je logický server hello toomanage instance později.</span><span class="sxs-lookup"><span data-stu-id="ac391-190">You need them toomanage hello logical server instance later.</span></span>

![Vytvoření instance systému SQL Server](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a><span data-ttu-id="ac391-192">Vytvoření databáze SQL</span><span class="sxs-lookup"><span data-stu-id="ac391-192">Create a SQL Database</span></span>

<span data-ttu-id="ac391-193">V hello **nakonfigurovat databázi SQL** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="ac391-193">In hello **Configure SQL Database** dialog:</span></span> 

* <span data-ttu-id="ac391-194">Ponechat výchozí hello generovaný **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="ac391-194">Keep hello default generated **Database Name**.</span></span>
* <span data-ttu-id="ac391-195">V **název připojovacího řetězce**, typ *MyDbConnection*.</span><span class="sxs-lookup"><span data-stu-id="ac391-195">In **Connection String Name**, type *MyDbConnection*.</span></span> <span data-ttu-id="ac391-196">Tento název musí odpovídat hello připojovací řetězec, který se odkazuje v *Models/MyDatabaseContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="ac391-196">This name must match hello connection string that is referenced in *Models/MyDatabaseContext.cs*.</span></span>
* <span data-ttu-id="ac391-197">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac391-197">Select **OK**.</span></span>

![Konfigurace databáze SQL](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

<span data-ttu-id="ac391-199">Hello **vytvořit službu App Service** dialog zobrazuje hello prostředky jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ac391-199">hello **Create App Service** dialog shows hello resources you've created.</span></span> <span data-ttu-id="ac391-200">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ac391-200">Click **Create**.</span></span> 

![Hello prostředky, které jste vytvořili](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

<span data-ttu-id="ac391-202">Po dokončení Průvodce hello hello vytváření prostředků Azure, publikuje vaše tooAzure aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ac391-202">Once hello wizard finishes creating hello Azure resources, it  publishes your ASP.NET app tooAzure.</span></span> <span data-ttu-id="ac391-203">Výchozí prohlížeč se spustí s hello URL toohello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-203">Your default browser is launched with hello URL toohello deployed app.</span></span> 

<span data-ttu-id="ac391-204">Přidejte několik položek úkolů.</span><span class="sxs-lookup"><span data-stu-id="ac391-204">Add a few to-do items.</span></span>

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="ac391-206">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="ac391-206">Congratulations!</span></span> <span data-ttu-id="ac391-207">Vaše aplikace ASP.NET řízené daty je spuštěna za provozu v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ac391-207">Your data-driven ASP.NET application is running live in Azure App Service.</span></span>

## <a name="access-hello-sql-database-locally"></a><span data-ttu-id="ac391-208">Přístup k místně hello databáze SQL</span><span class="sxs-lookup"><span data-stu-id="ac391-208">Access hello SQL Database locally</span></span>

<span data-ttu-id="ac391-209">Visual Studio umožňuje prozkoumat a spravovat nové databáze SQL snadno v hello **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ac391-209">Visual Studio lets you explore and manage your new SQL Database easily in hello **SQL Server Object Explorer**.</span></span>

### <a name="create-a-database-connection"></a><span data-ttu-id="ac391-210">Vytvoření připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="ac391-210">Create a database connection</span></span>

<span data-ttu-id="ac391-211">Z hello **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ac391-211">From hello **View** menu, select **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="ac391-212">Na začátku hello **Průzkumník objektů systému SQL Server**, klikněte na tlačítko hello **přidat SQL Server** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ac391-212">At hello top of **SQL Server Object Explorer**, click hello **Add SQL Server** button.</span></span>

### <a name="configure-hello-database-connection"></a><span data-ttu-id="ac391-213">Konfigurace připojení k databázi hello</span><span class="sxs-lookup"><span data-stu-id="ac391-213">Configure hello database connection</span></span>

<span data-ttu-id="ac391-214">V hello **připojit** dialogové okno, rozbalte položku hello **Azure** uzlu.</span><span class="sxs-lookup"><span data-stu-id="ac391-214">In hello **Connect** dialog, expand hello **Azure** node.</span></span> <span data-ttu-id="ac391-215">Všechny instance databáze SQL v Azure jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="ac391-215">All your SQL Database instances in Azure are listed here.</span></span>

<span data-ttu-id="ac391-216">Vyberte hello `DotNetAppSqlDb` databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="ac391-216">Select hello `DotNetAppSqlDb` SQL Database.</span></span> <span data-ttu-id="ac391-217">v dolní části hello se vyplní automaticky Hello připojení, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ac391-217">hello connection you created earlier is automatically filled at hello bottom.</span></span>

<span data-ttu-id="ac391-218">Zadejte heslo správce databáze hello jste dříve vytvořili a klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="ac391-218">Type hello database administrator password you created earlier and click **Connect**.</span></span>

![Konfigurace připojení k databázi ze sady Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a><span data-ttu-id="ac391-220">Povolit připojení klienta z vašeho počítače</span><span class="sxs-lookup"><span data-stu-id="ac391-220">Allow client connection from your computer</span></span>

<span data-ttu-id="ac391-221">Hello **vytvořit nové pravidlo brány firewall** se otevře dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ac391-221">hello **Create a new firewall rule** dialog is opened.</span></span> <span data-ttu-id="ac391-222">Ve výchozím nastavení umožňuje vaší instanci služby SQL Database pouze připojení ze služby Azure, jako je například Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-222">By default, your SQL Database instance only allows connections from Azure services, such as your Azure web app.</span></span> <span data-ttu-id="ac391-223">tooconnect tooyour databáze, vytvořte pravidlo brány firewall v instanci služby SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="ac391-223">tooconnect tooyour database, create a firewall rule in hello SQL Database instance.</span></span> <span data-ttu-id="ac391-224">pravidlo brány firewall Hello umožňuje hello veřejnou IP adresu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="ac391-224">hello firewall rule allows hello public IP address of your local computer.</span></span>

<span data-ttu-id="ac391-225">Dialogové okno Hello je již vyplněn veřejná IP adresa vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="ac391-225">hello dialog is already filled with your computer's public IP address.</span></span>

<span data-ttu-id="ac391-226">Ujistěte se, že **přidat Moje IP adresa klienta** je vybrána a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac391-226">Make sure that **Add my client IP** is selected and click **OK**.</span></span> 

![Nastavení brány firewall pro instanci databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

<span data-ttu-id="ac391-228">Jakmile sady Visual Studio dokončí vytváření hello nastavení brány firewall pro instanci SQL databáze, připojení se zobrazí v **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ac391-228">Once Visual Studio finishes creating hello firewall setting for your SQL Database instance, your connection shows up in **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="ac391-229">Zde můžete provádět hello nejběžnější databázových operací, například spuštění dotazů, vytvořit zobrazení a uložených procedur a další.</span><span class="sxs-lookup"><span data-stu-id="ac391-229">Here, you can perform hello most common database operations, such as run queries, create views and stored procedures, and more.</span></span> 

<span data-ttu-id="ac391-230">Klikněte pravým tlačítkem na hello `Todoes` tabulky a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ac391-230">Right-click on hello `Todoes` table and select **View Data**.</span></span> 

![Prozkoumejte objekty databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a><span data-ttu-id="ac391-232">Aktualizovat aplikaci migrace Code First</span><span class="sxs-lookup"><span data-stu-id="ac391-232">Update app with Code First Migrations</span></span>

<span data-ttu-id="ac391-233">Můžete vytvořit hello známých nástrojů v sadě Visual Studio tooupdate databáze a webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-233">You can use hello familiar tools in Visual Studio tooupdate your database and web app in Azure.</span></span> <span data-ttu-id="ac391-234">V tomto kroku použijete migrace Code First v Entity Framework toomake schéma databáze tooyour změn a publikujete ji tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ac391-234">In this step, you use Code First Migrations in Entity Framework toomake a change tooyour database schema and publish it tooAzure.</span></span>

<span data-ttu-id="ac391-235">Další informace o použití migrace Entity Framework Code First najdete v tématu [Začínáme s Entity Framework 6 Code First pomocí MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="ac391-235">For more information about using Entity Framework Code First Migrations, see [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="update-your-data-model"></a><span data-ttu-id="ac391-236">Aktualizovat data modelu</span><span class="sxs-lookup"><span data-stu-id="ac391-236">Update your data model</span></span>

<span data-ttu-id="ac391-237">Otevřete _Models\Todo.cs_ v editoru kódu hello.</span><span class="sxs-lookup"><span data-stu-id="ac391-237">Open _Models\Todo.cs_ in hello code editor.</span></span> <span data-ttu-id="ac391-238">Přidejte následující vlastnost toohello hello `ToDo` třídy:</span><span class="sxs-lookup"><span data-stu-id="ac391-238">Add hello following property toohello `ToDo` class:</span></span>

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a><span data-ttu-id="ac391-239">Spustit migrace Code First místně</span><span class="sxs-lookup"><span data-stu-id="ac391-239">Run Code First Migrations locally</span></span>

<span data-ttu-id="ac391-240">Spusťte několik příkazů toomake aktualizace tooyour místní databáze.</span><span class="sxs-lookup"><span data-stu-id="ac391-240">Run a few commands toomake updates tooyour local database.</span></span> 

<span data-ttu-id="ac391-241">Z hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="ac391-241">From hello **Tools** menu, click **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="ac391-242">V okně konzoly Správce balíčků hello povolte migrace Code First:</span><span class="sxs-lookup"><span data-stu-id="ac391-242">In hello Package Manager Console window, enable Code First Migrations:</span></span>

```PowerShell
Enable-Migrations
```

<span data-ttu-id="ac391-243">Přidejte migrace:</span><span class="sxs-lookup"><span data-stu-id="ac391-243">Add a migration:</span></span>

```PowerShell
Add-Migration AddProperty
```

<span data-ttu-id="ac391-244">Aktualizujte místní databázi hello:</span><span class="sxs-lookup"><span data-stu-id="ac391-244">Update hello local database:</span></span>

```PowerShell
Update-Database
```

<span data-ttu-id="ac391-245">Typ `Ctrl+F5` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-245">Type `Ctrl+F5` toorun hello app.</span></span> <span data-ttu-id="ac391-246">Test hello upravit podrobnosti a vytvoření odkazů.</span><span class="sxs-lookup"><span data-stu-id="ac391-246">Test hello edit, details, and create links.</span></span>

<span data-ttu-id="ac391-247">Pokud aplikace hello načte bez chyb, proběhla úspěšně migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="ac391-247">If hello application loads without errors, then Code First Migrations has succeeded.</span></span> <span data-ttu-id="ac391-248">Však stále vypadá vaše stránka hello stejné protože aplikační logiku ještě nepoužívá tuto novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ac391-248">However, your page still looks hello same because your application logic is not using this new property yet.</span></span> 

### <a name="use-hello-new-property"></a><span data-ttu-id="ac391-249">Použít novou vlastnost hello</span><span class="sxs-lookup"><span data-stu-id="ac391-249">Use hello new property</span></span>

<span data-ttu-id="ac391-250">Provedeme některé změny v váš kód toouse hello `Done` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ac391-250">Make some changes in your code toouse hello `Done` property.</span></span> <span data-ttu-id="ac391-251">Pro zjednodušení v tomto kurzu pouze budete toochange hello `Index` a `Create` zobrazení vlastnost hello toosee v akci.</span><span class="sxs-lookup"><span data-stu-id="ac391-251">For simplicity in this tutorial, you're only going toochange hello `Index` and `Create` views toosee hello property in action.</span></span>

<span data-ttu-id="ac391-252">Otevřete _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="ac391-252">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="ac391-253">Najde hello `Create()` metoda a přidejte `Done` toohello seznam vlastností v hello `Bind` atribut.</span><span class="sxs-lookup"><span data-stu-id="ac391-253">Find hello `Create()` method and add `Done` toohello list of properties in hello `Bind` attribute.</span></span> <span data-ttu-id="ac391-254">Když jste hotovi, vaše `Create()` podpis metody vypadá hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="ac391-254">When you're done, your `Create()` method signature looks like hello following code:</span></span>

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

<span data-ttu-id="ac391-255">Otevřete _Views\Todos\Create.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="ac391-255">Open _Views\Todos\Create.cshtml_.</span></span>

<span data-ttu-id="ac391-256">V hello kódu Razor, měli byste vidět `<div class="form-group">` element, který používá `model.Description`a pak další `<div class="form-group">` element, který používá `model.CreatedDate`.</span><span class="sxs-lookup"><span data-stu-id="ac391-256">In hello Razor code, you should see a `<div class="form-group">` element that uses `model.Description`, and then another `<div class="form-group">` element that uses `model.CreatedDate`.</span></span> <span data-ttu-id="ac391-257">Hned za tyto dva prvky, přidejte další `<div class="form-group">` element, který používá `model.Done`:</span><span class="sxs-lookup"><span data-stu-id="ac391-257">Immediately following these two elements, add another `<div class="form-group">` element that uses `model.Done`:</span></span>

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

<span data-ttu-id="ac391-258">Otevřete _Views\Todos\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="ac391-258">Open _Views\Todos\Index.cshtml_.</span></span>

<span data-ttu-id="ac391-259">Vyhledejte hello prázdný `<th></th>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ac391-259">Search for hello empty `<th></th>` element.</span></span> <span data-ttu-id="ac391-260">Nad tento element přidejte následující kódu Razor hello:</span><span class="sxs-lookup"><span data-stu-id="ac391-260">Just above this element, add hello following Razor code:</span></span>

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

<span data-ttu-id="ac391-261">Najde hello `<td>` elementu, který obsahuje hello `Html.ActionLink()` pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="ac391-261">Find hello `<td>` element that contains hello `Html.ActionLink()` helper methods.</span></span> <span data-ttu-id="ac391-262">Nad tento element přidejte následující kódu Razor hello:</span><span class="sxs-lookup"><span data-stu-id="ac391-262">Just above this element, add hello following Razor code:</span></span>

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

<span data-ttu-id="ac391-263">To je všechno potřebujete toosee hello změny v hello `Index` a `Create` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ac391-263">That's all you need toosee hello changes in hello `Index` and `Create` views.</span></span> 

<span data-ttu-id="ac391-264">Typ `Ctrl+F5` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-264">Type `Ctrl+F5` toorun hello app.</span></span>

<span data-ttu-id="ac391-265">Teď můžete přidat položku seznamu úkolů a zkontrolujte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="ac391-265">You can now add a to-do item and check **Done**.</span></span> <span data-ttu-id="ac391-266">Potom ho měli zobrazí v domovské stránce jako dokončené položka.</span><span class="sxs-lookup"><span data-stu-id="ac391-266">Then it should show up in your homepage as a completed item.</span></span> <span data-ttu-id="ac391-267">Mějte na paměti, že hello `Edit` zobrazení nezobrazí hello `Done` pole, protože jste nezměnili hello `Edit` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ac391-267">Remember that hello `Edit` view doesn't show hello `Done` field, because you didn't change hello `Edit` view.</span></span>

### <a name="enable-code-first-migrations-in-azure"></a><span data-ttu-id="ac391-268">Povolení migrace Code First v Azure</span><span class="sxs-lookup"><span data-stu-id="ac391-268">Enable Code First Migrations in Azure</span></span>

<span data-ttu-id="ac391-269">Teď, když kód změnit funguje, včetně migrace databáze, můžete ji publikovat tooyour webové aplikace Azure a příliš aktualizace databáze SQL se migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="ac391-269">Now that your code change works, including database migration, you publish it tooyour Azure web app and update your SQL Database with Code First Migrations too.</span></span>

<span data-ttu-id="ac391-270">Podobně jako před, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ac391-270">Just like before, right-click your project and select **Publish**.</span></span>

<span data-ttu-id="ac391-271">Klikněte na tlačítko **nastavení** Průvodce publikováním tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="ac391-271">Click **Settings** tooopen hello publish wizard.</span></span>

![Otevřete nastavení publikování](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

<span data-ttu-id="ac391-273">V Průvodci hello, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ac391-273">In hello wizard, click **Next**.</span></span>

<span data-ttu-id="ac391-274">Zajistěte, aby tento hello připojovací řetězec pro databáze SQL se naplní v **MyDatabaseContext (MyDbConnection)**.</span><span class="sxs-lookup"><span data-stu-id="ac391-274">Make sure that hello connection string for your SQL Database is populated in **MyDatabaseContext (MyDbConnection)**.</span></span> <span data-ttu-id="ac391-275">Může být nutné tooselect hello **myToDoAppDb** databáze z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ac391-275">You may need tooselect hello **myToDoAppDb** database from hello dropdown.</span></span> 

<span data-ttu-id="ac391-276">Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**, pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ac391-276">Select **Execute Code First Migrations (runs on application start)**, then click **Save**.</span></span>

![Povolení migrace Code First ve webové aplikace Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a><span data-ttu-id="ac391-278">Publikování změn</span><span class="sxs-lookup"><span data-stu-id="ac391-278">Publish your changes</span></span>

<span data-ttu-id="ac391-279">Teď, když jste povolili migrace Code First ve vaší webové aplikace Azure, publikování změn kódu.</span><span class="sxs-lookup"><span data-stu-id="ac391-279">Now that you enabled Code First Migrations in your Azure web app, publish your code changes.</span></span>

<span data-ttu-id="ac391-280">V hello publikovat stránku, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ac391-280">In hello publish page, click **Publish**.</span></span>

<span data-ttu-id="ac391-281">Přidejte položkami seznamu úkolů znovu a vyberte **provádí**, a měli objeví v domovské stránce jako dokončené položka.</span><span class="sxs-lookup"><span data-stu-id="ac391-281">Try adding to-do items again and select **Done**, and they should show up in your homepage as a completed item.</span></span>

![Webové aplikace Azure po migraci první kódu](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

<span data-ttu-id="ac391-283">Všechny vaše existující položky seznamu úkolů se pořád zobrazí.</span><span class="sxs-lookup"><span data-stu-id="ac391-283">All your existing to-do items are still displayed.</span></span> <span data-ttu-id="ac391-284">Při opětovném aplikace ASP.NET, není stávající data v databázi SQL ztraceny.</span><span class="sxs-lookup"><span data-stu-id="ac391-284">When you republish your ASP.NET application, existing data in your SQL Database is not lost.</span></span> <span data-ttu-id="ac391-285">Navíc migrace Code First pouze změní schéma dat hello a svoje existující data zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="ac391-285">Also, Code First Migrations only changes hello data schema and leaves your existing data intact.</span></span>


## <a name="stream-application-logs"></a><span data-ttu-id="ac391-286">Protokoly aplikací datového proudu</span><span class="sxs-lookup"><span data-stu-id="ac391-286">Stream application logs</span></span>

<span data-ttu-id="ac391-287">Přímo z vaší službě Azure web app tooVisual Studio dá Streamovat trasování zpráv.</span><span class="sxs-lookup"><span data-stu-id="ac391-287">You can stream tracing messages directly from your Azure web app tooVisual Studio.</span></span>

<span data-ttu-id="ac391-288">Otevřete _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="ac391-288">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="ac391-289">Každá akce začíná `Trace.WriteLine()` metoda.</span><span class="sxs-lookup"><span data-stu-id="ac391-289">Each action starts with a `Trace.WriteLine()` method.</span></span> <span data-ttu-id="ac391-290">Tento kód se přidá tooshow můžete jak tooadd trasování zprávy tooyour webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-290">This code is added tooshow you how tooadd trace messages tooyour Azure web app.</span></span>

### <a name="open-server-explorer"></a><span data-ttu-id="ac391-291">V Průzkumníku serveru otevřete</span><span class="sxs-lookup"><span data-stu-id="ac391-291">Open Server Explorer</span></span>

<span data-ttu-id="ac391-292">Z hello **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="ac391-292">From hello **View** menu, select **Server Explorer**.</span></span> <span data-ttu-id="ac391-293">Můžete nakonfigurovat protokolování pro Azure webové aplikace v **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="ac391-293">You can configure logging for your Azure web app in **Server Explorer**.</span></span> 

### <a name="enable-log-streaming"></a><span data-ttu-id="ac391-294">Povolení protokolu streamování</span><span class="sxs-lookup"><span data-stu-id="ac391-294">Enable log streaming</span></span>

<span data-ttu-id="ac391-295">V **Průzkumníka serveru**, rozbalte položku **Azure** > **služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="ac391-295">In **Server Explorer**, expand **Azure** > **App Service**.</span></span>

<span data-ttu-id="ac391-296">Rozbalte hello **myResourceGroup** skupinu prostředků, jste vytvořili při prvním vytvoření hello webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-296">Expand hello **myResourceGroup** resource group, you created when you first created hello Azure web app.</span></span>

<span data-ttu-id="ac391-297">Klikněte pravým tlačítkem na vaší webové aplikace Azure a vyberte **zobrazit protokoly streamování**.</span><span class="sxs-lookup"><span data-stu-id="ac391-297">Right-click your Azure web app and select **View Streaming Logs**.</span></span>

![Povolení protokolu streamování](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

<span data-ttu-id="ac391-299">Hello protokoly jsou nyní datového proudu do hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="ac391-299">hello logs are now streamed into hello **Output** window.</span></span> 

![V okně výstupu datový proud protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

<span data-ttu-id="ac391-301">Ale nezobrazí žádná hello trasování zpráv ještě.</span><span class="sxs-lookup"><span data-stu-id="ac391-301">However, you don't see any of hello trace messages yet.</span></span> <span data-ttu-id="ac391-302">To je proto když nejprve vyberete **zobrazit protokoly streamování**, vaší webové aplikace Azure nastaví úroveň trasování hello příliš`Error`, který jenom protokoluje události chyby (s hello `Trace.TraceError()` metoda).</span><span class="sxs-lookup"><span data-stu-id="ac391-302">That's because when you first select **View Streaming Logs**, your Azure web app sets hello trace level too`Error`, which only logs error events (with hello `Trace.TraceError()` method).</span></span>

### <a name="change-trace-levels"></a><span data-ttu-id="ac391-303">Změna úrovně trasování</span><span class="sxs-lookup"><span data-stu-id="ac391-303">Change trace levels</span></span>

<span data-ttu-id="ac391-304">trasování hello toochange úrovně toooutput dalších trasování zpráv, přejděte zpět příliš**Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="ac391-304">toochange hello trace levels toooutput other trace messages, go back too**Server Explorer**.</span></span>

<span data-ttu-id="ac391-305">Znovu klikněte pravým tlačítkem Azure webové aplikace a vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ac391-305">Right-click your Azure web app again and select **Settings**.</span></span>

<span data-ttu-id="ac391-306">V hello **protokolování aplikace (systém souborů)** rozevíracího seznamu vyberte **podrobné**.</span><span class="sxs-lookup"><span data-stu-id="ac391-306">In hello **Application Logging (File System)** dropdown, select **Verbose**.</span></span> <span data-ttu-id="ac391-307">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ac391-307">Click **Save**.</span></span>

![Změna úrovně tooVerbose trasování](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> <span data-ttu-id="ac391-309">Můžete experimentovat s jinou trasování úrovně toosee jaké typy zprávy se zobrazují pro každou úroveň.</span><span class="sxs-lookup"><span data-stu-id="ac391-309">You can experiment with different trace levels toosee what types of messages are displayed for each level.</span></span> <span data-ttu-id="ac391-310">Například hello **informace** úroveň zahrnuje všechny protokoly, které jsou vytvořené `Trace.TraceInformation()`, `Trace.TraceWarning()`, a `Trace.TraceError()`, ale není protokoly vytvořené `Trace.WriteLine()`.</span><span class="sxs-lookup"><span data-stu-id="ac391-310">For example, hello **Information** level includes all logs created by `Trace.TraceInformation()`, `Trace.TraceWarning()`, and `Trace.TraceError()`, but not logs created by `Trace.WriteLine()`.</span></span>
>
>

<span data-ttu-id="ac391-311">V prohlížeči zkuste klepnout kolem hello aplikaci seznamu úkolů v Azure.</span><span class="sxs-lookup"><span data-stu-id="ac391-311">In your browser, try clicking around hello to-do list application in Azure.</span></span> <span data-ttu-id="ac391-312">Hello trasovací zprávy jsou nyní streamování toohello **výstup** oken v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac391-312">hello trace messages are now streamed toohello **Output** window in Visual Studio.</span></span>

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a><span data-ttu-id="ac391-313">Zastavit streamování protokolu</span><span class="sxs-lookup"><span data-stu-id="ac391-313">Stop log streaming</span></span>

<span data-ttu-id="ac391-314">toostop hello vysílání datového proudu protokolu služby, klikněte na tlačítko hello **zastavit monitorování** tlačítka na hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="ac391-314">toostop hello log-streaming service, click hello **Stop monitoring** button in hello **Output** window.</span></span>

![Zastavit streamování protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="ac391-316">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ac391-316">Manage your Azure web app</span></span>

<span data-ttu-id="ac391-317">Přejděte toohello [portál Azure](https://portal.azure.com) toosee hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ac391-317">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span> 



<span data-ttu-id="ac391-318">V levé nabídce hello, klikněte na **služby App Service**, pak klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-318">From hello left menu, click **App Service**, then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

<span data-ttu-id="ac391-320">Dostali jste na stránce vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-320">You have landed in your web app's page.</span></span> 

<span data-ttu-id="ac391-321">Ve výchozím nastavení, hello portál zobrazuje hello **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="ac391-321">By default, hello portal shows hello **Overview** page.</span></span> <span data-ttu-id="ac391-322">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="ac391-322">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="ac391-323">Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="ac391-323">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="ac391-324">Hello karty na levé straně stránky hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="ac391-324">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span> 

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="ac391-326">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac391-326">Next steps</span></span>

<span data-ttu-id="ac391-327">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="ac391-327">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ac391-328">Vytvoření databáze SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="ac391-328">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="ac391-329">Připojit tooSQL aplikace ASP.NET databáze</span><span class="sxs-lookup"><span data-stu-id="ac391-329">Connect an ASP.NET app tooSQL Database</span></span>
> * <span data-ttu-id="ac391-330">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="ac391-330">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="ac391-331">Aktualizovat hello datový model a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="ac391-331">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="ac391-332">Datový proud protokolů z Azure tooyour terminálu</span><span class="sxs-lookup"><span data-stu-id="ac391-332">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="ac391-333">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ac391-333">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="ac391-334">Posunutí toohello další kurz toolearn jak toomap vlastní DNS název toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac391-334">Advance toohello next tutorial toolearn how toomap a custom DNS name toohello web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ac391-335">Mapovat existující vlastní DNS název tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ac391-335">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
