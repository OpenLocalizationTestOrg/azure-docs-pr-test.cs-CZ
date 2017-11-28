---
title: "aaaCreate aplikace obchodní Azure pomocí ověřování Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toocreate rozhraní ASP.NET MVC – obchodní aplikace v Azure App Service, se ověřuje Azure Active Directory"
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: ad947bdb-4463-43ff-a5e3-91d9b2169b60
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin
ms.openlocfilehash: 3bcafad78ac0151889b3e336784cc561009f244f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a><span data-ttu-id="d94a4-103">Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d94a4-103">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>
<span data-ttu-id="d94a4-104">Tento článek ukazuje, jak toocreate .NET – obchodní aplikace v [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí hello [ověřování / autorizace](../app-service/app-service-authentication-overview.md) funkce.</span><span class="sxs-lookup"><span data-stu-id="d94a4-104">This article shows you how toocreate a .NET line-of-business app in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) using hello [Authentication / Authorization](../app-service/app-service-authentication-overview.md) feature.</span></span> <span data-ttu-id="d94a4-105">Také ukazuje, jak toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery data adresáře v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-105">It also shows how toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery directory data in hello application.</span></span>

<span data-ttu-id="d94a4-106">Hello klienta Azure Active Directory, který používáte, může být adresář jen Azure.</span><span class="sxs-lookup"><span data-stu-id="d94a4-106">hello Azure Active Directory tenant that you use can be an Azure-only directory.</span></span> <span data-ttu-id="d94a4-107">Nebo může být [synchronizaci se službou Active Directory v místě](../active-directory/active-directory-aadconnect.md) toocreate jediného přihlašování rozhraní pro pracovní procesy, které jsou místní a vzdálené.</span><span class="sxs-lookup"><span data-stu-id="d94a4-107">Or, it can be [synced with your on-premises Active Directory](../active-directory/active-directory-aadconnect.md) toocreate a single sign-on experience for workers that are on-premises and remote.</span></span> <span data-ttu-id="d94a4-108">Tento článek používá hello výchozí adresář pro váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d94a4-108">This article uses hello default directory for your Azure account.</span></span>

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="d94a4-109">Co se sestavení</span><span class="sxs-lookup"><span data-stu-id="d94a4-109">What you will build</span></span>
<span data-ttu-id="d94a4-110">Bude vytvořit jednoduchou aplikaci vytvořit-čtení-aktualizace-odstranění (CRUD)-obchodní v App Service Web Apps, že sleduje pracovní položky s hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="d94a4-110">You will build a simple line-of-business Create-Read-Update-Delete (CRUD) application in App Service Web Apps that tracks work items with hello following features:</span></span>

