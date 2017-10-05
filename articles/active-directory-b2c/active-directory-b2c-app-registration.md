---
title: 'Azure Active Directory B2C: Registrace aplikace | Dokumentace Microsoftu'
description: "Postup registrace aplikace pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: 3d4fe2fa10d848c8b29e4d22d284c0d378f07ae0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a><span data-ttu-id="7f034-103">Azure Active Directory B2C: Registrace vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-103">Azure Active Directory B2C: Register your application</span></span>

<span data-ttu-id="7f034-104">Tento rychlý start vám pomůže zaregistrovat aplikaci v tenantovi Microsoft Azure Active Directory (Azure AD) B2C během několika minut.</span><span class="sxs-lookup"><span data-stu-id="7f034-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="7f034-105">Jakmile budete hotovi, vaše aplikace bude zaregistrovaná pro použití v tenantovi Azure B2C.</span><span class="sxs-lookup"><span data-stu-id="7f034-105">When you're finished, your application is registered for use in the Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f034-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f034-106">Prerequisites</span></span>

<span data-ttu-id="7f034-107">Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="7f034-107">To build an application that accepts consumer sign-up and sign-in, you first need to register the application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="7f034-108">Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7f034-108">Get your own tenant by using the steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="7f034-109">Aplikace vytvořené z okna Azure AD B2C na webu Azure Portal se musí spravovat ze stejného místa.</span><span class="sxs-lookup"><span data-stu-id="7f034-109">Applications created from the Azure AD B2C blade in the Azure portal must be managed from the same location.</span></span> <span data-ttu-id="7f034-110">Pokud upravíte aplikace B2C pomocí PowerShellu nebo jiného portálu, stanou se nepodporované a nebudou s Azure AD B2C pracovat.</span><span class="sxs-lookup"><span data-stu-id="7f034-110">If you edit the B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="7f034-111">Podrobnosti najdete v části věnující se [chybným aplikacím](#faulted-apps).</span><span class="sxs-lookup"><span data-stu-id="7f034-111">See details in the [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-to-b2c-settings"></a><span data-ttu-id="7f034-112">Přechod do nastavení B2C</span><span class="sxs-lookup"><span data-stu-id="7f034-112">Navigate to B2C settings</span></span>

