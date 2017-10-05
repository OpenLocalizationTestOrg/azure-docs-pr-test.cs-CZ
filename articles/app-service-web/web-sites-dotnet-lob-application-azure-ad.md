---
title: "Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak vytvořit aplikaci ASP.NET MVC – obchodní v Azure App Service ověřování s Azure Active Directory"
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
ms.openlocfilehash: 6eadf0a521a32c5bc580908e4e4b7f4305e2bf7e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a><span data-ttu-id="106bc-103">Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="106bc-103">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>
<span data-ttu-id="106bc-104">V tomto článku se dozvíte, jak vytvořit-obchodní aplikace .NET v [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí [ověřování / autorizace](../app-service/app-service-authentication-overview.md) funkce.</span><span class="sxs-lookup"><span data-stu-id="106bc-104">This article shows you how to create a .NET line-of-business app in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) using the [Authentication / Authorization](../app-service/app-service-authentication-overview.md) feature.</span></span> <span data-ttu-id="106bc-105">Také ukazuje, jak používat [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) do adresáře dotaz na data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="106bc-105">It also shows how to use the [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) to query directory data in the application.</span></span>

<span data-ttu-id="106bc-106">Klienta Azure Active Directory, který používáte, může být adresář jen Azure.</span><span class="sxs-lookup"><span data-stu-id="106bc-106">The Azure Active Directory tenant that you use can be an Azure-only directory.</span></span> <span data-ttu-id="106bc-107">Nebo může být [synchronizaci se službou Active Directory v místě](../active-directory/active-directory-aadconnect.md) k vytvoření jedné prostředí přihlášení pro pracovní procesy, které jsou místní a vzdálené.</span><span class="sxs-lookup"><span data-stu-id="106bc-107">Or, it can be [synced with your on-premises Active Directory](../active-directory/active-directory-aadconnect.md) to create a single sign-on experience for workers that are on-premises and remote.</span></span> <span data-ttu-id="106bc-108">Tento článek používá výchozí adresář pro váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="106bc-108">This article uses the default directory for your Azure account.</span></span>

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="106bc-109">Co se sestavení</span><span class="sxs-lookup"><span data-stu-id="106bc-109">What you will build</span></span>
<span data-ttu-id="106bc-110">Bude vytvořit jednoduchou aplikaci vytvořit-čtení-aktualizace-odstranění (CRUD)-obchodní v App Service Web Apps, že sleduje pracovní položky pomocí následujících funkcí:</span><span class="sxs-lookup"><span data-stu-id="106bc-110">You will build a simple line-of-business Create-Read-Update-Delete (CRUD) application in App Service Web Apps that tracks work items with the following features:</span></span>