* <span data-ttu-id="d94a4-111">Ověřuje uživatele na základě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d94a4-111">Authenticates users against Azure Active Directory</span></span>
* <span data-ttu-id="d94a4-112">Dotazuje adresáře uživatelů a skupin pomocí [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span><span class="sxs-lookup"><span data-stu-id="d94a4-112">Queries directory users and groups using [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span></span>
* <span data-ttu-id="d94a4-113">Použití hello ASP.NET MVC *bez ověřování* šablony</span><span class="sxs-lookup"><span data-stu-id="d94a4-113">Use hello ASP.NET MVC *No Authentication* template</span></span>

<span data-ttu-id="d94a4-114">Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v Azure, najdete v části [další krok](#next).</span><span class="sxs-lookup"><span data-stu-id="d94a4-114">If you need role-based access control (RBAC) for your line-of-business app in Azure, see [Next Step](#next).</span></span>

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="d94a4-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="d94a4-115">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="d94a4-116">V tomto kurzu se třeba hello následující toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d94a4-116">You need hello following toocomplete this tutorial:</span></span>

* <span data-ttu-id="d94a4-117">Klient služby Azure Active Directory s uživateli v různých skupinách</span><span class="sxs-lookup"><span data-stu-id="d94a4-117">An Azure Active Directory tenant with users in various groups</span></span>
* <span data-ttu-id="d94a4-118">Oprávnění aplikací toocreate na klienta Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="d94a4-118">Permissions toocreate applications on hello Azure Active Directory tenant</span></span>
* <span data-ttu-id="d94a4-119">Visual Studio 2013 Update 4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d94a4-119">Visual Studio 2013 Update 4 or later</span></span>
* [<span data-ttu-id="d94a4-120">Azure SDK 2.8.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d94a4-120">Azure SDK 2.8.1 or later</span></span>](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-tooazure"></a><span data-ttu-id="d94a4-121">Vytvoření a nasazení webové aplikace tooAzure</span><span class="sxs-lookup"><span data-stu-id="d94a4-121">Create and deploy a web app tooAzure</span></span>
1. <span data-ttu-id="d94a4-122">Ze sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-122">From Visual Studio, click **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="d94a4-123">Vyberte **webové aplikace ASP.NET**, pojmenujte svůj projekt a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-123">Select **ASP.NET Web Application**, name your project, and click **OK**.</span></span>
3. <span data-ttu-id="d94a4-124">Vyberte hello **MVC** šablony, změňte hello ověřování příliš**bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-124">Select hello **MVC** template, then change hello authentication too**No Authentication**.</span></span> <span data-ttu-id="d94a4-125">Zajistěte, aby **hostitel v cloudu hello** je vybrána a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-125">Make sure **Host in hello Cloud** is selected and click **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. <span data-ttu-id="d94a4-126">V hello **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **přidat účet** (a potom **přidat účet** v rozevírací nabídce hello) toolog v tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d94a4-126">In hello **Create App Service** dialog, click **Add an account** (and then **Add an account** in hello dropdown) toolog in tooyour Azure account.</span></span>
5. <span data-ttu-id="d94a4-127">Po přihlášení nakonfigurujte webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d94a4-127">Once logged in configure your web app.</span></span> <span data-ttu-id="d94a4-128">Vytvořit skupinu prostředků a nový plán služby App Service kliknutím na tlačítko hello příslušných **nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d94a4-128">Create a resource group and a new App Service plan by clicking hello respective **New** button.</span></span> <span data-ttu-id="d94a4-129">Klikněte na tlačítko **Objevte další služby Azure** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="d94a4-129">Click **Explore additional Azure services** toocontinue.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. <span data-ttu-id="d94a4-130">V hello **služby** , klikněte na  **+**  tooadd databáze SQL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d94a4-130">In hello **Services** tab, click **+** tooadd a SQL Database for your app.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. <span data-ttu-id="d94a4-131">V **nakonfigurovat databázi SQL**, klikněte na tlačítko **nový** toocreate instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d94a4-131">In **Configure SQL Database**, click **New** toocreate a SQL Server instance.</span></span>
8. <span data-ttu-id="d94a4-132">V **nakonfigurujte systém SQL Server**, nakonfigurovat instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="d94a4-132">In **Configure SQL Server**, configure your SQL Server instance.</span></span> <span data-ttu-id="d94a4-133">Potom klikněte na **OK**, **OK**, a **vytvořit** tookick vypnout hello vytváření aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="d94a4-133">Then, click **OK**, **OK**, and **Create** tookick off hello app creation in Azure.</span></span>
9. <span data-ttu-id="d94a4-134">V **aktivita služby Azure App Service**, se zobrazí po dokončení vytváření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-134">In **Azure App Service Activity**, you can see when hello app creation is finished.</span></span> <span data-ttu-id="d94a4-135">Klikněte na tlačítko  **publikovat &lt;* appname*> toothis webové aplikace teď **, pak klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-135">Click **Publish &lt;*appname*> toothis Web App now**, then click **Publish**.</span></span> 
   
    <span data-ttu-id="d94a4-136">Jakmile sady Visual Studio dokončí, otevře se hello publikování aplikace v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-136">Once Visual Studio finishes, it opens hello publish app in hello browser.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a><span data-ttu-id="d94a4-137">Konfigurace ověřování a directory přístupu</span><span class="sxs-lookup"><span data-stu-id="d94a4-137">Configure authentication and directory access</span></span>
1. <span data-ttu-id="d94a4-138">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d94a4-138">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d94a4-139">V levé nabídce hello, klikněte na **App Services** > **&lt;*appname*> ** > **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-139">From hello left menu, click **App Services** > **&lt;*appname*>** > **Authentication / Authorization**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. <span data-ttu-id="d94a4-140">Zapnout ověřování Azure Active Directory kliknutím **na** > **Azure Active Directory** > **Express** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-140">Turn on Azure Active Directory authentication by clicking **On** > **Azure Active Directory** > **Express** > **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. <span data-ttu-id="d94a4-141">Klikněte na tlačítko **Uložit** hello panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d94a4-141">Click **Save** in hello command bar.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    <span data-ttu-id="d94a4-142">Po nastavení hello ověřování se úspěšně uložily, zkuste navigace tooyour aplikaci znovu v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-142">Once hello authentication settings are saved successfully, try navigating tooyour app again in hello browser.</span></span> <span data-ttu-id="d94a4-143">Výchozí nastavení vynucení ověření hello celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d94a4-143">Your default settings enforce authentication on hello whole app.</span></span> <span data-ttu-id="d94a4-144">Pokud už nejste přihlášeni, budete přesměrovaného tooa přihlašovací obrazovku.</span><span class="sxs-lookup"><span data-stu-id="d94a4-144">If you aren't already logged in, you are redirected tooa login screen.</span></span> <span data-ttu-id="d94a4-145">Po přihlášení, by se zobrazit aplikaci zabezpečené pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d94a4-145">Once logged in, you see your app secured by HTTPS.</span></span> <span data-ttu-id="d94a4-146">Dále musíte tooenable přístup toodirectory data.</span><span class="sxs-lookup"><span data-stu-id="d94a4-146">Next, you need tooenable access toodirectory data.</span></span> 
5. <span data-ttu-id="d94a4-147">Přejděte toohello [portálu classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d94a4-147">Navigate toohello [classic portal](https://manage.windowsazure.com).</span></span>
6. <span data-ttu-id="d94a4-148">V levé nabídce hello, klikněte na **služby Active Directory** > **výchozí adresář** > **aplikace**  >   **&lt;* appname*> **.</span><span class="sxs-lookup"><span data-stu-id="d94a4-148">From hello left menu, click **Active Directory** > **Default Directory** > **Applications** > **&lt;*appname*>**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    <span data-ttu-id="d94a4-149">Toto je aplikace hello Azure Active Directory, vytvořené pro jste tooenable hello autorizace služby App Service nebo funkce ověřování.</span><span class="sxs-lookup"><span data-stu-id="d94a4-149">This is hello Azure Active Directory application that App Service created for you tooenable hello Authorization / Authentication feature.</span></span>
7. <span data-ttu-id="d94a4-150">Klikněte na tlačítko **uživatelé** a **skupiny** toomake, že máte některé uživatele a skupiny v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-150">Click **Users** and **Groups** toomake sure that you have some users and groups in hello directory.</span></span> <span data-ttu-id="d94a4-151">Pokud ne, vytvořte několik testovací uživatele a skupiny.</span><span class="sxs-lookup"><span data-stu-id="d94a4-151">If not, create a few test users and groups.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. <span data-ttu-id="d94a4-152">Klikněte na tlačítko **konfigurace** tooconfigure této aplikace.</span><span class="sxs-lookup"><span data-stu-id="d94a4-152">Click **Configure** tooconfigure this application.</span></span>
9. <span data-ttu-id="d94a4-153">Projděte dolů toohello **klíče** a přidejte klíč výběrem dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="d94a4-153">Scroll down toohello **Keys** section and add a key by selecting a duration.</span></span> <span data-ttu-id="d94a4-154">Potom klikněte na **delegovaná oprávnění** a vyberte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-154">Then, click **Delegated Permissions** and select **Read directory data**.</span></span> 
   <span data-ttu-id="d94a4-155">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-155">Click **Save**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. <span data-ttu-id="d94a4-156">Jakmile se vaše nastavení se ukládají, posuňte se zálohování toohello **klíče** části a klikněte na tlačítko hello **kopie** klíč klienta hello toocopy tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d94a4-156">Once your settings are saved, scroll back up toohello **Keys** section and click hello **Copy** button toocopy hello client key.</span></span> 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="d94a4-157">Pokud jste tuto stránku opustit nyní, nebudete moct tooaccess někdy znovu klíče tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="d94a4-157">If you navigate away from this page now, you won't be able tooaccess this client key ever again.</span></span>
    > 
    > 
11. <span data-ttu-id="d94a4-158">Dále musíte tooconfigure vaší webové aplikace s tímto klíčem.</span><span class="sxs-lookup"><span data-stu-id="d94a4-158">Next, you need tooconfigure your web app with this key.</span></span> <span data-ttu-id="d94a4-159">Přihlaste se toohello [Průzkumníka prostředků Azure](https://resources.azure.com) s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="d94a4-159">Log in toohello [Azure Resource Explorer](https://resources.azure.com) with your Azure account.</span></span>
12. <span data-ttu-id="d94a4-160">V horní části hello hello stránky, klikněte na tlačítko **pro čtení a zápis** toomake změny v hello Průzkumníka prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d94a4-160">At hello top of hello page, click **Read/Write** toomake changes in hello Azure Resource Explorer.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. <span data-ttu-id="d94a4-161">Najít nastavení ověřování pro aplikace umístěné v odběry hello >  **&lt;* název_předplatného*> ** > **Skupinyprostředků**  >   **&lt;* resourcegroupname*> ** > **zprostředkovatelé** > **Microsoft.Web**  >  **lokality** > **&lt;*appname*> ** > **konfigurace**  >  **authsettings**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-161">Find hello authentication settings for your app, located at subscriptions > **&lt;*subscriptionname*>** > **resourceGroups** > **&lt;*resourcegroupname*>** > **providers** > **Microsoft.Web** > **sites** > **&lt;*appname*>** > **config** > **authsettings**.</span></span>
14. <span data-ttu-id="d94a4-162">Klikněte na **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-162">Click **Edit**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. <span data-ttu-id="d94a4-163">V podokně úpravy hello, nastavte hello `clientSecret` a `additionalLoginParams` vlastnosti následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d94a4-163">In hello editing pane, set hello `clientSecret` and `additionalLoginParams` properties as follows.</span></span>
    
        ...
        "clientSecret": "<client key from hello Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. <span data-ttu-id="d94a4-164">Klikněte na tlačítko **Put** v hello nejvyšší toosubmit změny.</span><span class="sxs-lookup"><span data-stu-id="d94a4-164">Click **Put** at hello top toosubmit your changes.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. <span data-ttu-id="d94a4-165">Nyní, tootest, pokud máte hello autorizační token tooaccess hello Azure Active Directory Graph API, jednoduše přejděte k  **https://&lt;*appname*>.azurewebsites.net/.auth/me** ve vaší prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="d94a4-165">Now, tootest if you have hello authorization token tooaccess hello Azure Active Directory Graph API, just navigate to **https://&lt;*appname*>.azurewebsites.net/.auth/me** in your browser.</span></span> <span data-ttu-id="d94a4-166">Pokud jste nakonfigurovali všechno správně, měli byste vidět hello `access_token` vlastnost hello odpověď JSON.</span><span class="sxs-lookup"><span data-stu-id="d94a4-166">If you configured everything correctly, you should see hello `access_token` property in hello JSON response.</span></span>
    
    <span data-ttu-id="d94a4-167">Hello `~/.auth/me` cestu adresy URL spravuje aplikace služby ověřování / autorizace toogive můžete všechny hello informace týkající se relace tooyour ověření.</span><span class="sxs-lookup"><span data-stu-id="d94a4-167">hello `~/.auth/me` URL path is managed by App Service Authentication / Authorization toogive you all hello information related tooyour authenticated session.</span></span> <span data-ttu-id="d94a4-168">Další informace najdete v tématu [ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d94a4-168">For more information, see [Authentication and authorization in Azure App Service](../app-service/app-service-authentication-overview.md).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="d94a4-169">Hello `access_token` má dobu platnosti.</span><span class="sxs-lookup"><span data-stu-id="d94a4-169">hello `access_token` has an expiration period.</span></span> <span data-ttu-id="d94a4-170">Ale aplikace služby ověřování / autorizace poskytuje funkce tokenu aktualizace s `~/.auth/refresh`.</span><span class="sxs-lookup"><span data-stu-id="d94a4-170">However, App Service Authentication / Authorization provides token refresh functionality with `~/.auth/refresh`.</span></span> <span data-ttu-id="d94a4-171">Další informace o tom, toouse, najdete v části [obchod Token služby](https://cgillum.tech/2016/03/07/app-service-token-store/).</span><span class="sxs-lookup"><span data-stu-id="d94a4-171">For more information on how toouse it, see [App Service Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).</span></span>
    > 
    > 

<span data-ttu-id="d94a4-172">V dalším kroku provedete něco užitečný v případě dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="d94a4-172">Next, you will do something useful with directory data.</span></span>

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-tooyour-app"></a><span data-ttu-id="d94a4-173">Přidat aplikaci tooyour obchodní – funkce</span><span class="sxs-lookup"><span data-stu-id="d94a4-173">Add line-of-business functionality tooyour app</span></span>
<span data-ttu-id="d94a4-174">Nyní můžete vytvořit jednoduché sledovací modul CRUD položky pracovní.</span><span class="sxs-lookup"><span data-stu-id="d94a4-174">Now, you create a simple CRUD work items tracker.</span></span>  

1. <span data-ttu-id="d94a4-175">Ve složce ~\Models hello, vytvořte soubor třídy s názvem WorkItem.cs a nahraďte `public class WorkItem {...}` s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="d94a4-175">In hello ~\Models folder, create a class file called WorkItem.cs, and replace `public class WorkItem {...}` with hello following code:</span></span>
   
     <span data-ttu-id="d94a4-176">pomocí System.ComponentModel.DataAnnotations;</span><span class="sxs-lookup"><span data-stu-id="d94a4-176">using System.ComponentModel.DataAnnotations;</span></span>
   
     <span data-ttu-id="d94a4-177">Veřejná třída {pracovní položky</span><span class="sxs-lookup"><span data-stu-id="d94a4-177">public class WorkItem   {</span></span>
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     <span data-ttu-id="d94a4-178">}</span><span class="sxs-lookup"><span data-stu-id="d94a4-178">}</span></span>
   
     <span data-ttu-id="d94a4-179">Veřejný výčet WorkItemStatus {</span><span class="sxs-lookup"><span data-stu-id="d94a4-179">public enum WorkItemStatus   {</span></span>
   
         Open,
         Investigating,
         Resolved,
         Closed
     <span data-ttu-id="d94a4-180">}</span><span class="sxs-lookup"><span data-stu-id="d94a4-180">}</span></span>
2. <span data-ttu-id="d94a4-181">Sestavení projektu toomake hello nový model přístupné toohello generování uživatelského rozhraní logika v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d94a4-181">Build hello project toomake your new model accessible toohello scaffolding logic in Visual Studio.</span></span>
3. <span data-ttu-id="d94a4-182">Přidat novou položku vygenerované `WorkItemsController` toohello ~\Controllers složky (klikněte pravým tlačítkem na **řadiče**, bod příliš**přidat**a vyberte **nové vygenerované položky**).</span><span class="sxs-lookup"><span data-stu-id="d94a4-182">Add a new scaffolded item `WorkItemsController` toohello ~\Controllers folder (right-click **Controllers**, point too**Add**, and select **New scaffolded item**).</span></span> 
4. <span data-ttu-id="d94a4-183">Vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-183">Select **MVC 5 Controller with views, using Entity Framework** and click **Add**.</span></span>
5. <span data-ttu-id="d94a4-184">Vyberte hello model, který jste vytvořili, klikněte  **+**  a potom **přidat** tooadd data kontextu a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-184">Select hello model that you created, then click **+** and then **Add** tooadd a data context, and then click **Add**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. <span data-ttu-id="d94a4-185">V ~\Views\WorkItems\Create.cshtml (automaticky vygenerované položky), vyhledejte hello `Html.BeginForm` pomocnou metodu a nastavit hello následující zvýrazněný změny:</span><span class="sxs-lookup"><span data-stu-id="d94a4-185">In ~\Views\WorkItems\Create.cshtml (an automatically scaffolded item), find hello `Html.BeginForm` helper method and make hello following highlighted changes:</span></span>  
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = &quot;Create&quot;;
   }
   
   &lt;h2&gt;Create&lt;/h2&gt;
   
   @using (Html.BeginForm(<mark>&quot;Create&quot;, &quot;WorkItems&quot;, FormMethod.Post, new { id = &quot;main-form&quot; }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class=&quot;form-horizontal&quot;&gt;
        &lt;h4&gt;WorkItem&lt;/h4&gt;
        &lt;hr /&gt;
        @Html.ValidationSummary(true, &quot;&quot;, new { @class = &quot;text-danger&quot; })
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToID, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToID, new { htmlAttributes = new { @class = &quot;form-control&quot;<mark>, @type = &quot;hidden&quot;</mark> } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToID, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToName, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToName, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToName, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Description, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.Description, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.Description, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EnumDropDownListFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;form-control&quot; })
                @Html.ValidationMessageFor(model =&gt; model.Status, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            &lt;div class=&quot;col-md-offset-2 col-md-10&quot;&gt;
                &lt;input type=&quot;submit&quot; value=&quot;Create&quot; class=&quot;btn btn-default&quot;<mark> id=&quot;submit-button&quot;</mark> /&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
   }
   
   &lt;div&gt;
    @Html.ActionLink(&quot;Back tooList&quot;, &quot;Index&quot;)
   &lt;/div&gt;
   
   @section Scripts {
    @Scripts.Render(&quot;~/bundles/jqueryval&quot;)
    <mark>&lt;script&gt;
        // People/Group Picker Code
        var maxResultsPerPage = 14;
        var input = document.getElementById(&quot;AssignedToName&quot;);
   
        // Access token from request header, and tenantID from claims identity
        var token = &quot;@Request.Headers[&quot;X-MS-TOKEN-AAD-ACCESS-TOKEN&quot;]&quot;;
        var tenant =&quot;@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == &quot;http://schemas.microsoft.com/identity/claims/tenantid&quot;)
                        .Select(c => c.Value).SingleOrDefault())&quot;;
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Submit hello selected user/group toobe asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   <span data-ttu-id="d94a4-186">Všimněte si, že `token` a `tenant` jsou používány hello `AadPicker` toomake objekt volá Azure Active Directory Graph API.</span><span class="sxs-lookup"><span data-stu-id="d94a4-186">Note that `token` and `tenant` are used by hello `AadPicker` object toomake Azure Active Directory Graph API calls.</span></span> <span data-ttu-id="d94a4-187">Přidáte `AadPicker` později.</span><span class="sxs-lookup"><span data-stu-id="d94a4-187">You'll add `AadPicker` later.</span></span>     
   
   > [!NOTE]
   > <span data-ttu-id="d94a4-188">Stejně dobře získáte `token` a `tenant` z hello na straně klienta s `~/.auth/me`, ale, že by být volání další server.</span><span class="sxs-lookup"><span data-stu-id="d94a4-188">You can just as well get `token` and `tenant` from hello client side with `~/.auth/me`, but that would be an additional server call.</span></span> <span data-ttu-id="d94a4-189">Například:</span><span class="sxs-lookup"><span data-stu-id="d94a4-189">For example:</span></span>
   > 
   > <span data-ttu-id="d94a4-190">$.ajax ({datový typ: "json", adresa url: "/.auth/me", Úspěch: funkce (data) {var token = data [0] .access_token; var klienta = dat [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});</span><span class="sxs-lookup"><span data-stu-id="d94a4-190">$.ajax({ dataType: "json", url: "/.auth/me", success: function (data) { var token = data[0].access_token; var tenant = data[0].user_claims .find(c => c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val; } });</span></span>
   > 
   > 
7. <span data-ttu-id="d94a4-191">Provést stejné změny s hello ~ \Views\WorkItems\Edit.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d94a4-191">Make hello same changes with ~\Views\WorkItems\Edit.cshtml.</span></span>
8. <span data-ttu-id="d94a4-192">Hello `AadPicker` objektu je definována ve skriptu, je nutné, aby tooadd tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="d94a4-192">hello `AadPicker` object is defined in a script that you need tooadd tooyour project.</span></span> <span data-ttu-id="d94a4-193">Klikněte pravým tlačítkem na složku ~\Scripts hello, přejděte příliš**přidat**a klikněte na tlačítko **soubor JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-193">Right-click hello ~\Scripts folder, point too**Add**, and click **JavaScript file**.</span></span> <span data-ttu-id="d94a4-194">Typ `AadPickerLibrary` hello název souboru a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-194">Type `AadPickerLibrary` for hello filename and click **OK**.</span></span>
9. <span data-ttu-id="d94a4-195">Zkopírujte obsah hello z [sem](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) do ~ \Scripts\AadPickerLibrary.js.</span><span class="sxs-lookup"><span data-stu-id="d94a4-195">Copy hello content from [here](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) into ~\Scripts\AadPickerLibrary.js.</span></span>
   
   <span data-ttu-id="d94a4-196">Ve skriptu hello hello `AadPicker` objektu volání [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch pro uživatele a skupiny, které odpovídají vstupní hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-196">In hello script, hello `AadPicker` object calls [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch for users and groups that match hello input.</span></span>  
10. <span data-ttu-id="d94a4-197">~\Scripts\AadPickerLibrary.js také používá hello [pomůcky Autocomplete uživatelského rozhraní jQuery](https://jqueryui.com/autocomplete/).</span><span class="sxs-lookup"><span data-stu-id="d94a4-197">~\Scripts\AadPickerLibrary.js also uses hello [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/).</span></span> <span data-ttu-id="d94a4-198">Proto musíte tooadd jQuery UI tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="d94a4-198">So you need tooadd jQuery UI tooyour project.</span></span> <span data-ttu-id="d94a4-199">Klikněte pravým tlačítkem na projekt v a klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-199">Right-click your project in and click **Manage NuGet Packages**.</span></span>
11. <span data-ttu-id="d94a4-200">V hello Správce balíčků NuGet, klikněte na tlačítko Procházet, typ **uživatelského rozhraní jquery** v hello panelu vyhledávání a klikněte na tlačítko **jQuery.UI.Combined**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-200">In hello NuGet Package Manager, click Browse, type **jquery-ui** in hello search bar, and click **jQuery.UI.Combined**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. <span data-ttu-id="d94a4-201">V pravém podokně hello, klikněte na **nainstalovat**, pak klikněte na tlačítko **OK** tooproceed.</span><span class="sxs-lookup"><span data-stu-id="d94a4-201">In hello right pane, click **Install**, then click **OK** tooproceed.</span></span>
13. <span data-ttu-id="d94a4-202">Otevřete ~\App_Start\BundleConfig.cs a proveďte následující zvýrazněný změny hello:</span><span class="sxs-lookup"><span data-stu-id="d94a4-202">Open ~\App_Start\BundleConfig.cs and make hello following highlighted changes:</span></span>  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
        // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
        bundles.Add(new ScriptBundle(&quot;~/bundles/modernizr&quot;).Include(
                    &quot;~/Scripts/modernizr-*&quot;));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/bootstrap&quot;).Include(
                    &quot;~/Scripts/bootstrap.js&quot;,
                    &quot;~/Scripts/respond.js&quot;));
    
        bundles.Add(new StyleBundle(&quot;~/Content/css&quot;).Include(
                    &quot;~/Content/bootstrap.css&quot;,
                    &quot;~/Content/site.css&quot;<mark>,
                    &quot;~/Content/themes/base/jquery-ui.css&quot;</mark>));
    }
    </pre>
    
    <span data-ttu-id="d94a4-203">Existují další původce způsoby toomanage JavaScript a CSS souborů ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d94a4-203">There are more performant ways toomanage JavaScript and CSS files in your app.</span></span> <span data-ttu-id="d94a4-204">Ale pro jednoduchost právě budete toopiggyback na hello sady, načítaných s každé zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d94a4-204">However, for simplicity you're just going toopiggyback on hello bundles that are loaded with every view.</span></span>
14. <span data-ttu-id="d94a4-205">Nakonec v ~ \Global.asax, přidejte následující řádek kódu hello hello `Application_Start()` metoda.</span><span class="sxs-lookup"><span data-stu-id="d94a4-205">Finally, in ~\Global.asax, add hello following line of code in hello `Application_Start()` method.</span></span> <span data-ttu-id="d94a4-206">`Ctrl`+`.`na každém pojmenování chyby překladu názvů příliš opravte ji.</span><span class="sxs-lookup"><span data-stu-id="d94a4-206">`Ctrl`+`.` on each naming resolution error too fix it.</span></span>
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > <span data-ttu-id="d94a4-207">Je nutné tento řádek kódu, protože používá výchozí šablony MVC hello <code>[ValidateAntiForgeryToken]</code> decoration u některých akcí hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-207">You need this line of code because hello default MVC template uses <code>[ValidateAntiForgeryToken]</code> decoration on some of hello actions.</span></span> <span data-ttu-id="d94a4-208">Z důvodu toohello chování popsaného [Allen společnosti Brock](https://twitter.com/BrockLAllen) v [MVC 4, AntiForgeryToken a deklarace identity](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST podařit ověření tokenu proti zfalšování, protože:</span><span class="sxs-lookup"><span data-stu-id="d94a4-208">Due toohello behavior described by [Brock Allen](https://twitter.com/BrockLAllen) at [MVC 4, AntiForgeryToken and Claims](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) your HTTP POST may fail anti-forgery token validation because:</span></span>
    > 
    > * <span data-ttu-id="d94a4-209">Azure Active Directory neodešle hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, který je požadován ve výchozím nastavení token proti padělání hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-209">Azure Active Directory does not send hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, which is required by default by hello anti-forgery token.</span></span>
    > * <span data-ttu-id="d94a4-210">Pokud Azure Active Directory directory synchronizované se službou AD FS, důvěryhodnosti hello služby AD FS ve výchozím nastavení neodesílá hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider deklarace identity buď, i když můžete nakonfigurovat ručně toosend služby AD FS Tato deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="d94a4-210">If Azure Active Directory is directory synced with AD FS, hello AD FS trust by default does not send hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider claim either, although you can manually configure AD FS toosend this claim.</span></span>
    > 
    > <span data-ttu-id="d94a4-211">`ClaimTypes.NameIdentifies`Určuje hello deklarace `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, která poskytnout Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d94a4-211">`ClaimTypes.NameIdentifies` specifies hello claim `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, which Azure Active Directory does supply.</span></span>  
    > 
    > 
15. <span data-ttu-id="d94a4-212">Nyní publikujte změny.</span><span class="sxs-lookup"><span data-stu-id="d94a4-212">Now, publish your changes.</span></span> <span data-ttu-id="d94a4-213">Klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-213">Right-click your project and click **Publish**.</span></span>
16. <span data-ttu-id="d94a4-214">Klikněte na tlačítko **nastavení**, ověřte zda je k dispozici tooyour řetězce připojení SQL Database, vyberte **aktualizace databáze** toomake hello změny schématu pro váš model a klikněte na tlačítko **publikovat** .</span><span class="sxs-lookup"><span data-stu-id="d94a4-214">Click **Settings**, make sure there is a connection string tooyour SQL Database, select **Update Database** toomake hello schema changes for your model, and click **Publish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. <span data-ttu-id="d94a4-215">Přejděte v prohlížeči hello toohttps: / /&lt;*appname*>.azurewebsites.net/workitems a klikněte na tlačítko **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="d94a4-215">In hello browser, navigate toohttps://&lt;*appname*>.azurewebsites.net/workitems and click **Create New**.</span></span>
18. <span data-ttu-id="d94a4-216">Klikněte na tlačítko v hello **AssignedToName** pole.</span><span class="sxs-lookup"><span data-stu-id="d94a4-216">Click in hello **AssignedToName** box.</span></span> <span data-ttu-id="d94a4-217">Měli byste nyní vidět uživatele a skupiny z klienta služby Azure Active Directory v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="d94a4-217">You should now see users and groups from your Azure Active Directory tenant in a dropdown.</span></span> <span data-ttu-id="d94a4-218">Můžete zadat toofilter nebo použít hello `Up` nebo `Down` klíče nebo klikněte na tlačítko tooselect hello uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="d94a4-218">You can type toofilter, or use hello `Up` or `Down` key or click tooselect hello user or group.</span></span> 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. <span data-ttu-id="d94a4-219">Klikněte na tlačítko **vytvořit** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="d94a4-219">Click **Create** toosave hello changes.</span></span> <span data-ttu-id="d94a4-220">Potom klikněte na **upravit** na hello vytvoření pracovní položky tooobserve hello stejné chování.</span><span class="sxs-lookup"><span data-stu-id="d94a4-220">Then, click **Edit** on hello created work item tooobserve hello same behavior.</span></span>

<span data-ttu-id="d94a4-221">Congrats nyní je spuštěn-obchodní aplikace v Azure s přístupem k adresáři!</span><span class="sxs-lookup"><span data-stu-id="d94a4-221">Congrats, you are now running a line-of-business app in Azure with directory access!</span></span> <span data-ttu-id="d94a4-222">Je mnohem víc, že můžete provést hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="d94a4-222">There's a lot more you can do with hello Graph API.</span></span> <span data-ttu-id="d94a4-223">V tématu [referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span><span class="sxs-lookup"><span data-stu-id="d94a4-223">See [Azure AD Graph API reference](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span></span>

<a name="next"></a>

## <a name="next-step"></a><span data-ttu-id="d94a4-224">Další krok</span><span class="sxs-lookup"><span data-stu-id="d94a4-224">Next Step</span></span>
<span data-ttu-id="d94a4-225">Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v azure, najdete v části [WebApp. RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) pro ukázku od týmu Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="d94a4-225">If you need role-based access control (RBAC) for your line-of-business app in azure, see [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) for a sample from hello Azure Active Directory team.</span></span> <span data-ttu-id="d94a4-226">Se dozvíte, jak tooenable role pro vaši aplikaci Azure Active Directory a potom budete autorizovat uživatele s hello `[Authorize]` decoration.</span><span class="sxs-lookup"><span data-stu-id="d94a4-226">It shows you how tooenable roles for your Azure Active Directory application, and then authorize users with hello `[Authorize]` decoration.</span></span>

<span data-ttu-id="d94a4-227">Pokud-obchodní aplikace potřebuje přístup k datům místní tooon, přečtěte si téma [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d94a4-227">If your line-of-business app needs access tooon-premises data, see [Access on-premises resources using hybrid connections in Azure App Service](web-sites-hybrid-connection-get-started.md).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="d94a4-228">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="d94a4-228">Further resources</span></span>
* [<span data-ttu-id="d94a4-229">Ověřování a autorizace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d94a4-229">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)
* [<span data-ttu-id="d94a4-230">Ověření pomocí místní služby Active Directory v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="d94a4-230">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="d94a4-231">Vytvoření-obchodní aplikace v Azure pomocí ověřování služby AD FS</span><span class="sxs-lookup"><span data-stu-id="d94a4-231">Create a line-of-business app in Azure with AD FS authentication</span></span>](web-sites-dotnet-lob-application-adfs.md)
* [<span data-ttu-id="d94a4-232">Aplikace služby ověřování a hello Azure AD Graph API</span><span class="sxs-lookup"><span data-stu-id="d94a4-232">App Service Auth and hello Azure AD Graph API</span></span>](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [<span data-ttu-id="d94a4-233">Microsoft Azure Active Directory ukázky a dokumentace</span><span class="sxs-lookup"><span data-stu-id="d94a4-233">Microsoft Azure Active Directory Samples and Documentation</span></span>](https://github.com/AzureADSamples)
* [<span data-ttu-id="d94a4-234">Azure Active Directory podporované Token a typy deklarací identity</span><span class="sxs-lookup"><span data-stu-id="d94a4-234">Azure Active Directory Supported Token and Claim Types</span></span>](http://msdn.microsoft.com/library/azure/dn195587.aspx)
