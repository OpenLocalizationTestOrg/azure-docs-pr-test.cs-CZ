---
title: "Jak nakonfigurovat Microsoft Account ověřování pro aplikaci aplikační služby"
description: "Zjistěte, jak nakonfigurovat Microsoft Account ověřování pro aplikaci aplikační služby."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 67386b03ae4cc683fe00e11e8dad19d1442eff09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a><span data-ttu-id="b9af5-103">Postup konfigurace aplikace služby App Service k používání Microsoft Account přihlášení</span><span class="sxs-lookup"><span data-stu-id="b9af5-103">How to configure your App Service application to use Microsoft Account login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="b9af5-104">Toto téma ukazuje, jak nakonfigurovat služby Azure App Service pro použití Microsoft Account jako zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="b9af5-104">This topic shows you how to configure Azure App Service to use Microsoft Account as an authentication provider.</span></span> 

## <span data-ttu-id="b9af5-105"><a name="register-microsoft-account"></a>Svou aplikaci zaregistrovat pomocí účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9af5-105"><a name="register-microsoft-account"> </a>Register your app with Microsoft Account</span></span>
1. <span data-ttu-id="b9af5-106">Přihlaste se na [portál Azure]a přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b9af5-106">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="b9af5-107">Kopie vašeho **URL**, které později můžete použít ke konfiguraci vaší aplikace s Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9af5-107">Copy your **URL**, which later you use to configure your app with Microsoft Account.</span></span>
2. <span data-ttu-id="b9af5-108">Přejděte na [Moje aplikace] stránky na webu Microsoft Developer Center Account a přihlaste se pomocí účtu Microsoft, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b9af5-108">Navigate to the [My Applications] page in the Microsoft Account Developer Center, and log on with your Microsoft account, if required.</span></span>
3. <span data-ttu-id="b9af5-109">Klikněte na tlačítko **přidat aplikaci**, pak zadejte název aplikace a klikněte na **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-109">Click **Add an app**, then type an application name, and click **Create application**.</span></span>
4. <span data-ttu-id="b9af5-110">Poznamenejte si **ID aplikace**, protože jej budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="b9af5-110">Make a note of the **Application ID**, as you will need it later.</span></span> 
5. <span data-ttu-id="b9af5-111">V části "Platformy", klikněte na **přidejte platformu** a vyberte možnost "Web".</span><span class="sxs-lookup"><span data-stu-id="b9af5-111">Under "Platforms," click **Add Platform** and select "Web".</span></span>
6. <span data-ttu-id="b9af5-112">V části "Identifikátory URI přesměrování" zadat koncový bod pro vaši aplikaci a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-112">Under "Redirect URIs" supply the endpoint for your application, then click **Save**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="b9af5-113">Vaše přesměrování identifikátor URI je adresa URL aplikace připojí s cestou, */.auth/login/microsoftaccount/callback*.</span><span class="sxs-lookup"><span data-stu-id="b9af5-113">Your redirect URI is the URL of your application appended with the path, */.auth/login/microsoftaccount/callback*.</span></span> <span data-ttu-id="b9af5-114">Například, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span><span class="sxs-lookup"><span data-stu-id="b9af5-114">For example, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span></span>   
   > <span data-ttu-id="b9af5-115">Ujistěte se, že používáte schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b9af5-115">Make sure that you are using the HTTPS scheme.</span></span>
   
7. <span data-ttu-id="b9af5-116">V části "Aplikace tajné údaje", klikněte na **generovat nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-116">Under "Application Secrets," click **Generate New Password**.</span></span> <span data-ttu-id="b9af5-117">Poznamenejte si hodnotu, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b9af5-117">Make note of the value that appears.</span></span> <span data-ttu-id="b9af5-118">Jakmile stránku opustit, nebudou zobrazeny znovu.</span><span class="sxs-lookup"><span data-stu-id="b9af5-118">Once you leave the page, it will not be displayed again.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b9af5-119">Heslo je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="b9af5-119">The password is an important security credential.</span></span> <span data-ttu-id="b9af5-120">S kýmkoli sdílet heslo nebo distribuovat v rámci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9af5-120">Do not share the password with anyone or distribute it within a client application.</span></span>

## <span data-ttu-id="b9af5-121"><a name="secrets"></a>Přidat účet Microsoft informace, které aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="b9af5-121"><a name="secrets"> </a>Add Microsoft Account information to your App Service application</span></span>
1. <span data-ttu-id="b9af5-122">Zpět v [portál Azure], přejděte k aplikaci, klikněte na **nastavení** > **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-122">Back in the [Azure portal], navigate to your application, click **Settings** > **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="b9af5-123">Pokud ověřování / autorizace funkce není povolena, je přepínač **na**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-123">If the Authentication / Authorization feature is not enabled, switch it **On**.</span></span>
3. <span data-ttu-id="b9af5-124">Klikněte na tlačítko **účtu Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-124">Click **Microsoft Account**.</span></span> <span data-ttu-id="b9af5-125">Vložte hodnoty ID aplikace a heslo, které jste získali dříve a volitelně povolte všechny obory, které vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="b9af5-125">Paste in the Application ID and Password values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="b9af5-126">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-126">Then click **OK**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="b9af5-127">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b9af5-127">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="b9af5-128">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9af5-128">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="b9af5-129">(Volitelné) Chcete-li omezit přístup k webu jenom na uživatele ověřeného službou účet Microsoft, nastavte **akci provést, když požadavek nebude ověřený** k **Account Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-129">(Optional) To restrict access to your site to only users authenticated by Microsoft account, set **Action to take when request is not authenticated** to **Microsoft Account**.</span></span> <span data-ttu-id="b9af5-130">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do účtu Microsoft pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="b9af5-130">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Microsoft account for authentication.</span></span>
5. <span data-ttu-id="b9af5-131">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9af5-131">Click **Save**.</span></span>

<span data-ttu-id="b9af5-132">Nyní jste připraveni použít Microsoft Account pro ověřování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b9af5-132">You are now ready to use Microsoft Account for authentication in your app.</span></span>

## <span data-ttu-id="b9af5-133"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="b9af5-133"><a name="related-content"> </a>Related content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[portál Azure]: https://portal.azure.com/