* <span data-ttu-id="106bc-111">Ověřuje uživatele na základě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="106bc-111">Authenticates users against Azure Active Directory</span></span>
* <span data-ttu-id="106bc-112">Dotazuje adresáře uživatelů a skupin pomocí [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span><span class="sxs-lookup"><span data-stu-id="106bc-112">Queries directory users and groups using [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span></span>
* <span data-ttu-id="106bc-113">Pomocí rozhraní ASP.NET MVC *bez ověřování* šablony</span><span class="sxs-lookup"><span data-stu-id="106bc-113">Use the ASP.NET MVC *No Authentication* template</span></span>

<span data-ttu-id="106bc-114">Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v Azure, najdete v části [další krok](#next).</span><span class="sxs-lookup"><span data-stu-id="106bc-114">If you need role-based access control (RBAC) for your line-of-business app in Azure, see [Next Step](#next).</span></span>

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="106bc-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="106bc-115">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="106bc-116">Budete potřebovat k dokončení tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="106bc-116">You need the following to complete this tutorial:</span></span>

* <span data-ttu-id="106bc-117">Klient služby Azure Active Directory s uživateli v různých skupinách</span><span class="sxs-lookup"><span data-stu-id="106bc-117">An Azure Active Directory tenant with users in various groups</span></span>
* <span data-ttu-id="106bc-118">Oprávnění k vytvoření aplikace na klienta Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="106bc-118">Permissions to create applications on the Azure Active Directory tenant</span></span>
* <span data-ttu-id="106bc-119">Visual Studio 2013 Update 4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="106bc-119">Visual Studio 2013 Update 4 or later</span></span>
* [<span data-ttu-id="106bc-120">Azure SDK 2.8.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="106bc-120">Azure SDK 2.8.1 or later</span></span>](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-to-azure"></a><span data-ttu-id="106bc-121">Vytvoření a nasazení webové aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="106bc-121">Create and deploy a web app to Azure</span></span>
1. <span data-ttu-id="106bc-122">Ze sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="106bc-122">From Visual Studio, click **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="106bc-123">Vyberte **webové aplikace ASP.NET**, pojmenujte svůj projekt a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="106bc-123">Select **ASP.NET Web Application**, name your project, and click **OK**.</span></span>
3. <span data-ttu-id="106bc-124">Vyberte **MVC** šablony, pak změňte ověření na **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="106bc-124">Select the **MVC** template, then change the authentication to **No Authentication**.</span></span> <span data-ttu-id="106bc-125">Zajistěte, aby **hostitel v cloudu** je vybrána a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="106bc-125">Make sure **Host in the Cloud** is selected and click **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. <span data-ttu-id="106bc-126">V **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **přidat účet** (a potom **přidat účet** v rozevírací nabídce) pro přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="106bc-126">In the **Create App Service** dialog, click **Add an account** (and then **Add an account** in the dropdown) to log in to your Azure account.</span></span>
5. <span data-ttu-id="106bc-127">Po přihlášení nakonfigurujte webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="106bc-127">Once logged in configure your web app.</span></span> <span data-ttu-id="106bc-128">Kliknutím příslušné vytvořit skupinu prostředků a nový plán aplikační služby **nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="106bc-128">Create a resource group and a new App Service plan by clicking the respective **New** button.</span></span> <span data-ttu-id="106bc-129">Klikněte na tlačítko **Objevte další služby Azure** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="106bc-129">Click **Explore additional Azure services** to continue.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. <span data-ttu-id="106bc-130">V **služby** , klikněte na  **+**  přidání databáze SQL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="106bc-130">In the **Services** tab, click **+** to add a SQL Database for your app.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. <span data-ttu-id="106bc-131">V **nakonfigurovat databázi SQL**, klikněte na tlačítko **nový** k vytvoření instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="106bc-131">In **Configure SQL Database**, click **New** to create a SQL Server instance.</span></span>
8. <span data-ttu-id="106bc-132">V **nakonfigurujte systém SQL Server**, nakonfigurovat instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="106bc-132">In **Configure SQL Server**, configure your SQL Server instance.</span></span> <span data-ttu-id="106bc-133">Potom klikněte na **OK**, **OK**, a **vytvořit** k ji vytváření aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="106bc-133">Then, click **OK**, **OK**, and **Create** to kick off the app creation in Azure.</span></span>
9. <span data-ttu-id="106bc-134">V **aktivita služby Azure App Service**, se zobrazí po dokončení vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="106bc-134">In **Azure App Service Activity**, you can see when the app creation is finished.</span></span> <span data-ttu-id="106bc-135">Klikněte na tlačítko  **publikovat &lt;* appname*> a této webové aplikace teď **, potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="106bc-135">Click **Publish &lt;*appname*> to this Web App now**, then click **Publish**.</span></span> 
   
    <span data-ttu-id="106bc-136">Po dokončení sady Visual Studio otevře publikování aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="106bc-136">Once Visual Studio finishes, it opens the publish app in the browser.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a><span data-ttu-id="106bc-137">Konfigurace ověřování a directory přístupu</span><span class="sxs-lookup"><span data-stu-id="106bc-137">Configure authentication and directory access</span></span>
1. <span data-ttu-id="106bc-138">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="106bc-138">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="106bc-139">V levé nabídce klikněte na tlačítko **App Services** > **&lt;*appname*> ** > **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="106bc-139">From the left menu, click **App Services** > **&lt;*appname*>** > **Authentication / Authorization**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. <span data-ttu-id="106bc-140">Zapnout ověřování Azure Active Directory kliknutím **na** > **Azure Active Directory** > **Express** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="106bc-140">Turn on Azure Active Directory authentication by clicking **On** > **Azure Active Directory** > **Express** > **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. <span data-ttu-id="106bc-141">Klikněte na tlačítko **Uložit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="106bc-141">Click **Save** in the command bar.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    <span data-ttu-id="106bc-142">Po nastavení ověřování se úspěšně uložily, zkuste přejdete do vaší aplikace znovu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="106bc-142">Once the authentication settings are saved successfully, try navigating to your app again in the browser.</span></span> <span data-ttu-id="106bc-143">Výchozí nastavení vynucení ověřování na celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="106bc-143">Your default settings enforce authentication on the whole app.</span></span> <span data-ttu-id="106bc-144">Pokud už nejste přihlášeni, budete přesměrováni na přihlašovací obrazovku.</span><span class="sxs-lookup"><span data-stu-id="106bc-144">If you aren't already logged in, you are redirected to a login screen.</span></span> <span data-ttu-id="106bc-145">Po přihlášení, by se zobrazit aplikaci zabezpečené pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="106bc-145">Once logged in, you see your app secured by HTTPS.</span></span> <span data-ttu-id="106bc-146">Dále musíte povolit přístup k datům adresáře.</span><span class="sxs-lookup"><span data-stu-id="106bc-146">Next, you need to enable access to directory data.</span></span> 
5. <span data-ttu-id="106bc-147">Přejděte na [portálu classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="106bc-147">Navigate to the [classic portal](https://manage.windowsazure.com).</span></span>
6. <span data-ttu-id="106bc-148">V levé nabídce klikněte na tlačítko **služby Active Directory** > **výchozí adresář** > **aplikace** > **&lt;*appname*> **.</span><span class="sxs-lookup"><span data-stu-id="106bc-148">From the left menu, click **Active Directory** > **Default Directory** > **Applications** > **&lt;*appname*>**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    <span data-ttu-id="106bc-149">Toto je aplikace Azure Active Directory, která vytvoří služby App Service Povolit autorizaci nebo funkce ověřování.</span><span class="sxs-lookup"><span data-stu-id="106bc-149">This is the Azure Active Directory application that App Service created for you to enable the Authorization / Authentication feature.</span></span>
7. <span data-ttu-id="106bc-150">Klikněte na tlačítko **uživatelé** a **skupiny** a ujistěte se, že máte některé uživatele a skupiny v adresáři.</span><span class="sxs-lookup"><span data-stu-id="106bc-150">Click **Users** and **Groups** to make sure that you have some users and groups in the directory.</span></span> <span data-ttu-id="106bc-151">Pokud ne, vytvořte několik testovací uživatele a skupiny.</span><span class="sxs-lookup"><span data-stu-id="106bc-151">If not, create a few test users and groups.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. <span data-ttu-id="106bc-152">Klikněte na tlačítko **konfigurace** ke konfiguraci této aplikace.</span><span class="sxs-lookup"><span data-stu-id="106bc-152">Click **Configure** to configure this application.</span></span>
9. <span data-ttu-id="106bc-153">Přejděte dolů k položce **klíče** a přidejte klíč výběrem dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="106bc-153">Scroll down to the **Keys** section and add a key by selecting a duration.</span></span> <span data-ttu-id="106bc-154">Potom klikněte na **delegovaná oprávnění** a vyberte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="106bc-154">Then, click **Delegated Permissions** and select **Read directory data**.</span></span> 
   <span data-ttu-id="106bc-155">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="106bc-155">Click **Save**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. <span data-ttu-id="106bc-156">Jakmile vaše nastavení se ukládají, přejděte zpět na **klíče** části a klikněte na tlačítko **kopie** tlačítko zkopírujte klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="106bc-156">Once your settings are saved, scroll back up to the **Keys** section and click the **Copy** button to copy the client key.</span></span> 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="106bc-157">Pokud jste tuto stránku opustit nyní, nebudete mít přístup k tento klíč klienta někdy znovu.</span><span class="sxs-lookup"><span data-stu-id="106bc-157">If you navigate away from this page now, you won't be able to access this client key ever again.</span></span>
    > 
    > 
11. <span data-ttu-id="106bc-158">Dále musíte nakonfigurovat webové aplikace s tímto klíčem.</span><span class="sxs-lookup"><span data-stu-id="106bc-158">Next, you need to configure your web app with this key.</span></span> <span data-ttu-id="106bc-159">Přihlaste se k [Průzkumníka prostředků Azure](https://resources.azure.com) s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="106bc-159">Log in to the [Azure Resource Explorer](https://resources.azure.com) with your Azure account.</span></span>
12. <span data-ttu-id="106bc-160">V horní části stránky klikněte na tlačítko **pro čtení a zápis** provést změny v Průzkumníku prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="106bc-160">At the top of the page, click **Read/Write** to make changes in the Azure Resource Explorer.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. <span data-ttu-id="106bc-161">Najít nastavení ověřování pro aplikace umístěné v odběry >  **&lt;* název_předplatného*> ** > **Skupinyprostředků** > **&lt;*resourcegroupname*> ** > **zprostředkovatelé** > **Microsoft.Web** > **lokality** > **&lt;*appname*> ** > **konfigurace** > **authsettings**.</span><span class="sxs-lookup"><span data-stu-id="106bc-161">Find the authentication settings for your app, located at subscriptions > **&lt;*subscriptionname*>** > **resourceGroups** > **&lt;*resourcegroupname*>** > **providers** > **Microsoft.Web** > **sites** > **&lt;*appname*>** > **config** > **authsettings**.</span></span>
14. <span data-ttu-id="106bc-162">Klikněte na **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="106bc-162">Click **Edit**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. <span data-ttu-id="106bc-163">V podokně úpravy nastavit `clientSecret` a `additionalLoginParams` vlastnosti následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="106bc-163">In the editing pane, set the `clientSecret` and `additionalLoginParams` properties as follows.</span></span>
    
        ...
        "clientSecret": "<client key from the Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. <span data-ttu-id="106bc-164">Klikněte na tlačítko **Put** v horní části k odeslání změn.</span><span class="sxs-lookup"><span data-stu-id="106bc-164">Click **Put** at the top to submit your changes.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. <span data-ttu-id="106bc-165">Nyní Pokud chcete otestovat, pokud máte autorizační token pro přístup k Azure Active Directory Graph API, jednoduše přejděte k  **https://&lt;*appname*>.azurewebsites.net/.auth/me** v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="106bc-165">Now, to test if you have the authorization token to access the Azure Active Directory Graph API, just navigate to **https://&lt;*appname*>.azurewebsites.net/.auth/me** in your browser.</span></span> <span data-ttu-id="106bc-166">Pokud jste nakonfigurovali všechno správně, měli byste vidět `access_token` vlastnost v odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="106bc-166">If you configured everything correctly, you should see the `access_token` property in the JSON response.</span></span>
    
    <span data-ttu-id="106bc-167">`~/.auth/me` Cestu adresy URL spravuje aplikace služby ověřování / autorizace tak, abyste získali všechny informace související s ověřená relace.</span><span class="sxs-lookup"><span data-stu-id="106bc-167">The `~/.auth/me` URL path is managed by App Service Authentication / Authorization to give you all the information related to your authenticated session.</span></span> <span data-ttu-id="106bc-168">Další informace najdete v tématu [ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="106bc-168">For more information, see [Authentication and authorization in Azure App Service](../app-service/app-service-authentication-overview.md).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="106bc-169">`access_token` Má dobu platnosti.</span><span class="sxs-lookup"><span data-stu-id="106bc-169">The `access_token` has an expiration period.</span></span> <span data-ttu-id="106bc-170">Ale aplikace služby ověřování / autorizace poskytuje funkce tokenu aktualizace s `~/.auth/refresh`.</span><span class="sxs-lookup"><span data-stu-id="106bc-170">However, App Service Authentication / Authorization provides token refresh functionality with `~/.auth/refresh`.</span></span> <span data-ttu-id="106bc-171">Další informace o tom, jak ho použít, najdete v části [obchod Token služby](https://cgillum.tech/2016/03/07/app-service-token-store/).</span><span class="sxs-lookup"><span data-stu-id="106bc-171">For more information on how to use it, see [App Service Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).</span></span>
    > 
    > 

<span data-ttu-id="106bc-172">V dalším kroku provedete něco užitečný v případě dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="106bc-172">Next, you will do something useful with directory data.</span></span>

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-to-your-app"></a><span data-ttu-id="106bc-173">Přidat další funkce – obchodní aplikace</span><span class="sxs-lookup"><span data-stu-id="106bc-173">Add line-of-business functionality to your app</span></span>
<span data-ttu-id="106bc-174">Nyní můžete vytvořit jednoduché sledovací modul CRUD položky pracovní.</span><span class="sxs-lookup"><span data-stu-id="106bc-174">Now, you create a simple CRUD work items tracker.</span></span>  

1. <span data-ttu-id="106bc-175">Ve složce ~\Models, vytvořte soubor třídy s názvem WorkItem.cs a nahraďte `public class WorkItem {...}` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="106bc-175">In the ~\Models folder, create a class file called WorkItem.cs, and replace `public class WorkItem {...}` with the following code:</span></span>
   
     <span data-ttu-id="106bc-176">pomocí System.ComponentModel.DataAnnotations;</span><span class="sxs-lookup"><span data-stu-id="106bc-176">using System.ComponentModel.DataAnnotations;</span></span>
   
     <span data-ttu-id="106bc-177">Veřejná třída {pracovní položky</span><span class="sxs-lookup"><span data-stu-id="106bc-177">public class WorkItem   {</span></span>
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     <span data-ttu-id="106bc-178">}</span><span class="sxs-lookup"><span data-stu-id="106bc-178">}</span></span>
   
     <span data-ttu-id="106bc-179">Veřejný výčet WorkItemStatus {</span><span class="sxs-lookup"><span data-stu-id="106bc-179">public enum WorkItemStatus   {</span></span>
   
         Open,
         Investigating,
         Resolved,
         Closed
     <span data-ttu-id="106bc-180">}</span><span class="sxs-lookup"><span data-stu-id="106bc-180">}</span></span>
2. <span data-ttu-id="106bc-181">Sestavte projekt a zpřístupněte nový model pro logiku generování uživatelského rozhraní v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="106bc-181">Build the project to make your new model accessible to the scaffolding logic in Visual Studio.</span></span>
3. <span data-ttu-id="106bc-182">Přidat novou položku vygenerované `WorkItemsController` ke složce ~\Controllers (klikněte pravým tlačítkem na **řadiče**, přejděte na příkaz **přidat**a vyberte **nové vygenerované položky**).</span><span class="sxs-lookup"><span data-stu-id="106bc-182">Add a new scaffolded item `WorkItemsController` to the ~\Controllers folder (right-click **Controllers**, point to **Add**, and select **New scaffolded item**).</span></span> 
4. <span data-ttu-id="106bc-183">Vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="106bc-183">Select **MVC 5 Controller with views, using Entity Framework** and click **Add**.</span></span>
5. <span data-ttu-id="106bc-184">Vyberte model, který jste vytvořili a pak klikněte na tlačítko  **+**  a potom **přidat** přidat data kontextu, a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="106bc-184">Select the model that you created, then click **+** and then **Add** to add a data context, and then click **Add**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. <span data-ttu-id="106bc-185">V ~\Views\WorkItems\Create.cshtml (automaticky vygenerované položky), najdete `Html.BeginForm` Pomocná metoda a proveďte následující zvýrazněný změny:</span><span class="sxs-lookup"><span data-stu-id="106bc-185">In ~\Views\WorkItems\Create.cshtml (an automatically scaffolded item), find the `Html.BeginForm` helper method and make the following highlighted changes:</span></span>  
   
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
    @Html.ActionLink(&quot;Back to List&quot;, &quot;Index&quot;)
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
   
        // Submit the selected user/group to be asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   <span data-ttu-id="106bc-186">Všimněte si, že `token` a `tenant` jsou používány `AadPicker` objekt volání Azure Active Directory Graph API.</span><span class="sxs-lookup"><span data-stu-id="106bc-186">Note that `token` and `tenant` are used by the `AadPicker` object to make Azure Active Directory Graph API calls.</span></span> <span data-ttu-id="106bc-187">Přidáte `AadPicker` později.</span><span class="sxs-lookup"><span data-stu-id="106bc-187">You'll add `AadPicker` later.</span></span>     
   
   > [!NOTE]
   > <span data-ttu-id="106bc-188">Stejně dobře získáte `token` a `tenant` ze strany klienta s `~/.auth/me`, ale, že by být volání další server.</span><span class="sxs-lookup"><span data-stu-id="106bc-188">You can just as well get `token` and `tenant` from the client side with `~/.auth/me`, but that would be an additional server call.</span></span> <span data-ttu-id="106bc-189">Například:</span><span class="sxs-lookup"><span data-stu-id="106bc-189">For example:</span></span>
   > 
   > <span data-ttu-id="106bc-190">$.ajax ({datový typ: "json", adresa url: "/.auth/me", Úspěch: funkce (data) {var token = data [0] .access_token; var klienta = dat [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});</span><span class="sxs-lookup"><span data-stu-id="106bc-190">$.ajax({ dataType: "json", url: "/.auth/me", success: function (data) { var token = data[0].access_token; var tenant = data[0].user_claims .find(c => c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val; } });</span></span>
   > 
   > 
7. <span data-ttu-id="106bc-191">Provést stejné změny s ~ \Views\WorkItems\Edit.cshtml.</span><span class="sxs-lookup"><span data-stu-id="106bc-191">Make the same changes with ~\Views\WorkItems\Edit.cshtml.</span></span>
8. <span data-ttu-id="106bc-192">`AadPicker` Objektu je definována v skript, který je nutné přidat do projektu.</span><span class="sxs-lookup"><span data-stu-id="106bc-192">The `AadPicker` object is defined in a script that you need to add to your project.</span></span> <span data-ttu-id="106bc-193">Klikněte pravým tlačítkem na složku ~\Scripts, přejděte na **přidat**a klikněte na tlačítko **soubor JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="106bc-193">Right-click the ~\Scripts folder, point to **Add**, and click **JavaScript file**.</span></span> <span data-ttu-id="106bc-194">Typ `AadPickerLibrary` pro název souboru a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="106bc-194">Type `AadPickerLibrary` for the filename and click **OK**.</span></span>
9. <span data-ttu-id="106bc-195">Kopírovat obsah z [sem](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) do ~ \Scripts\AadPickerLibrary.js.</span><span class="sxs-lookup"><span data-stu-id="106bc-195">Copy the content from [here](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) into ~\Scripts\AadPickerLibrary.js.</span></span>
   
   <span data-ttu-id="106bc-196">Ve skriptu `AadPicker` objektu volání [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) pro vyhledání uživatelů a skupin, které odpovídají vstupní.</span><span class="sxs-lookup"><span data-stu-id="106bc-196">In the script, the `AadPicker` object calls [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) to search for users and groups that match the input.</span></span>  
10. <span data-ttu-id="106bc-197">~\Scripts\AadPickerLibrary.js používá také [pomůcky Autocomplete uživatelského rozhraní jQuery](https://jqueryui.com/autocomplete/).</span><span class="sxs-lookup"><span data-stu-id="106bc-197">~\Scripts\AadPickerLibrary.js also uses the [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/).</span></span> <span data-ttu-id="106bc-198">Proto je nutné přidat do projektu jQuery uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="106bc-198">So you need to add jQuery UI to your project.</span></span> <span data-ttu-id="106bc-199">Klikněte pravým tlačítkem na projekt v a klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="106bc-199">Right-click your project in and click **Manage NuGet Packages**.</span></span>
11. <span data-ttu-id="106bc-200">V Správce balíčků NuGet, klikněte na tlačítko Procházet, typ **uživatelského rozhraní jquery** v panelu vyhledávání a klikněte na **jQuery.UI.Combined**.</span><span class="sxs-lookup"><span data-stu-id="106bc-200">In the NuGet Package Manager, click Browse, type **jquery-ui** in the search bar, and click **jQuery.UI.Combined**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. <span data-ttu-id="106bc-201">V pravém podokně klikněte na **nainstalovat**, pak klikněte na tlačítko **OK** pokračovat.</span><span class="sxs-lookup"><span data-stu-id="106bc-201">In the right pane, click **Install**, then click **OK** to proceed.</span></span>
13. <span data-ttu-id="106bc-202">Otevřete ~\App_Start\BundleConfig.cs a proveďte následující zvýrazněný změny:</span><span class="sxs-lookup"><span data-stu-id="106bc-202">Open ~\App_Start\BundleConfig.cs and make the following highlighted changes:</span></span>  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
        // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
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
    
    <span data-ttu-id="106bc-203">Existuje více způsobů původce ke správě souborů JavaScript a CSS ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="106bc-203">There are more performant ways to manage JavaScript and CSS files in your app.</span></span> <span data-ttu-id="106bc-204">Ale pro jednoduchost právě budete počítač na sady načítaných s každé zobrazení.</span><span class="sxs-lookup"><span data-stu-id="106bc-204">However, for simplicity you're just going to piggyback on the bundles that are loaded with every view.</span></span>
14. <span data-ttu-id="106bc-205">Nakonec v ~ \Global.asax, přidejte následující řádek kódu `Application_Start()` metoda.</span><span class="sxs-lookup"><span data-stu-id="106bc-205">Finally, in ~\Global.asax, add the following line of code in the `Application_Start()` method.</span></span> <span data-ttu-id="106bc-206">`Ctrl`+`.`na každém pojmenování chyby překladu názvů a opravte ji.</span><span class="sxs-lookup"><span data-stu-id="106bc-206">`Ctrl`+`.` on each naming resolution error to fix it.</span></span>
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > <span data-ttu-id="106bc-207">Je nutné tento řádek kódu, protože používá výchozí šablony MVC <code>[ValidateAntiForgeryToken]</code> decoration na některé akce.</span><span class="sxs-lookup"><span data-stu-id="106bc-207">You need this line of code because the default MVC template uses <code>[ValidateAntiForgeryToken]</code> decoration on some of the actions.</span></span> <span data-ttu-id="106bc-208">Z důvodu chování popsaného [Allen společnosti Brock](https://twitter.com/BrockLAllen) v [MVC 4, AntiForgeryToken a deklarace identity](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST podařit ověření tokenu proti zfalšování, protože:</span><span class="sxs-lookup"><span data-stu-id="106bc-208">Due to the behavior described by [Brock Allen](https://twitter.com/BrockLAllen) at [MVC 4, AntiForgeryToken and Claims](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) your HTTP POST may fail anti-forgery token validation because:</span></span>
    > 
    > * <span data-ttu-id="106bc-209">Azure Active Directory neodešle http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, který je požadován ve výchozím nastavení token proti padělání.</span><span class="sxs-lookup"><span data-stu-id="106bc-209">Azure Active Directory does not send the http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, which is required by default by the anti-forgery token.</span></span>
    > * <span data-ttu-id="106bc-210">Pokud Azure Active Directory directory synchronizované se službou AD FS, vztah důvěryhodnosti služby AD FS ve výchozím nastavení neodesílá http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider deklarace identity buď, i když můžete ručně nakonfigurovat na odesílání této deklarace identity služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="106bc-210">If Azure Active Directory is directory synced with AD FS, the AD FS trust by default does not send the http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider claim either, although you can manually configure AD FS to send this claim.</span></span>
    > 
    > <span data-ttu-id="106bc-211">`ClaimTypes.NameIdentifies`Určuje deklarace `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, která poskytnout Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="106bc-211">`ClaimTypes.NameIdentifies` specifies the claim `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, which Azure Active Directory does supply.</span></span>  
    > 
    > 
15. <span data-ttu-id="106bc-212">Nyní publikujte změny.</span><span class="sxs-lookup"><span data-stu-id="106bc-212">Now, publish your changes.</span></span> <span data-ttu-id="106bc-213">Klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="106bc-213">Right-click your project and click **Publish**.</span></span>
16. <span data-ttu-id="106bc-214">Klikněte na tlačítko **nastavení**, ujistěte se, že je připojovací řetězec k vaší databázi SQL, vyberte **aktualizace databáze** provést změny schématu pro váš model a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="106bc-214">Click **Settings**, make sure there is a connection string to your SQL Database, select **Update Database** to make the schema changes for your model, and click **Publish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. <span data-ttu-id="106bc-215">V prohlížeči přejděte na https://&lt;*appname*>.azurewebsites.net/workitems a klikněte na tlačítko **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="106bc-215">In the browser, navigate to https://&lt;*appname*>.azurewebsites.net/workitems and click **Create New**.</span></span>
18. <span data-ttu-id="106bc-216">Kliknutím na tlačítko ve **AssignedToName** pole.</span><span class="sxs-lookup"><span data-stu-id="106bc-216">Click in the **AssignedToName** box.</span></span> <span data-ttu-id="106bc-217">Měli byste nyní vidět uživatele a skupiny z klienta služby Azure Active Directory v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="106bc-217">You should now see users and groups from your Azure Active Directory tenant in a dropdown.</span></span> <span data-ttu-id="106bc-218">Můžete zadat pro filtrování, nebo pomocí `Up` nebo `Down` klíče nebo klikněte na tlačítko Vybrat uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="106bc-218">You can type to filter, or use the `Up` or `Down` key or click to select the user or group.</span></span> 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. <span data-ttu-id="106bc-219">Klikněte na tlačítko **vytvořit** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="106bc-219">Click **Create** to save the changes.</span></span> <span data-ttu-id="106bc-220">Potom klikněte na **upravit** stejné chování zachovávají vytvořený pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="106bc-220">Then, click **Edit** on the created work item to observe the same behavior.</span></span>

<span data-ttu-id="106bc-221">Congrats nyní je spuštěn-obchodní aplikace v Azure s přístupem k adresáři!</span><span class="sxs-lookup"><span data-stu-id="106bc-221">Congrats, you are now running a line-of-business app in Azure with directory access!</span></span> <span data-ttu-id="106bc-222">Je mnohem víc, že můžete provést pomocí rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="106bc-222">There's a lot more you can do with the Graph API.</span></span> <span data-ttu-id="106bc-223">V tématu [referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span><span class="sxs-lookup"><span data-stu-id="106bc-223">See [Azure AD Graph API reference](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span></span>

<a name="next"></a>

## <a name="next-step"></a><span data-ttu-id="106bc-224">Další krok</span><span class="sxs-lookup"><span data-stu-id="106bc-224">Next Step</span></span>
<span data-ttu-id="106bc-225">Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v azure, najdete v části [WebApp. RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) pro ukázku od týmu Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="106bc-225">If you need role-based access control (RBAC) for your line-of-business app in azure, see [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) for a sample from the Azure Active Directory team.</span></span> <span data-ttu-id="106bc-226">Ukazuje, jak povolit role pro vaše aplikace Azure Active Directory, a potom budete autorizovat uživatele s `[Authorize]` decoration.</span><span class="sxs-lookup"><span data-stu-id="106bc-226">It shows you how to enable roles for your Azure Active Directory application, and then authorize users with the `[Authorize]` decoration.</span></span>

<span data-ttu-id="106bc-227">Pokud vaše-obchodní aplikace potřebuje přístup k místním datům, přečtěte si téma [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="106bc-227">If your line-of-business app needs access to on-premises data, see [Access on-premises resources using hybrid connections in Azure App Service](web-sites-hybrid-connection-get-started.md).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="106bc-228">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="106bc-228">Further resources</span></span>
* [<span data-ttu-id="106bc-229">Ověřování a autorizace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="106bc-229">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)
* [<span data-ttu-id="106bc-230">Ověření pomocí místní služby Active Directory v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="106bc-230">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="106bc-231">Vytvoření-obchodní aplikace v Azure pomocí ověřování služby AD FS</span><span class="sxs-lookup"><span data-stu-id="106bc-231">Create a line-of-business app in Azure with AD FS authentication</span></span>](web-sites-dotnet-lob-application-adfs.md)
* [<span data-ttu-id="106bc-232">Ověřování služby App Service a Azure AD Graph API</span><span class="sxs-lookup"><span data-stu-id="106bc-232">App Service Auth and the Azure AD Graph API</span></span>](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [<span data-ttu-id="106bc-233">Microsoft Azure Active Directory ukázky a dokumentace</span><span class="sxs-lookup"><span data-stu-id="106bc-233">Microsoft Azure Active Directory Samples and Documentation</span></span>](https://github.com/AzureADSamples)
* [<span data-ttu-id="106bc-234">Azure Active Directory podporované Token a typy deklarací identity</span><span class="sxs-lookup"><span data-stu-id="106bc-234">Azure Active Directory Supported Token and Claim Types</span></span>](http://msdn.microsoft.com/library/azure/dn195587.aspx)