<span data-ttu-id="7f034-113">Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce tenanta B2C.</span><span class="sxs-lookup"><span data-stu-id="7f034-113">Log in to the [Azure portal](https://portal.azure.com/) as the Global Administrator of the B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="7f034-114">Další postup zvolte podle typu aplikace, kterou registrujete:</span><span class="sxs-lookup"><span data-stu-id="7f034-114">Choose next steps based on the application type you are registering:</span></span>

* [<span data-ttu-id="7f034-115">Registrace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-115">Register a web application</span></span>](#register-a-web-app)
* [<span data-ttu-id="7f034-116">Registrace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7f034-116">Register a web API</span></span>](#register-a-web-api)
* [<span data-ttu-id="7f034-117">Registrace mobilní nebo nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-117">Register a mobile or native application</span></span>](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a><span data-ttu-id="7f034-118">Registrace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-118">Register a web app</span></span>

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

<span data-ttu-id="7f034-119">Pokud vaše webová aplikace volá webové rozhraní API zabezpečené pomocí Azure AD B2C, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="7f034-119">If your web application calls a web API secured by Azure AD B2C, perform these steps:</span></span>
   1. <span data-ttu-id="7f034-120">Vytvořte Tajný klíč aplikace – přejděte do okna **Klíče** a klikněte na tlačítko **Vygenerovat klíč**.</span><span class="sxs-lookup"><span data-stu-id="7f034-120">Create an application secret by going to the **Keys** blade and clicking the **Generate Key** button.</span></span> <span data-ttu-id="7f034-121">Poznamenejte si hodnotu **Klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f034-121">Make note of the **App key** value.</span></span> <span data-ttu-id="7f034-122">Tuto hodnotu použijete jako tajný klíč aplikace v kódu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f034-122">You use the value as the application secret in your application's code.</span></span>
   2. <span data-ttu-id="7f034-123">Klikněte na **Přístup přes rozhraní API**, pak na **Přidat** a vyberte vaše webové rozhraní API a obory (oprávnění).</span><span class="sxs-lookup"><span data-stu-id="7f034-123">Click **API Access**, click **Add**, and select your web API and scopes (permissions).</span></span>

> [!NOTE]
> <span data-ttu-id="7f034-124">**Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.</span><span class="sxs-lookup"><span data-stu-id="7f034-124">An **Application Secret** is an important security credential, and should be secured appropriately.</span></span>
> 

[<span data-ttu-id="7f034-125">Přejít na **další kroky**</span><span class="sxs-lookup"><span data-stu-id="7f034-125">Jump to **next steps**</span></span>](#next-steps)

## <a name="register-a-web-api"></a><span data-ttu-id="7f034-126">Registrace webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7f034-126">Register a web API</span></span>

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

<span data-ttu-id="7f034-127">Klikněte na **Publikované obory** a podle potřeby přidejte další obory.</span><span class="sxs-lookup"><span data-stu-id="7f034-127">Click **Published scopes** to add more scopes as necessary.</span></span> <span data-ttu-id="7f034-128">Ve výchozím nastavení je definovaný obor user_impersonation.</span><span class="sxs-lookup"><span data-stu-id="7f034-128">By default, the "user_impersonation" scope is defined.</span></span> <span data-ttu-id="7f034-129">Obor user_impersonation umožňuje jiným aplikacím přístup k tomuto rozhraní API jménem přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7f034-129">The user_impersonation scope gives other applications the ability to access this api on behalf of the signed-in user.</span></span> <span data-ttu-id="7f034-130">Pokud chcete, můžete obor user_impersonation odebrat.</span><span class="sxs-lookup"><span data-stu-id="7f034-130">If you wish, the user_impersonation scope can be removed.</span></span>

[<span data-ttu-id="7f034-131">Přejít na **další kroky**</span><span class="sxs-lookup"><span data-stu-id="7f034-131">Jump to **next steps**</span></span>](#next-steps)

## <a name="register-a-mobile-or-native-app"></a><span data-ttu-id="7f034-132">Registrace mobilní nebo nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-132">Register a mobile or native app</span></span>

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[<span data-ttu-id="7f034-133">Přejít na **další kroky**</span><span class="sxs-lookup"><span data-stu-id="7f034-133">Jump to **next steps**</span></span>](#next-steps)

## <a name="limitations"></a><span data-ttu-id="7f034-134">Omezení</span><span class="sxs-lookup"><span data-stu-id="7f034-134">Limitations</span></span>

### <a name="choosing-a-web-app-or-api-reply-url"></a><span data-ttu-id="7f034-135">Výběr adresy URL odpovědi webové aplikace nebo rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7f034-135">Choosing a web app or api reply URL</span></span>

<span data-ttu-id="7f034-136">Aktuálně je u aplikací zaregistrovaných pomocí Azure AD B2C omezená sada hodnot adresy URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f034-136">Currently, apps that are registered with Azure AD B2C are restricted to a limited set of reply URL values.</span></span> <span data-ttu-id="7f034-137">Adresa URL odpovědi pro webové aplikace a služby musí začínat schématem `https` a všechny adresy URL odpovědi musí sdílet jednu doménu DNS.</span><span class="sxs-lookup"><span data-stu-id="7f034-137">The reply URL for web apps and services must begin with the scheme `https`, and all reply URL values must share a single DNS domain.</span></span> <span data-ttu-id="7f034-138">Například nemůžete zaregistrovat webovou aplikaci s některou z těchto adres URL odpovědi:</span><span class="sxs-lookup"><span data-stu-id="7f034-138">For example, you cannot register a web app that has one of these reply URLs:</span></span>

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="7f034-139">Registrační systém porovnává celý název DNS stávající adresy URL odpovědi s názvem DNS adresy URL odpovědi, kterou přidáváte.</span><span class="sxs-lookup"><span data-stu-id="7f034-139">The registration system compares the whole DNS name of the existing reply URL to the DNS name of the reply URL that you are adding.</span></span> <span data-ttu-id="7f034-140">Požadavek na přidání názvu DNS selže, pokud platí některá z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="7f034-140">The request to add the DNS name fails if either of the following conditions is true:</span></span>

* <span data-ttu-id="7f034-141">Celý název DNS nové adresy URL odpovědi neodpovídá názvu DNS stávající adresy URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f034-141">The whole DNS name of the new reply URL does not match the DNS name of the existing reply URL.</span></span>
* <span data-ttu-id="7f034-142">Celý název DNS nové adresy URL odpovědi není subdoménou stávající adresy URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f034-142">The whole DNS name of the new reply URL is not a subdomain of the existing reply URL.</span></span>

<span data-ttu-id="7f034-143">Pokud má aplikace například tuto adresu URL odpovědi:</span><span class="sxs-lookup"><span data-stu-id="7f034-143">For example, if the app has this reply URL:</span></span>

`https://login.contoso.com`

<span data-ttu-id="7f034-144">Můžete ji přidat tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="7f034-144">You can add to it, like this:</span></span>

`https://login.contoso.com/new`

<span data-ttu-id="7f034-145">V tomto případě se název DNS přesně shoduje.</span><span class="sxs-lookup"><span data-stu-id="7f034-145">In this case, the DNS name matches exactly.</span></span> <span data-ttu-id="7f034-146">Nebo můžete provést toto:</span><span class="sxs-lookup"><span data-stu-id="7f034-146">Or, you can do this:</span></span>

`https://new.login.contoso.com`

<span data-ttu-id="7f034-147">V tomto případě odkazujete na subdoménu DNS login.contoso.com. Pokud chcete mít aplikaci s adresami URL odpovědi login-east.contoso.com a login-west.contoso.com, musíte tyto adresy URL odpovědi přidat v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="7f034-147">In this case, you're referring to a DNS subdomain of login.contoso.com. If you want to have an app that has login-east.contoso.com and login-west.contoso.com as reply URLs, you must add those reply URLs in this order:</span></span>

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="7f034-148">Druhé dvě adresy URL odpovědi můžete přidat, protože jsou subdoménami první adresy URL odpovědi contoso.com.</span><span class="sxs-lookup"><span data-stu-id="7f034-148">You can add the latter two because they are subdomains of the first reply URL, contoso.com.</span></span>

### <a name="choosing-a-native-app-redirect-uri"></a><span data-ttu-id="7f034-149">Výběr identifikátoru URI přesměrování nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-149">Choosing a native app redirect URI</span></span>

<span data-ttu-id="7f034-150">Existují dva důležité aspekty při výběru identifikátoru URI přesměrování pro mobilní/nativní aplikace:</span><span class="sxs-lookup"><span data-stu-id="7f034-150">There are two important considerations when choosing a redirect URI for mobile/native applications:</span></span>

* <span data-ttu-id="7f034-151">**Jedinečnost:** Schéma identifikátoru URI přesměrování by mělo být pro každou aplikaci jedinečné.</span><span class="sxs-lookup"><span data-stu-id="7f034-151">**Unique**: The scheme of the redirect URI should be unique for every application.</span></span> <span data-ttu-id="7f034-152">V našem příkladu (com.onmicrosoft.contoso.appname://redirect/path) použijeme jako schéma com.onmicrosoft.contoso.appname.</span><span class="sxs-lookup"><span data-stu-id="7f034-152">In our example (com.onmicrosoft.contoso.appname://redirect/path), we use com.onmicrosoft.contoso.appname as the scheme.</span></span> <span data-ttu-id="7f034-153">Doporučujeme používat tento vzor.</span><span class="sxs-lookup"><span data-stu-id="7f034-153">We recommend following this pattern.</span></span> <span data-ttu-id="7f034-154">Pokud dvě aplikace sdílejí stejné schéma, uživateli se zobrazí dialogové okno pro výběr aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f034-154">If two applications share the same scheme, the user sees a "choose app" dialog.</span></span> <span data-ttu-id="7f034-155">Pokud uživatel použije nesprávnou volbu, přihlášení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="7f034-155">If the user makes an incorrect choice, the login fails.</span></span>
* <span data-ttu-id="7f034-156">**Úplnost:** Identifikátor URI přesměrování musí mít schéma a cestu.</span><span class="sxs-lookup"><span data-stu-id="7f034-156">**Complete**: Redirect URI must have a scheme and a path.</span></span> <span data-ttu-id="7f034-157">Cesta musí obsahovat za doménou alespoň jedno lomítko (například //contoso/ funguje a //contoso selže).</span><span class="sxs-lookup"><span data-stu-id="7f034-157">The path must contain at least one forward slash after the domain (for example, //contoso/ works and //contoso fails).</span></span>

<span data-ttu-id="7f034-158">Ujistěte se, že identifikátor URI přesměrování neobsahuje žádné speciální znaky jako podtržítka.</span><span class="sxs-lookup"><span data-stu-id="7f034-158">Ensure there are no special characters like underscores in the redirect uri.</span></span>

### <a name="faulted-apps"></a><span data-ttu-id="7f034-159">Chybné aplikace</span><span class="sxs-lookup"><span data-stu-id="7f034-159">Faulted apps</span></span>

<span data-ttu-id="7f034-160">Aplikace B2C se NESMÍ upravovat:</span><span class="sxs-lookup"><span data-stu-id="7f034-160">B2C applications should NOT be edited:</span></span>

* <span data-ttu-id="7f034-161">Na jiných portálech pro správu aplikací, jako je [portál Azure Classic](https://manage.windowsazure.com/) a [Portál pro registraci aplikací](https://apps.dev.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="7f034-161">On other application management portals such as the [Azure classic portal](https://manage.windowsazure.com/) & the [Application Registration Portal](https://apps.dev.microsoft.com/).</span></span>
* <span data-ttu-id="7f034-162">Pomocí rozhraní Graph API nebo PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="7f034-162">Using Graph API or PowerShell</span></span>

<span data-ttu-id="7f034-163">Pokud aplikaci B2C upravíte výše popsaným způsobem a pokusíte se ji znovu upravit v okně funkcí Azure AD B2C na webu Azure Portal, stane se chybnou aplikací a už ji nebude možné použít s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="7f034-163">If you edit the B2C application as described above and try to edit it again in the Azure AD B2C features blade on the Azure portal, it becomes a faulted app, and your application is no longer usable with Azure AD B2C.</span></span> <span data-ttu-id="7f034-164">Je nutné aplikaci odstranit a znovu ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="7f034-164">You have to delete the application and create it again.</span></span>

<span data-ttu-id="7f034-165">Pokud chcete aplikaci odstranit, přejděte na [Portál pro registraci aplikací](https://apps.dev.microsoft.com/) a tam ji odstraňte.</span><span class="sxs-lookup"><span data-stu-id="7f034-165">To delete the app, go to the [Application Registration Portal](https://apps.dev.microsoft.com/) and delete the application there.</span></span> <span data-ttu-id="7f034-166">Aby byla aplikace viditelná, musíte být vlastníkem aplikace (nestačí být pouze správcem tenanta).</span><span class="sxs-lookup"><span data-stu-id="7f034-166">In order for the application to be visible, you need to be the owner of the application (and not just an admin of the tenant).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f034-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f034-167">Next steps</span></span>

<span data-ttu-id="7f034-168">Nyní, když máte aplikaci registrovanou pomocí Azure AD B2C, můžete dokončit jeden z [našich kurzů pro rychlý start](active-directory-b2c-overview.md#get-started) pro uvedení do provozu.</span><span class="sxs-lookup"><span data-stu-id="7f034-168">Now that you have an application registered with Azure AD B2C, you can complete one of [our quick-start tutorials](active-directory-b2c-overview.md#get-started) to get up and running.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7f034-169">Vytvoření webové aplikace ASP.NET s registrací, přihlášením a resetováním hesla</span><span class="sxs-lookup"><span data-stu-id="7f034-169">Create an ASP.NET web app with sign-up, sign-in, and password reset</span></span>](active-directory-b2c-devquickstarts-web-dotnet-susi.md)