---
title: "Ověřování uživatelů pro aplikace API v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak chránit aplikace API v Azure App Service, povolí přístup pouze ověřené uživatele."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: a5b713f3a60b3886d813438496d704f464d914e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a><span data-ttu-id="7dce9-103">Ověřování uživatelů pro aplikace API v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7dce9-103">User authentication for API Apps in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="7dce9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7dce9-104">Overview</span></span>
<span data-ttu-id="7dce9-105">Tento článek ukazuje, jak do aplikace Azure API jsou chráněny, aby ji volat pouze ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="7dce9-105">This article shows how to protect an Azure API app so that only authenticated users can call it.</span></span> <span data-ttu-id="7dce9-106">Článek předpokládá, že jste četli [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7dce9-106">The article assumes that you have read the [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span>

<span data-ttu-id="7dce9-107">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="7dce9-107">You'll learn:</span></span>

* <span data-ttu-id="7dce9-108">Postup konfigurace zprostředkovatele ověřování, s podrobnostmi o službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7dce9-108">How to configure an authentication provider, with details for Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="7dce9-109">Jak používat chráněné aplikace API přes [Active Directory Authentication Library (ADAL) pro JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).</span><span class="sxs-lookup"><span data-stu-id="7dce9-109">How to consume a protected API app by using the [Active Directory Authentication Library (ADAL) for JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).</span></span>

<span data-ttu-id="7dce9-110">Článek obsahuje dvě části:</span><span class="sxs-lookup"><span data-stu-id="7dce9-110">The article contains two sections:</span></span>

* <span data-ttu-id="7dce9-111">[Postup konfigurace ověřování uživatelů ve službě Azure App Service](#authconfig) část popisuje obecně konfiguraci ověřování uživatelů pro libovolnou aplikaci API a vztahuje stejnou měrou na všech rozhraní podporovaných službou App Service, včetně .NET, Node.js a Java.</span><span class="sxs-lookup"><span data-stu-id="7dce9-111">The [How to configure user authentication in Azure App Service](#authconfig) section explains in general how to configure user authentication for any API app and applies equally to all frameworks supported by App Service, including .NET, Node.js, and Java.</span></span>
* <span data-ttu-id="7dce9-112">Od verze [pokračováním kurzů k .NET API Apps](#tutorialstart) části článek vás provede konfiguraci ukázkové aplikace s .NET back-end a AngularJS front-end tak, aby používala Azure Active Directory pro ověřování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7dce9-112">Starting with the [Continuing the .NET API Apps tutorials](#tutorialstart) section, the article guides you through configuring a sample application with a .NET back end and an AngularJS front end so that it uses Azure Active Directory for user authentication.</span></span> 

## <span data-ttu-id="7dce9-113"><a id="authconfig"></a>Postup konfigurace ověřování uživatelů ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7dce9-113"><a id="authconfig"></a> How to configure user authentication in Azure App Service</span></span>
<span data-ttu-id="7dce9-114">Tato část obsahuje obecné pokyny, které platí pro všechny aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-114">This section provides general instructions that apply to any API app.</span></span> <span data-ttu-id="7dce9-115">Konkrétní kroky pro ukázkovou aplikaci udělat rozhraní seznamu .NET přejděte na [pokračování úvodních kurzů k .NET](#tutorialstart).</span><span class="sxs-lookup"><span data-stu-id="7dce9-115">For steps specific to the To Do List .NET sample application, go to [Continuing the .NET getting-started tutorials](#tutorialstart).</span></span>

1. <span data-ttu-id="7dce9-116">V [portál Azure](https://portal.azure.com/), přejděte na **nastavení** okno aplikace API, který chcete chránit, vyhledejte **funkce** části a potom klikněte na **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-116">In the [Azure portal](https://portal.azure.com/), navigate to the **Settings** blade of the API app that you want to protect, find the **Features** section, and then click **Authentication/ Authorization**.</span></span>
   
    ![Portál Azure ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. <span data-ttu-id="7dce9-118">V **ověřování / autorizace** okně klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-118">In the **Authentication / Authorization** blade, click **On**.</span></span>
3. <span data-ttu-id="7dce9-119">Vyberte jednu z možností z **akci provést, když požadavek nebude ověřený** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="7dce9-119">Select one of the options from the **Action to take when request is not authenticated** drop-down list.</span></span>
   
   * <span data-ttu-id="7dce9-120">Pokud chcete pouze ověřené volání k dosažení aplikace API, vyberte jednu z **protokolu s...**  možnosti.</span><span class="sxs-lookup"><span data-stu-id="7dce9-120">If you want only authenticated calls to reach your API app, choose one of the **Log in with ...** options.</span></span> <span data-ttu-id="7dce9-121">Tato možnost umožňuje chránit aplikace API bez nutnosti napsat kód, který běží v ní.</span><span class="sxs-lookup"><span data-stu-id="7dce9-121">This option enables you to protect the API app without writing any code that runs in it.</span></span>
   * <span data-ttu-id="7dce9-122">Pokud chcete, aby všechna volání k dosažení aplikace API, zvolte **povolit požadavku (žádná akce)**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-122">If you want all calls to reach your API app, choose **Allow request (no action)**.</span></span> <span data-ttu-id="7dce9-123">Tuto možnost můžete použít k přímé neověřené volající k vybrané zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="7dce9-123">You can use this option to direct unauthenticated callers to a choice of authentication providers.</span></span> <span data-ttu-id="7dce9-124">Pomocí této možnosti budete muset napsat kód pro autorizaci.</span><span class="sxs-lookup"><span data-stu-id="7dce9-124">With this option you have to write code to handle authorization.</span></span>
     
     <span data-ttu-id="7dce9-125">Další informace najdete v tématu [Ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md#multiple-protection-options).</span><span class="sxs-lookup"><span data-stu-id="7dce9-125">For more information, see [Authentication and authorization for API Apps in Azure App Service](app-service-api-authentication.md#multiple-protection-options).</span></span>
4. <span data-ttu-id="7dce9-126">Vyberte jeden nebo více **zprostředkovatele ověřování**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-126">Select one or more of the **Authentication Providers**.</span></span>
   
    <span data-ttu-id="7dce9-127">Obrázek ukazuje možnosti, které vyžadují všechny volající ověřovány službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-127">The image shows    choices that require all callers to be authenticated by Azure AD.</span></span>
   
    ![Okno Azure portálu ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    <span data-ttu-id="7dce9-129">Když zvolíte zprostředkovatele ověřování, na portálu zobrazí okno konfigurace pro tohoto zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="7dce9-129">When you choose an authentication provider, the portal displays a configuration blade for that provider.</span></span> 
   
    <span data-ttu-id="7dce9-130">Podrobné pokyny, které vysvětlují, jak nakonfigurovat oken konfigurace zprostředkovatele ověřování najdete v tématu [postup konfigurace aplikace služby App Service pomocí přihlášení Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7dce9-130">For detailed instructions that explain how to configure the authentication provider configuration blades, see [How to configure your App Service application to use Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span> <span data-ttu-id="7dce9-131">(Tento odkaz je odkazem na článek o službě Azure AD, ale samotný článek obsahuje karty, které odkazují na články pro ostatní zprostředkovatelé ověřování.)</span><span class="sxs-lookup"><span data-stu-id="7dce9-131">(The link goes to an article about Azure AD, but the article itself contains tabs that link to articles for the other authentication providers.)</span></span>
5. <span data-ttu-id="7dce9-132">Když jste hotovi s okno Konfigurace zprostředkovatele ověřování, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-132">When you're done with the authentication provider configuration blade, click **OK**.</span></span>
6. <span data-ttu-id="7dce9-133">V **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-133">In the **Authentication / Authorization** blade, click **Save**.</span></span>

<span data-ttu-id="7dce9-134">Když to uděláte, ověřuje služby App Service všechna volání rozhraní API, než dosáhnou aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-134">When this is done, App Service authenticates all API calls before they reach the API app.</span></span> <span data-ttu-id="7dce9-135">Ověřovací služby fungovat stejně pro všechny jazyky, které podporuje služby App service, včetně .NET, Node.js a Java.</span><span class="sxs-lookup"><span data-stu-id="7dce9-135">The authentication services work the same for all languages that App service supports, including .NET, Node.js, and Java.</span></span> 

<span data-ttu-id="7dce9-136">Chcete-li ověřených volání rozhraní API, volající zahrnuje zprostředkovatele ověřování tokenu nosiče OAuth 2.0 v hlavičce autorizace požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="7dce9-136">To make authenticated API calls, the caller includes the authentication provider's OAuth 2.0 bearer token in the Authorization header of HTTP requests.</span></span> <span data-ttu-id="7dce9-137">Pomocí sady SDK pro zprostředkovatele ověřování, může získat token.</span><span class="sxs-lookup"><span data-stu-id="7dce9-137">The token can be acquired by using the authentication provider's SDK.</span></span>

## <span data-ttu-id="7dce9-138"><a id="tutorialstart"></a>Pokračováním kurzů k .NET API Apps</span><span class="sxs-lookup"><span data-stu-id="7dce9-138"><a id="tutorialstart"></a> Continuing the .NET API Apps tutorials</span></span>
<span data-ttu-id="7dce9-139">Pokud postupujete podle Node.js nebo Java kurzy pro aplikace API, přejděte k další článek, [objekt zabezpečení ověřování služby pro aplikace API](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="7dce9-139">If you are following the Node.js or Java tutorials for API apps, skip to the next article, [service principal authentication for API apps](app-service-api-dotnet-service-principal-auth.md).</span></span> 

<span data-ttu-id="7dce9-140">Pokud postupujete podle řady kurz .NET pro aplikace API a už jste nasadili ukázkovou aplikaci podle pokynů v [první](app-service-api-dotnet-get-started.md) a [druhý](app-service-api-cors-consume-javascript.md) kurzy, pokračujte [nastavení ověřování v App Service a Azure AD](#azureauth) části.</span><span class="sxs-lookup"><span data-stu-id="7dce9-140">If you are following the .NET tutorial series for API apps and have already deployed the sample application as directed in the [first](app-service-api-dotnet-get-started.md) and [second](app-service-api-cors-consume-javascript.md) tutorials, skip to the [Set up authentication in App Service and Azure AD](#azureauth) section.</span></span>

<span data-ttu-id="7dce9-141">Pokud chcete v tomto kurzu bez průchodu přes kurzy první a druhé, proveďte následující kroky, které popisují, jak začít pracovat s použitím automatizovaného procesu nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dce9-141">If you want to follow this tutorial without going through the first and second tutorials, do the following steps which explain how to get started by using an automated process to deploy the sample application.</span></span>

> [!NOTE]
> <span data-ttu-id="7dce9-142">Následující kroky vám pomůžou na stejné počáteční bod jako, pokud jste to absolvovali první dva kurzy, s jednou výjimkou: Visual Studio nebude již víte, které webové aplikace nebo aplikace API, který získá každý projekt nasazen.</span><span class="sxs-lookup"><span data-stu-id="7dce9-142">The following steps get you to the same starting point as if you did the first two tutorials, with one exception: Visual Studio won't already know which web app or API app that each project gets deployed to.</span></span> <span data-ttu-id="7dce9-143">To znamená, že nebudete mít přesné pokyny v tomto kurzu, které vysvětlují, jak nasadit do správné cíle.</span><span class="sxs-lookup"><span data-stu-id="7dce9-143">That means you won't have exact instructions in this tutorial that explain how to deploy to the right targets.</span></span> <span data-ttu-id="7dce9-144">Pokud si nejste tedy se nad tím, jak to provést kroky nasazení sami, je lepší postupujte podle kurzu řady z [první kurz](app-service-api-dotnet-get-started.md) než začínat to automatizované procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="7dce9-144">If you're not comfortable with figuring out how to do the deployment steps on your own, it's better to follow the tutorial series from the [first tutorial](app-service-api-dotnet-get-started.md) than to start with this automated deployment process.</span></span>
> 
> 

1. <span data-ttu-id="7dce9-145">Ujistěte se, že máte všechny požadavky uvedené v [první kurz](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7dce9-145">Make sure that you have all of the prerequisites listed in the [first tutorial](app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="7dce9-146">Kromě předpoklady uvedené tato kurzech ověřování předpokládají, jste už pracovali s webové aplikace aplikační služby a aplikace API v sadě Visual Studio a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7dce9-146">In addition to the prerequisites listed, these authentication tutorials assume that you have worked with App Service web apps and API apps in Visual Studio and the Azure portal.</span></span>
2. <span data-ttu-id="7dce9-147">Klikněte na tlačítko **nasadit do Azure** v tlačítko [soubor readme úložiště seznam úkolů ukázkové](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) k nasazení aplikace API a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dce9-147">Click the **Deploy to Azure** button in the [To Do List sample repository readme file](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) to deploy the API apps and the web app.</span></span> <span data-ttu-id="7dce9-148">Skupina prostředků Azure, která je vytvořena, jak to můžete později k vyhledání webové aplikace a API aplikace názvy si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="7dce9-148">Make a note of the Azure resource group that gets created, as you can use this later to look up web app and API app names.</span></span>
3. <span data-ttu-id="7dce9-149">Stáhnout nebo klonovat [seznam úkolů ukázkové úložiště](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) získat kód, který budete pracujete s místně v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7dce9-149">Download or clone the [To Do List sample repository](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) to get the code that you'll work with locally in Visual Studio.</span></span>

## <span data-ttu-id="7dce9-150"><a id="azureauth"></a>Nastavení ověřování v App Service a Azure AD</span><span class="sxs-lookup"><span data-stu-id="7dce9-150"><a id="azureauth"></a> Set up authentication in App Service and Azure AD</span></span>
<span data-ttu-id="7dce9-151">Nyní máte aplikace běžící v Azure App Service bez nutnosti ověřit uživatele.</span><span class="sxs-lookup"><span data-stu-id="7dce9-151">You now have the application running in Azure App Service without requiring that users be authenticated.</span></span> <span data-ttu-id="7dce9-152">V této části přidáte ověřování provedením následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="7dce9-152">In this section you add authentication by doing the following tasks:</span></span>

* <span data-ttu-id="7dce9-153">Nakonfigurujte aplikační služby a chcete vyžadovat ověřování Azure Active Directory (Azure AD) pro volání metody aplikace API střední vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7dce9-153">Configure App Service to require Azure Active Directory (Azure AD) authentication for calling the middle tier API app.</span></span>
* <span data-ttu-id="7dce9-154">Vytvořte aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-154">Create an Azure AD application.</span></span>
* <span data-ttu-id="7dce9-155">Nakonfigurujte aplikace Azure AD k odeslání token nosiče po přihlášení do AngularJS front-endu.</span><span class="sxs-lookup"><span data-stu-id="7dce9-155">Configure the Azure AD application to send the bearer token after logon to the AngularJS front end.</span></span> 

<span data-ttu-id="7dce9-156">Pokud narazíte na problémy při následujícím kurzu pokynů, najdete v článku [Poradce při potížích s](#troubleshooting) části na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7dce9-156">If you run into problems while following the tutorial directions, see the [Troubleshooting](#troubleshooting) section at the end of the tutorial.</span></span> 

### <a name="configure-authentication-for-the-middle-tier-api-app"></a><span data-ttu-id="7dce9-157">Konfigurovat ověřování pro aplikaci API střední vrstvy</span><span class="sxs-lookup"><span data-stu-id="7dce9-157">Configure authentication for the middle tier API app</span></span>
1. <span data-ttu-id="7dce9-158">V [portál Azure](https://portal.azure.com/), přejděte na **nastavení** okno aplikace API, které jste vytvořili pro projekt ToDoListAPI najít **funkce** části a potom klikněte na **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-158">In the [Azure portal](https://portal.azure.com/), navigate to the **Settings** blade of the API app that you created for the ToDoListAPI project, find the **Features** section, and then click **Authentication / Authorization**.</span></span>
   
    ![Portál Azure ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. <span data-ttu-id="7dce9-160">V **ověřování / autorizace** okně klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-160">In the **Authentication / Authorization** blade, click **On**.</span></span>
3. <span data-ttu-id="7dce9-161">V **akci provést, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-161">In the **Action to take when request is not authenticated** drop-down list, select **Log in with Azure Active Directory**.</span></span>
   
    <span data-ttu-id="7dce9-162">Tato možnost zajistí, že žádné žádosti o unauathenticated dosáhne aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-162">This option ensures that no unauathenticated requests will reach the API app.</span></span> 
4. <span data-ttu-id="7dce9-163">V části **zprostředkovatele ověřování**, klikněte na tlačítko **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-163">Under **Authentication Providers**, click **Azure Active Directory**.</span></span>
   
    ![Okno Azure portálu ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. <span data-ttu-id="7dce9-165">V **nastavení Azure Active Directory** okně klikněte na tlačítko **Express**</span><span class="sxs-lookup"><span data-stu-id="7dce9-165">In the **Azure Active Directory Settings** blade, click **Express**</span></span>
   
    ![Možnost Azure Express ověřování/autorizace služby portálu](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    <span data-ttu-id="7dce9-167">Pomocí **Express** možnosti služby App Service můžete automaticky vytvořit aplikaci Azure AD ve službě Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant).</span><span class="sxs-lookup"><span data-stu-id="7dce9-167">With the **Express** option, App Service can automatically create an Azure AD application in your Azure AD [tenant](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant).</span></span> 
   
    <span data-ttu-id="7dce9-168">Nemusíte vytvořit klienta, protože každý účet Azure automaticky jeden má.</span><span class="sxs-lookup"><span data-stu-id="7dce9-168">You don't have to create a tenant, because every Azure account automatically has one.</span></span>
6. <span data-ttu-id="7dce9-169">V části **režim správy**, klikněte na tlačítko **vytvořit novou aplikaci AD** Pokud již není vybrána a Všimněte si, hodnotu, která je v **vytvořit aplikaci** textového pole; budete vyhledání této AAD aplikace na portálu Azure classic později.</span><span class="sxs-lookup"><span data-stu-id="7dce9-169">Under **Management mode**, click **Create New AD App** if it isn't already selected, and notice the value that is in the **Create App** text box; you'll look up this AAD application in the Azure classic portal later.</span></span>
   
    ![Nastavení Azure portálu Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    <span data-ttu-id="7dce9-171">Azure automaticky vytvoří aplikaci Azure AD s názvem uvedené v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-171">Azure will automatically create an Azure AD application with the indicated name in your Azure AD tenant.</span></span> <span data-ttu-id="7dce9-172">Ve výchozím nastavení, aplikace Azure AD se stejným názvem jako aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-172">By default, the Azure AD application is named the same as the API app.</span></span> <span data-ttu-id="7dce9-173">Pokud dáváte přednost, můžete zadat jiný název.</span><span class="sxs-lookup"><span data-stu-id="7dce9-173">If you prefer, you can enter a different name.</span></span>
7. <span data-ttu-id="7dce9-174">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-174">Click **OK**.</span></span>
8. <span data-ttu-id="7dce9-175">V **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-175">In the **Authentication / Authorization** blade, click **Save**.</span></span>
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

<span data-ttu-id="7dce9-177">Nyní jenom uživatelé v klientovi služby Azure AD můžete volat aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-177">Now only users in your Azure AD tenant can call the API app.</span></span>

### <a name="optional-test-the-api-app"></a><span data-ttu-id="7dce9-178">Volitelné: Testovací aplikace API</span><span class="sxs-lookup"><span data-stu-id="7dce9-178">Optional: Test the API app</span></span>
1. <span data-ttu-id="7dce9-179">V prohlížeči přejděte na adresu URL aplikace API: v **aplikace API** okno na portálu Azure klikněte na odkaz v části **URL**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-179">In a browser go to the URL of the API app: in the **API app** blade in the Azure portal, click the link under **URL**.</span></span>  
   
    <span data-ttu-id="7dce9-180">Budete přesměrováni na přihlašovací obrazovku, protože neověřené požadavky nejsou povoleny dosáhne aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-180">You are redirected to a login screen because unauthenticated requests are not allowed to reach the API app.</span></span>
   
    <span data-ttu-id="7dce9-181">Pokud prohlížeč přejde na stránku "byla úspěšně vytvořena.", prohlížeče mohou již být zaznamenány na – v takovém případě otevřete okno InPrivate nebo Incognito, přejděte na adresu URL aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-181">If your browser goes to the "successfully created" page, the browser might already be logged on -- in that case, open an InPrivate or Incognito window and go to the API app's URL.</span></span>
2. <span data-ttu-id="7dce9-182">Přihlaste se pomocí přihlašovacích údajů pro uživatele v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-182">Log on using credentials for a user in your Azure AD tenant.</span></span>
   
    <span data-ttu-id="7dce9-183">Pokud jste přihlášeni, "úspěšně vytvořil" stránka se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7dce9-183">When you're logged on, the "successfully created" page appears in the browser.</span></span>
3. <span data-ttu-id="7dce9-184">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="7dce9-184">Close the browser.</span></span>

### <a name="configure-the-azure-ad-application"></a><span data-ttu-id="7dce9-185">Konfigurace aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="7dce9-185">Configure the Azure AD application</span></span>
<span data-ttu-id="7dce9-186">Když jste nakonfigurovali ověřování Azure AD, služby App Service vytvoří aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-186">When you configured Azure AD authentication, App Service created an Azure AD application for you.</span></span> <span data-ttu-id="7dce9-187">Ve výchozím nastavení nové služby Azure AD aplikace byla nakonfigurována k poskytování token nosiče adresu URL aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-187">By default the new Azure AD application was configured to provide the bearer token to the API app's URL.</span></span> <span data-ttu-id="7dce9-188">V této části můžete nakonfigurovat aplikace Azure AD zajistit token nosiče, který má AngularJS front-endu místo přímo do aplikace API střední vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7dce9-188">In this section you configure the Azure AD application to provide the bearer token to the AngularJS front end instead of directly to the middle tier API app.</span></span> <span data-ttu-id="7dce9-189">Front-endu AngularJS odešle token do aplikace API, když volá aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-189">The AngularJS front end will send the token to the API app when it calls the API app.</span></span>

> [!NOTE]
> <span data-ttu-id="7dce9-190">Aplikace pro front-endu a středu zachovat proces jako jednoduchý nejblíže, tento kurz používá jednu Azure AD vrstvy aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-190">To keep the process as simple as possible, this tutorial uses a single Azure AD application for both the front end and the middle tier API app.</span></span> <span data-ttu-id="7dce9-191">Další možností je použití dvou aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-191">Another option is to use two Azure AD applications.</span></span> <span data-ttu-id="7dce9-192">V takovém případě je třeba udělit oprávnění aplikace Azure AD části front end volat aplikaci Azure AD střední vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7dce9-192">In that case you would have to give the front end's Azure AD application permission to call the middle tier's Azure AD application.</span></span> <span data-ttu-id="7dce9-193">Tento přístup více aplikacemi, získáte podrobnější kontrolu nad oprávnění pro každou úroveň.</span><span class="sxs-lookup"><span data-stu-id="7dce9-193">This multi-application approach would give you more granular control over permissions to each tier.</span></span>
> 
> 

1. <span data-ttu-id="7dce9-194">V [portál Azure classic](https://manage.windowsazure.com/), přejděte na **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-194">In the [Azure classic portal](https://manage.windowsazure.com/), go to **Azure Active Directory**.</span></span>
   
   <span data-ttu-id="7dce9-195">Budete muset použít klasický portál, protože některá nastavení Azure Active Directory, kteří potřebují přístup k ještě nejsou k dispozici v aktuální portál Azure.</span><span class="sxs-lookup"><span data-stu-id="7dce9-195">You have to use the classic portal because certain Azure Active Directory settings that you need access to are not yet available in the current Azure portal.</span></span>
2. <span data-ttu-id="7dce9-196">Na **Directory** , klikněte na vašem tenantovi AAD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-196">On the **Directory** tab, click your AAD tenant.</span></span>
   
   ![Azure AD na portálu classic](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. <span data-ttu-id="7dce9-198">Klikněte na tlačítko **aplikace > Moje společnost vlastní aplikace**a klikněte na symbol zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="7dce9-198">Click **Applications > Applications my company owns**, and then click the check mark.</span></span>
   
   <span data-ttu-id="7dce9-199">Budete také muset aktualizací stránky zobrazíte nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dce9-199">You might also have to refresh the page to see the new application.</span></span>
4. <span data-ttu-id="7dce9-200">V seznamu aplikací klikněte na název ten, který vytvořil Azure za vás, pokud jste povolili ověřování pro vaše aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-200">In the list of applications, click the name of the one that Azure created for you when you enabled authentication for your API app.</span></span>
   
   ![Karta Azure AD aplikace](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. <span data-ttu-id="7dce9-202">Klikněte na **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-202">Click **Configure**.</span></span>
   
   ![Azure AD konfigurovat karta](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. <span data-ttu-id="7dce9-204">Nastavit **přihlašovací adresa URL** na adresu URL pro webové aplikace AngularJS, žádné koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="7dce9-204">Set **Sign-on URL** to the URL for your AngularJS web app, no trailing slash.</span></span>
   
   <span data-ttu-id="7dce9-205">Příklad: https://todolistangular.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="7dce9-205">For example: https://todolistangular.azurewebsites.net</span></span>
   
   ![Adresa URL přihlašování](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. <span data-ttu-id="7dce9-207">Nastavit **adresa URL odpovědi** na adresu URL pro webovou aplikaci, žádné koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="7dce9-207">Set **Reply URL** to the URL for your web app, no trailing slash.</span></span>
   
   <span data-ttu-id="7dce9-208">Příklad: https://todolistsangular.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="7dce9-208">For example: https://todolistsangular.azurewebsites.net</span></span>
8. <span data-ttu-id="7dce9-209">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-209">Click **Save**.</span></span>
   
   ![Adresa URL odpovědi](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. <span data-ttu-id="7dce9-211">V dolní části stránky klikněte na tlačítko **spravovat manifest > stáhnout manifest**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-211">At the bottom of the page, click **Manage manifest > Download manifest**.</span></span>
   
   ![Stáhněte manifest](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. <span data-ttu-id="7dce9-213">Stáhněte soubor do umístění, kde můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="7dce9-213">Download the file to a location where you can edit it.</span></span>
11. <span data-ttu-id="7dce9-214">Vyhledejte v souboru manifestu stažený `oauth2AllowImplicitFlow` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7dce9-214">In the downloaded manifest file, search for the  `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="7dce9-215">Změna hodnoty této vlastnosti z `false` k `true`a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="7dce9-215">Change the value of this property from `false` to `true`, and then save the file.</span></span>
    
    <span data-ttu-id="7dce9-216">Toto nastavení je povinné pro přístup z jednostránkové aplikace JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7dce9-216">This setting is required for access from a JavaScript single-page application.</span></span> <span data-ttu-id="7dce9-217">Umožňuje tokenu nosiče Oauth 2.0, který se má vrátit v fragment adresy URL.</span><span class="sxs-lookup"><span data-stu-id="7dce9-217">It enables the Oauth 2.0 bearer token to be returned in the URL fragment.</span></span>
12. <span data-ttu-id="7dce9-218">Klikněte na tlačítko **spravovat manifest > nahrávání manifest**a odešlete soubor, který jste aktualizovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="7dce9-218">Click **Manage manifest > Upload manifest**, and upload the file that you updated in the preceding step.</span></span>
    
    ![Nahrát manifestu](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. <span data-ttu-id="7dce9-220">Kopírování **ID klienta** hodnotu a uložte ho někde můžete získat z později.</span><span class="sxs-lookup"><span data-stu-id="7dce9-220">Copy the **Client ID** value and save it somewhere you can get it from later.</span></span>

## <a name="configure-the-todolistangular-project-to-use-authentication"></a><span data-ttu-id="7dce9-221">Konfigurace projektu ToDoListAngular používat ověřování</span><span class="sxs-lookup"><span data-stu-id="7dce9-221">Configure the ToDoListAngular project to use authentication</span></span>
<span data-ttu-id="7dce9-222">V této části můžete změnit AngularJS front-endu, tak, aby používala Active Directory Authentication Library (ADAL) pro JS získat token nosiče pro přihlášeného uživatele z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-222">In this section you change the AngularJS front end so that it uses Active Directory Authentication Library (ADAL) for JS to acquire a bearer token for the logged-on user from Azure AD.</span></span> <span data-ttu-id="7dce9-223">Kód bude obsahovat token v požadavky HTTP odeslané na střední vrstvy, jak je znázorněno v následujícím diagramu.</span><span class="sxs-lookup"><span data-stu-id="7dce9-223">The code will include the token in HTTP requests sent to the middle tier, as shown in the following diagram.</span></span> 

![Diagram ověřování uživatele](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

<span data-ttu-id="7dce9-225">Proveďte následující změny do souborů projektu ToDoListAngular.</span><span class="sxs-lookup"><span data-stu-id="7dce9-225">Make the following changes to files in the ToDoListAngular project.</span></span>

1. <span data-ttu-id="7dce9-226">Otevřete *index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="7dce9-226">Open the *index.cshtml* file.</span></span>
2. <span data-ttu-id="7dce9-227">Zrušením komentáře u řádků, které odkazují Active Directory Authentication Library (ADAL) pro skripty JS.</span><span class="sxs-lookup"><span data-stu-id="7dce9-227">Uncomment the lines that reference the Active Directory Authentication Library (ADAL) for JS scripts.</span></span>
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. <span data-ttu-id="7dce9-228">Otevřete *app/scripts/app.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="7dce9-228">Open the *app/scripts/app.js* file.</span></span>
4. <span data-ttu-id="7dce9-229">Komentář blok kódu označen pro "ověřování bez" a zrušte komentář u blok kódu označen pro "ověřování s".</span><span class="sxs-lookup"><span data-stu-id="7dce9-229">Comment out the block of code marked for "without authentication" and uncomment the block of code marked for "with authentication".</span></span>
   
    <span data-ttu-id="7dce9-230">Tato změna odkazuje zprostředkovatele ověřování ADAL JS a poskytuje hodnoty konfigurace do ní.</span><span class="sxs-lookup"><span data-stu-id="7dce9-230">This change references the ADAL JS authentication provider and provides configuration values to it.</span></span> <span data-ttu-id="7dce9-231">V následujícím postupu nastavíte hodnoty konfigurace pro vaše aplikace API a aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-231">In the following steps you set the configuration values for your API app and Azure AD application.</span></span>
5. <span data-ttu-id="7dce9-232">V kódu, který nastaví `endpoints` proměnnou, nastavte adresu URL rozhraní API na adresu URL aplikace API, že jste vytvořili pro projekt ToDoListAPI a nastavit ID aplikace Azure AD na ID klienta, který jste zkopírovali z portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="7dce9-232">In the code that sets the `endpoints` variable, set the API URL to the URL of the API app that you created for the ToDoListAPI project, and set the Azure AD application ID to the client ID that you copied from the Azure classic portal.</span></span>
   
    <span data-ttu-id="7dce9-233">Podobně jako v následujícím příkladu je kód.</span><span class="sxs-lookup"><span data-stu-id="7dce9-233">The code is now similar to the following example.</span></span>
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. <span data-ttu-id="7dce9-234">Ve volání `adalProvider.init`, nastavte `tenant` na název vašeho klienta a `clientId` stejnou hodnotu, kterou jste použili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="7dce9-234">In the call to `adalProvider.init`, set `tenant` to your tenant name and `clientId` to same value you used in the previous step.</span></span>
   
    <span data-ttu-id="7dce9-235">Podobně jako v následujícím příkladu je kód.</span><span class="sxs-lookup"><span data-stu-id="7dce9-235">The code is now similar to the following example.</span></span>
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    <span data-ttu-id="7dce9-236">Tyto změny `app.js` zadejte, zda kód volání a názvem rozhraní API jsou ve stejné aplikaci, Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-236">These changes to `app.js` specify that the calling code and the called API are in the same Azure AD application.</span></span>
7. <span data-ttu-id="7dce9-237">Otevřete *app/scripts/homeCtrl.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="7dce9-237">Open the *app/scripts/homeCtrl.js* file.</span></span>
8. <span data-ttu-id="7dce9-238">Komentář blok kódu označen pro "ověřování bez" a zrušte komentář u blok kódu označen pro "ověřování s".</span><span class="sxs-lookup"><span data-stu-id="7dce9-238">Comment out the block of code marked for "without authentication" and uncomment the block of code marked for "with authentication".</span></span>
9. <span data-ttu-id="7dce9-239">Otevřete *app/scripts/indexCtrl.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="7dce9-239">Open the *app/scripts/indexCtrl.js* file.</span></span>
10. <span data-ttu-id="7dce9-240">Komentář blok kódu označen pro "ověřování bez" a zrušte komentář u blok kódu označen pro "ověřování s".</span><span class="sxs-lookup"><span data-stu-id="7dce9-240">Comment out the block of code marked for "without authentication" and uncomment the block of code marked for "with authentication".</span></span>

### <a name="deploy-the-todolistangular-project-to-azure"></a><span data-ttu-id="7dce9-241">Nasazení projektu ToDoListAngular do Azure</span><span class="sxs-lookup"><span data-stu-id="7dce9-241">Deploy the ToDoListAngular project to Azure</span></span>
1. <span data-ttu-id="7dce9-242">V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt ToDoListAngular a potom klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-242">In **Solution Explorer**, right-click the ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="7dce9-243">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-243">Click **Publish**.</span></span>
   
    <span data-ttu-id="7dce9-244">Visual Studio nasadí projekt a otevře prohlížeč na základní adresu URL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dce9-244">Visual Studio deploys the project and opens a browser to the web app's base URL.</span></span> <span data-ttu-id="7dce9-245">Zobrazí 403 chybovou stránku, což je normální pro pokus o přechod z prohlížeče do webového rozhraní API základní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7dce9-245">This will show a 403 error page, which is normal for an attempt to go to a Web API base URL from a browser.</span></span>
   
    <span data-ttu-id="7dce9-246">Máte k provedení změny do aplikace API střední vrstvy, musíte před testováním aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dce9-246">You still have to make a change to the middle tier API app before you can test the application.</span></span>
3. <span data-ttu-id="7dce9-247">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="7dce9-247">Close the browser.</span></span>

## <a name="configure-the-todolistapi-project-to-use-authentication"></a><span data-ttu-id="7dce9-248">Konfigurace projektu ToDoListAPI používat ověřování</span><span class="sxs-lookup"><span data-stu-id="7dce9-248">Configure the ToDoListAPI project to use authentication</span></span>
<span data-ttu-id="7dce9-249">Aktuálně projekt ToDoListAPI odešle "*" jako `owner` hodnotu ToDoListDataAPI.</span><span class="sxs-lookup"><span data-stu-id="7dce9-249">Currently the ToDoListAPI project sends "*" as the `owner` value to ToDoListDataAPI.</span></span> <span data-ttu-id="7dce9-250">V této části můžete změnit kód odeslat ID přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7dce9-250">In this section you change the code to send the ID of the logged-on user.</span></span>

<span data-ttu-id="7dce9-251">Proveďte následující změny v projektu ToDoListAPI.</span><span class="sxs-lookup"><span data-stu-id="7dce9-251">Make the following changes in the ToDoListAPI project.</span></span>

1. <span data-ttu-id="7dce9-252">Otevřete *Controllers/ToDoListController.cs* souboru a zrušte komentář u řádku v každé metody akce, která nastaví `owner` do služby Azure AD `NameIdentifier` hodnoty deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="7dce9-252">Open the *Controllers/ToDoListController.cs* file, and uncomment the line in each action method that sets `owner` to the Azure AD `NameIdentifier` claim value.</span></span> <span data-ttu-id="7dce9-253">Například:</span><span class="sxs-lookup"><span data-stu-id="7dce9-253">For example:</span></span>
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    <span data-ttu-id="7dce9-254">**Důležité**: nemáte zrušte komentář u kód `ToDoListDataAPI` metoda; můžete to udělat později kurzu ověření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="7dce9-254">**Important**: Don't uncomment code in the `ToDoListDataAPI` method; you'll do that later for the service principal authentication tutorial.</span></span>

### <a name="deploy-the-todolistapi-project-to-azure"></a><span data-ttu-id="7dce9-255">Nasazení projektu ToDoListAPI do Azure</span><span class="sxs-lookup"><span data-stu-id="7dce9-255">Deploy the ToDoListAPI project to Azure</span></span>
1. <span data-ttu-id="7dce9-256">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListAPI a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-256">In **Solution Explorer**, right-click the ToDoListAPI project, and then click **Publish**.</span></span>
2. <span data-ttu-id="7dce9-257">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-257">Click **Publish**.</span></span>
   
    <span data-ttu-id="7dce9-258">Visual Studio nasadí projekt a otevře prohlížeč na základní adresu URL aplikace API.</span><span class="sxs-lookup"><span data-stu-id="7dce9-258">Visual Studio deploys the project and opens a browser to the API app's base URL.</span></span>
3. <span data-ttu-id="7dce9-259">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="7dce9-259">Close the browser.</span></span>

### <a name="test-the-application"></a><span data-ttu-id="7dce9-260">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="7dce9-260">Test the application</span></span>
1. <span data-ttu-id="7dce9-261">Přejděte na adresu URL webové aplikace, **pomocí protokolu HTTPS, není HTTP**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-261">Go to the URL of the web app, **using HTTPS, not HTTP**.</span></span>
2. <span data-ttu-id="7dce9-262">Klikněte **seznam úkolů** kartě.</span><span class="sxs-lookup"><span data-stu-id="7dce9-262">Click the **To Do List** tab.</span></span>
   
    <span data-ttu-id="7dce9-263">Zobrazí se výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7dce9-263">You are prompted to log in.</span></span>
3. <span data-ttu-id="7dce9-264">Přihlaste se pomocí přihlašovacích údajů uživatele ve vašem tenantovi AAD.</span><span class="sxs-lookup"><span data-stu-id="7dce9-264">Log in with the credentials of a user in your AAD tenant.</span></span>
4. <span data-ttu-id="7dce9-265">**Seznam úkolů** se zobrazí stránka.</span><span class="sxs-lookup"><span data-stu-id="7dce9-265">The **To Do List** page appears.</span></span>
   
   ![Seznam úkolů stránky](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   <span data-ttu-id="7dce9-267">Žádné položky seznamu úkolů se nezobrazují, protože dosud všechny nejsou pro vlastníka "*".</span><span class="sxs-lookup"><span data-stu-id="7dce9-267">No to-do items are displayed because until now they have all been for owner "*".</span></span> <span data-ttu-id="7dce9-268">Nyní bude prostřední vrstva požaduje položek pro přihlášeného uživatele a žádné ještě nejsou vytvořené.</span><span class="sxs-lookup"><span data-stu-id="7dce9-268">Now the middle tier is requesting items for the logged-on user, and none have been created yet.</span></span>
5. <span data-ttu-id="7dce9-269">Přidání nových položek úkolů, chcete-li ověřit, zda aplikace pracuje.</span><span class="sxs-lookup"><span data-stu-id="7dce9-269">Add new to-do items to verify that the application is working.</span></span>
6. <span data-ttu-id="7dce9-270">V jiném okně prohlížeče, přejděte na adresu URL uživatelského rozhraní Swagger pro aplikaci ToDoListDataAPI API a klikněte na tlačítko **ToDoList > získat**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-270">In another browser window, go to the Swagger UI URL for the ToDoListDataAPI API app and click **ToDoList > Get**.</span></span> <span data-ttu-id="7dce9-271">Zadejte hvězdičku pro `owner` parametr a pak klikněte na tlačítko **vyzkoušení**.</span><span class="sxs-lookup"><span data-stu-id="7dce9-271">Enter an asterisk for the `owner` parameter, and then click **Try it out**.</span></span>
   
   <span data-ttu-id="7dce9-272">Odpověď ukazuje, že mají nové položky seznamu úkolů skutečnou ID uživatele Azure AD ve vlastnosti vlastníka.</span><span class="sxs-lookup"><span data-stu-id="7dce9-272">The response shows that the new to-do items have the actual Azure AD user ID in the Owner property.</span></span>
   
   ![ID vlastníka v odpovědi JSON](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-the-projects-from-scratch"></a><span data-ttu-id="7dce9-274">Sestavení projektů od začátku</span><span class="sxs-lookup"><span data-stu-id="7dce9-274">Building the projects from scratch</span></span>
<span data-ttu-id="7dce9-275">Dva projekty webového rozhraní API, které byly vytvořeny pomocí **aplikace API Azure** šablony a nahrazení řadiče hodnoty výchozí řadič seznamu úkolů projektu.</span><span class="sxs-lookup"><span data-stu-id="7dce9-275">The two Web API projects were created by using the **Azure API App** project template and replacing the default Values controller with a ToDoList controller.</span></span> 

<span data-ttu-id="7dce9-276">Informace o tom, jak vytvořit jednostránková aplikace AngularJS s back-end webovém rozhraní API 2 najdete v tématu [rukou na testovacím: sestavení jedné stránce aplikace (SPA) pomocí rozhraní ASP.NET Web API a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs).</span><span class="sxs-lookup"><span data-stu-id="7dce9-276">For information about how to  create an AngularJS single-page application with a Web API 2 back end, see  [Hands On Lab: Build a Single Page Application (SPA) with ASP.NET Web API and Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs).</span></span> <span data-ttu-id="7dce9-277">Informace o tom, jak přidat kód pro ověřování Azure AD najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="7dce9-277">For information about how to add Azure AD authentication code, see the following resources:</span></span>

* <span data-ttu-id="7dce9-278">[Zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).</span><span class="sxs-lookup"><span data-stu-id="7dce9-278">[Securing AngularJS Single Page Apps with Azure AD](../active-directory/active-directory-devquickstarts-angular.md).</span></span>
* [<span data-ttu-id="7dce9-279">Představení ADAL JS v1</span><span class="sxs-lookup"><span data-stu-id="7dce9-279">Introducing ADAL JS v1</span></span>](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a><span data-ttu-id="7dce9-280">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7dce9-280">Troubleshooting</span></span>
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* <span data-ttu-id="7dce9-281">Ujistěte se, že jste Nepleťte ToDoListAPI (střední úroveň) a ToDoListDataAPI (datové vrstvy).</span><span class="sxs-lookup"><span data-stu-id="7dce9-281">Make sure that you don't confuse ToDoListAPI (middle tier) and ToDoListDataAPI (data tier).</span></span> <span data-ttu-id="7dce9-282">Ověřte například, že jste přidali ověřování do aplikace API střední vrstvy, ne na datovou vrstvu.</span><span class="sxs-lookup"><span data-stu-id="7dce9-282">For example, verify that you added authentication to the middle tier API app, not the data tier.</span></span> 
* <span data-ttu-id="7dce9-283">Ujistěte se, že zdrojový kód AngularJS odkazuje na adresu URL aplikace API střední vrstvy (ToDoListAPI, ToDoListDataAPI není) a správné Azure AD ID klienta.</span><span class="sxs-lookup"><span data-stu-id="7dce9-283">Make sure that the AngularJS source code references the middle tier API app URL (ToDoListAPI, not ToDoListDataAPI)and the correct Azure AD client ID.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7dce9-284">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7dce9-284">Next steps</span></span>
<span data-ttu-id="7dce9-285">V tomto kurzu jste se dozvěděli, jak používat ověřování služby App Service pro aplikaci API a jak volat aplikaci API pomocí knihovny ADAL JS.</span><span class="sxs-lookup"><span data-stu-id="7dce9-285">In this tutorial you learned how to use App Service authentication for an API app and how to call the API app by using the ADAL JS library.</span></span> <span data-ttu-id="7dce9-286">V dalším kurzu se dozvíte jak [zabezpečit přístup do vaší aplikace API pro scénáře service-to-service](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="7dce9-286">In the next tutorial you'll learn how to [secure access to your API app for service-to-service scenarios](app-service-api-dotnet-service-principal-auth.md).</span></span>

