---
title: 'Azure Active Directory B2C: Registrace aplikace | Dokumentace Microsoftu'
description: "Jak tooregister vaší aplikace pomocí Azure Active Directory B2C"
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
ms.openlocfilehash: bd58e123751db387d6c8f16bd010291ba698b1a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a><span data-ttu-id="2764a-103">Azure Active Directory B2C: Registrace vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-103">Azure Active Directory B2C: Register your application</span></span>

<span data-ttu-id="2764a-104">Tento rychlý start vám pomůže zaregistrovat aplikaci v tenantovi Microsoft Azure Active Directory (Azure AD) B2C během několika minut.</span><span class="sxs-lookup"><span data-stu-id="2764a-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="2764a-105">Jakmile budete hotovi, vaše aplikace je zaregistrovaný pro použití v klientovi hello Azure B2C.</span><span class="sxs-lookup"><span data-stu-id="2764a-105">When you're finished, your application is registered for use in hello Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2764a-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2764a-106">Prerequisites</span></span>

<span data-ttu-id="2764a-107">toobuild aplikaci, která podporuje registrace a přihlašování uživatelů, musíte nejdřív aplikace hello tooregister s klienta služby Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="2764a-107">toobuild an application that accepts consumer sign-up and sign-in, you first need tooregister hello application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="2764a-108">Vlastního klienta získáte pomocí hello kroků uvedených v [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2764a-108">Get your own tenant by using hello steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="2764a-109">Aplikace vytvořené v okně hello Azure AD B2C v hello portál Azure musí být spravován z hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="2764a-109">Applications created from hello Azure AD B2C blade in hello Azure portal must be managed from hello same location.</span></span> <span data-ttu-id="2764a-110">Pokud chcete upravit hello B2C aplikací pomocí prostředí PowerShell nebo jiný portál, stane nepodporovaný a nefungují s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2764a-110">If you edit hello B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="2764a-111">Zobrazit podrobnosti v hello [došlo k chybě aplikace](#faulted-apps) části.</span><span class="sxs-lookup"><span data-stu-id="2764a-111">See details in hello [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-toob2c-settings"></a><span data-ttu-id="2764a-112">Přejděte tooB2C nastavení</span><span class="sxs-lookup"><span data-stu-id="2764a-112">Navigate tooB2C settings</span></span>

<span data-ttu-id="2764a-113">Přihlaste se toohello [portál Azure](https://portal.azure.com/) jako globální správce klienta hello B2C hello.</span><span class="sxs-lookup"><span data-stu-id="2764a-113">Log in toohello [Azure portal](https://portal.azure.com/) as hello Global Administrator of hello B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="2764a-114">Vyberte další kroky na základě typu aplikace hello, které chcete zaregistrovat:</span><span class="sxs-lookup"><span data-stu-id="2764a-114">Choose next steps based on hello application type you are registering:</span></span>

* [<span data-ttu-id="2764a-115">Registrace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-115">Register a web application</span></span>](#register-a-web-app)
* [<span data-ttu-id="2764a-116">Registrace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2764a-116">Register a web API</span></span>](#register-a-web-api)
* [<span data-ttu-id="2764a-117">Registrace mobilní nebo nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-117">Register a mobile or native application</span></span>](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a><span data-ttu-id="2764a-118">Registrace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-118">Register a web app</span></span>

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

<span data-ttu-id="2764a-119">Pokud vaše webová aplikace volá webové rozhraní API zabezpečené pomocí Azure AD B2C, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="2764a-119">If your web application calls a web API secured by Azure AD B2C, perform these steps:</span></span>
   1. <span data-ttu-id="2764a-120">Vytvořte tajný klíč aplikace tak, že budete toohello **klíče** okno a kliknutím na hello **vygenerovat klíč** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2764a-120">Create an application secret by going toohello **Keys** blade and clicking hello **Generate Key** button.</span></span> <span data-ttu-id="2764a-121">Poznamenejte si hello **klíč aplikace** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2764a-121">Make note of hello **App key** value.</span></span> <span data-ttu-id="2764a-122">Použijte hodnotu hello jako tajný klíč aplikace hello v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="2764a-122">You use hello value as hello application secret in your application's code.</span></span>
   2. <span data-ttu-id="2764a-123">Klikněte na **Přístup přes rozhraní API**, pak na **Přidat** a vyberte vaše webové rozhraní API a obory (oprávnění).</span><span class="sxs-lookup"><span data-stu-id="2764a-123">Click **API Access**, click **Add**, and select your web API and scopes (permissions).</span></span>

> [!NOTE]
> <span data-ttu-id="2764a-124">**Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.</span><span class="sxs-lookup"><span data-stu-id="2764a-124">An **Application Secret** is an important security credential, and should be secured appropriately.</span></span>
> 

[<span data-ttu-id="2764a-125">Přeskočit příliš**další kroky**</span><span class="sxs-lookup"><span data-stu-id="2764a-125">Jump too**next steps**</span></span>](#next-steps)

## <a name="register-a-web-api"></a><span data-ttu-id="2764a-126">Registrace webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2764a-126">Register a web API</span></span>

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

<span data-ttu-id="2764a-127">Klikněte na tlačítko **publikovaná obory** tooadd více rozsahy podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="2764a-127">Click **Published scopes** tooadd more scopes as necessary.</span></span> <span data-ttu-id="2764a-128">Ve výchozím nastavení je definováno obor "user_impersonation" hello.</span><span class="sxs-lookup"><span data-stu-id="2764a-128">By default, hello "user_impersonation" scope is defined.</span></span> <span data-ttu-id="2764a-129">obor user_impersonation Hello poskytuje další aplikace hello možnost tooaccess toto rozhraní api jménem hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2764a-129">hello user_impersonation scope gives other applications hello ability tooaccess this api on behalf of hello signed-in user.</span></span> <span data-ttu-id="2764a-130">Pokud chcete, můžete odebrat hello user_impersonation oboru.</span><span class="sxs-lookup"><span data-stu-id="2764a-130">If you wish, hello user_impersonation scope can be removed.</span></span>

[<span data-ttu-id="2764a-131">Přeskočit příliš**další kroky**</span><span class="sxs-lookup"><span data-stu-id="2764a-131">Jump too**next steps**</span></span>](#next-steps)

## <a name="register-a-mobile-or-native-app"></a><span data-ttu-id="2764a-132">Registrace mobilní nebo nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-132">Register a mobile or native app</span></span>

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[<span data-ttu-id="2764a-133">Přeskočit příliš**další kroky**</span><span class="sxs-lookup"><span data-stu-id="2764a-133">Jump too**next steps**</span></span>](#next-steps)

## <a name="limitations"></a><span data-ttu-id="2764a-134">Omezení</span><span class="sxs-lookup"><span data-stu-id="2764a-134">Limitations</span></span>

### <a name="choosing-a-web-app-or-api-reply-url"></a><span data-ttu-id="2764a-135">Výběr adresy URL odpovědi webové aplikace nebo rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2764a-135">Choosing a web app or api reply URL</span></span>

<span data-ttu-id="2764a-136">Aplikace, které jsou registrovány Azure AD B2C v současné době se s omezeným přístupem tooa omezenou sadu hodnot adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2764a-136">Currently, apps that are registered with Azure AD B2C are restricted tooa limited set of reply URL values.</span></span> <span data-ttu-id="2764a-137">Hello odpověď adresu URL pro webové aplikace a služby musí začínat řetězcem schématu hello `https`, a všechny odpovědi hodnoty adresa URL musí sdílí jedinou doménu DNS.</span><span class="sxs-lookup"><span data-stu-id="2764a-137">hello reply URL for web apps and services must begin with hello scheme `https`, and all reply URL values must share a single DNS domain.</span></span> <span data-ttu-id="2764a-138">Například nemůžete zaregistrovat webovou aplikaci s některou z těchto adres URL odpovědi:</span><span class="sxs-lookup"><span data-stu-id="2764a-138">For example, you cannot register a web app that has one of these reply URLs:</span></span>

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="2764a-139">systém registrace Hello porovná hello celý název DNS hello existující odpovědi URL toohello název DNS hello adresa URL odpovědi, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="2764a-139">hello registration system compares hello whole DNS name of hello existing reply URL toohello DNS name of hello reply URL that you are adding.</span></span> <span data-ttu-id="2764a-140">Hello požadavek tooadd hello název DNS selže, pokud platí některá z hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="2764a-140">hello request tooadd hello DNS name fails if either of hello following conditions is true:</span></span>

* <span data-ttu-id="2764a-141">Hello celý název DNS hello nové adresa URL odpovědi neodpovídá názvu DNS hello hello stávající adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2764a-141">hello whole DNS name of hello new reply URL does not match hello DNS name of hello existing reply URL.</span></span>
* <span data-ttu-id="2764a-142">Hello celý název DNS hello nové adresa URL odpovědi není subdoména hello stávající adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2764a-142">hello whole DNS name of hello new reply URL is not a subdomain of hello existing reply URL.</span></span>

<span data-ttu-id="2764a-143">Například pokud hello aplikace má tato adresa URL odpovědi:</span><span class="sxs-lookup"><span data-stu-id="2764a-143">For example, if hello app has this reply URL:</span></span>

`https://login.contoso.com`

<span data-ttu-id="2764a-144">Můžete přidat tooit takto:</span><span class="sxs-lookup"><span data-stu-id="2764a-144">You can add tooit, like this:</span></span>

`https://login.contoso.com/new`

<span data-ttu-id="2764a-145">V tomto případě název DNS hello přesně odpovídá.</span><span class="sxs-lookup"><span data-stu-id="2764a-145">In this case, hello DNS name matches exactly.</span></span> <span data-ttu-id="2764a-146">Nebo můžete provést toto:</span><span class="sxs-lookup"><span data-stu-id="2764a-146">Or, you can do this:</span></span>

`https://new.login.contoso.com`

<span data-ttu-id="2764a-147">V takovém případě odkazujete tooa DNS subdoménou login.contoso.com. Pokud chcete toohave aplikaci, která má přihlášení east.contoso.com a west.contoso.com přihlášení jako odpověď adresy URL, je nutné přidat tyto adresy URL odpovědí v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="2764a-147">In this case, you're referring tooa DNS subdomain of login.contoso.com. If you want toohave an app that has login-east.contoso.com and login-west.contoso.com as reply URLs, you must add those reply URLs in this order:</span></span>

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="2764a-148">Protože jsou subdomény hello první adresa URL odpovědi, můžete přidat hello dvou contoso.com.</span><span class="sxs-lookup"><span data-stu-id="2764a-148">You can add hello latter two because they are subdomains of hello first reply URL, contoso.com.</span></span>

### <a name="choosing-a-native-app-redirect-uri"></a><span data-ttu-id="2764a-149">Výběr identifikátoru URI přesměrování nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-149">Choosing a native app redirect URI</span></span>

<span data-ttu-id="2764a-150">Existují dva důležité aspekty při výběru identifikátoru URI přesměrování pro mobilní/nativní aplikace:</span><span class="sxs-lookup"><span data-stu-id="2764a-150">There are two important considerations when choosing a redirect URI for mobile/native applications:</span></span>

* <span data-ttu-id="2764a-151">**Jedinečné**: hello schéma hello identifikátor URI přesměrování by měl být jedinečný pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2764a-151">**Unique**: hello scheme of hello redirect URI should be unique for every application.</span></span> <span data-ttu-id="2764a-152">V našem příkladu (com.onmicrosoft.contoso.appname://redirect/path) používáme com.onmicrosoft.contoso.appname hello schéma.</span><span class="sxs-lookup"><span data-stu-id="2764a-152">In our example (com.onmicrosoft.contoso.appname://redirect/path), we use com.onmicrosoft.contoso.appname as hello scheme.</span></span> <span data-ttu-id="2764a-153">Doporučujeme používat tento vzor.</span><span class="sxs-lookup"><span data-stu-id="2764a-153">We recommend following this pattern.</span></span> <span data-ttu-id="2764a-154">Pokud dvě aplikace sdílet hello stejné scheme, hello uživateli se zobrazí dialogové okno "Vyberte aplikace".</span><span class="sxs-lookup"><span data-stu-id="2764a-154">If two applications share hello same scheme, hello user sees a "choose app" dialog.</span></span> <span data-ttu-id="2764a-155">Pokud uživatel hello provede nesprávné volbě, hello přihlášení selže.</span><span class="sxs-lookup"><span data-stu-id="2764a-155">If hello user makes an incorrect choice, hello login fails.</span></span>
* <span data-ttu-id="2764a-156">**Úplnost:** Identifikátor URI přesměrování musí mít schéma a cestu.</span><span class="sxs-lookup"><span data-stu-id="2764a-156">**Complete**: Redirect URI must have a scheme and a path.</span></span> <span data-ttu-id="2764a-157">Hello cesta musí obsahovat alespoň jeden lomítkem po hello domény (například //contoso/ funguje a selže //contoso).</span><span class="sxs-lookup"><span data-stu-id="2764a-157">hello path must contain at least one forward slash after hello domain (for example, //contoso/ works and //contoso fails).</span></span>

<span data-ttu-id="2764a-158">Ujistěte se, že neexistují žádné speciální znaky, jako identifikátor uri pro přesměrování podtržítka v hello.</span><span class="sxs-lookup"><span data-stu-id="2764a-158">Ensure there are no special characters like underscores in hello redirect uri.</span></span>

### <a name="faulted-apps"></a><span data-ttu-id="2764a-159">Chybné aplikace</span><span class="sxs-lookup"><span data-stu-id="2764a-159">Faulted apps</span></span>

<span data-ttu-id="2764a-160">Aplikace B2C se NESMÍ upravovat:</span><span class="sxs-lookup"><span data-stu-id="2764a-160">B2C applications should NOT be edited:</span></span>

* <span data-ttu-id="2764a-161">Na jiných portálech pro správu aplikací, jako je [portál Azure Classic](https://manage.windowsazure.com/) a [Portál pro registraci aplikací](https://apps.dev.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="2764a-161">On other application management portals such as the [Azure classic portal](https://manage.windowsazure.com/) & the [Application Registration Portal](https://apps.dev.microsoft.com/).</span></span>
* <span data-ttu-id="2764a-162">Pomocí rozhraní Graph API nebo PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="2764a-162">Using Graph API or PowerShell</span></span>

<span data-ttu-id="2764a-163">Pokud jste upravit aplikaci hello B2C, jak je popsáno výše a opakujte tooedit ho znovu okno s funkcemi hello Azure AD B2C na portálu Azure hello, stane se aplikace pro práci s chybou a aplikace již není použitelné s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2764a-163">If you edit hello B2C application as described above and try tooedit it again in hello Azure AD B2C features blade on hello Azure portal, it becomes a faulted app, and your application is no longer usable with Azure AD B2C.</span></span> <span data-ttu-id="2764a-164">Aplikace hello toodelete a vytvořte ho znovu.</span><span class="sxs-lookup"><span data-stu-id="2764a-164">You have toodelete hello application and create it again.</span></span>

<span data-ttu-id="2764a-165">toodelete hello aplikace, přejděte toohello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/) a odstranit hello aplikaci existuje.</span><span class="sxs-lookup"><span data-stu-id="2764a-165">toodelete hello app, go toohello [Application Registration Portal](https://apps.dev.microsoft.com/) and delete hello application there.</span></span> <span data-ttu-id="2764a-166">Aby toobe aplikace hello viditelné je třeba vlastník hello toobe aplikace hello (a ne jenom správce hello klienta).</span><span class="sxs-lookup"><span data-stu-id="2764a-166">In order for hello application toobe visible, you need toobe hello owner of hello application (and not just an admin of hello tenant).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2764a-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2764a-167">Next steps</span></span>

<span data-ttu-id="2764a-168">Teď, když máte aplikaci registrovanou v Azure AD B2C, můžete dokončit jeden z [naše kurzy úvodní](active-directory-b2c-overview.md#get-started) tooget fungovaly.</span><span class="sxs-lookup"><span data-stu-id="2764a-168">Now that you have an application registered with Azure AD B2C, you can complete one of [our quick-start tutorials](active-directory-b2c-overview.md#get-started) tooget up and running.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2764a-169">Vytvoření webové aplikace ASP.NET s registrací, přihlášením a resetováním hesla</span><span class="sxs-lookup"><span data-stu-id="2764a-169">Create an ASP.NET web app with sign-up, sign-in, and password reset</span></span>](active-directory-b2c-devquickstarts-web-dotnet-susi.md)