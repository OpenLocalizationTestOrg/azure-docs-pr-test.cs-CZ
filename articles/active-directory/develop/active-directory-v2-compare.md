---
title: "Co je nového v koncového bodu v2.0 Azure AD? | Dokumentace Microsoftu"
description: "Porovnání mezi původní Azure AD v2.0 koncové body a."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 81de65b0e825dec64383f52b02c5ee56c9434807
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="whats-different-about-the-v20-endpoint"></a><span data-ttu-id="7c94e-104">Co se liší o koncový bod v2.0?</span><span class="sxs-lookup"><span data-stu-id="7c94e-104">What's different about the v2.0 endpoint?</span></span>
<span data-ttu-id="7c94e-105">Pokud jste se seznámili s Azure Active Directory nebo aplikací mít integrované s Azure AD v minulosti, může být určité rozdíly v koncový bod v2.0, které by uživatel očekával.</span><span class="sxs-lookup"><span data-stu-id="7c94e-105">If you're familiar with Azure Active Directory or have integrated apps with Azure AD in the past, there may be some differences in the v2.0 endpoint that you would not expect.</span></span>  <span data-ttu-id="7c94e-106">Tento dokument se volá na těchto rozdílů za pochopení.</span><span class="sxs-lookup"><span data-stu-id="7c94e-106">This document calls out those differences for your understanding.</span></span>

> [!NOTE]
> <span data-ttu-id="7c94e-107">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="7c94e-107">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="7c94e-108">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7c94e-108">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a><span data-ttu-id="7c94e-109">Účty Microsoft a účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c94e-109">Microsoft accounts and Azure AD accounts</span></span>
<span data-ttu-id="7c94e-110">Koncový bod v2.0 umožňují vývojářům zápisu aplikace, které přijímají přihlášení z účtů Microsoft Accounts i Azure AD, pomocí koncový bod jedné ověřování.</span><span class="sxs-lookup"><span data-stu-id="7c94e-110">The v2.0 endpoint allow developers to write apps that accept sign-in from both Microsoft Accounts and Azure AD accounts, using a single auth endpoint.</span></span>  <span data-ttu-id="7c94e-111">To vám dává možnost zapisovat aplikaci zcela účet bez ohledu na; může být typu účtu, který uživatel přihlásí, které ignorují.</span><span class="sxs-lookup"><span data-stu-id="7c94e-111">This gives you the ability to write your app completely account-agnostic; it can be ignorant of the type of account that the user signs in with.</span></span>  <span data-ttu-id="7c94e-112">Samozřejmě můžete *můžete* zvýšit vaši aplikaci vědět, typ účtu se používá v konkrétní relace, ale není nutné.</span><span class="sxs-lookup"><span data-stu-id="7c94e-112">Of course, you *can* make your app aware of the type of account being used in a particular session, but you don't have to.</span></span>

<span data-ttu-id="7c94e-113">Například pokud aplikace zavolá [Microsoft Graph](https://graph.microsoft.io), některé další funkce a dat budou k dispozici uživatelům enterprise, jako jsou jejich weby SharePoint nebo dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="7c94e-113">For instance, if your app calls the [Microsoft Graph](https://graph.microsoft.io), some additional functionality and data will be available to enterprise users, such as their SharePoint sites or Directory data.</span></span>  <span data-ttu-id="7c94e-114">Ale pro mnoho akce jako například [čtení e-mailu uživatele](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), kód může být napsán stejně pro účty Microsoft Accounts a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c94e-114">But for many actions, such as [Reading a user's mail](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), the code can be written exactly the same for both Microsoft Accounts and Azure AD accounts.</span></span>  

<span data-ttu-id="7c94e-115">Integrace aplikace s Accounts Microsoft a účty Azure AD je nyní jeden jednoduchý proces.</span><span class="sxs-lookup"><span data-stu-id="7c94e-115">Integrating your app with Microsoft Accounts and Azure AD accounts is now one simple process.</span></span>  <span data-ttu-id="7c94e-116">K získání přístupu k příjemce i enterprise světů můžete použít jednu sadu koncové body, jeden knihovny a registrace jediné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c94e-116">You can use a single set of endpoints, a single library, and a single app registration to gain access to both the consumer and enterprise worlds.</span></span>  <span data-ttu-id="7c94e-117">Další informace o koncový bod v2.0, podívejte se na [přehledu](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7c94e-117">To learn more about the v2.0 endpoint, check out [the overview](active-directory-appmodel-v2-overview.md).</span></span>

## <a name="new-app-registration-portal"></a><span data-ttu-id="7c94e-118">Nový portál pro registraci aplikace</span><span class="sxs-lookup"><span data-stu-id="7c94e-118">New app registration portal</span></span>
<span data-ttu-id="7c94e-119">Chcete-li zaregistrovat aplikaci, která funguje s koncovým bodem v2.0, musíte použít nové portálu pro registraci aplikace: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="7c94e-119">To register an app that works with the v2.0 endpoint, you must use a new app registration portal: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="7c94e-120">Toto je portál, kde lze získat ID aplikace přizpůsobení vzhledu přihlašovací stránku vaší aplikace a další.</span><span class="sxs-lookup"><span data-stu-id="7c94e-120">This is the portal where you can obtain an application ID, customize the appearance of your app's sign-in page, and more.</span></span>  <span data-ttu-id="7c94e-121">Všechny potřebné pro přístup k portálu je účet používá technologii Microsoft - osobní nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="7c94e-121">All you need to access the portal is a Microsoft powered account - either personal or work/school account.</span></span>

## <a name="one-app-id-for-all-platforms"></a><span data-ttu-id="7c94e-122">ID jednu aplikaci pro všechny platformy</span><span class="sxs-lookup"><span data-stu-id="7c94e-122">One app ID for all platforms</span></span>
<span data-ttu-id="7c94e-123">Pokud jste použili Azure Active Directory, pravděpodobně jste registrováni několik různých aplikací pro jeden projekt.</span><span class="sxs-lookup"><span data-stu-id="7c94e-123">If you've used Azure Active Directory, you've probably registered several different apps for a single project.</span></span>  <span data-ttu-id="7c94e-124">Například pokud jste vytvořili web a aplikaci pro iOS, museli jste samostatně, zaregistrujte je použití dvou různých ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-124">For example, if you built both a website and an iOS app, you had to register them separately, using two different Application Ids.</span></span> <span data-ttu-id="7c94e-125">Portálu pro registraci aplikace Azure AD vynutit vám umožní provádět na těchto rozdílech během registrace:</span><span class="sxs-lookup"><span data-stu-id="7c94e-125">The Azure AD app registration portal forced you to make this distinction during registration:</span></span>

![Původní registrace aplikace uživatelského rozhraní](../../media/active-directory-v2-flows/old_app_registration.PNG)

<span data-ttu-id="7c94e-127">Podobně pokud jste měli web a back-endové webové rozhraní api, může jste si zaregistrovali každý jako samostatnou aplikaci ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c94e-127">Similarly, if you had a website and a backend web api, you might have registered each as a separate app in Azure AD.</span></span>  <span data-ttu-id="7c94e-128">Nebo pokud jste měli k aplikaci pro iOS a aplikace pro Android, můžete také může mít registrovaný dvě různé aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-128">Or if you had an iOS app and an Android app, you also might have registered two different apps.</span></span>  <span data-ttu-id="7c94e-129">Registrace jednotlivých součástí aplikace vedla k některé neočekávané chování pro vývojáře a zákazníkům:</span><span class="sxs-lookup"><span data-stu-id="7c94e-129">Registering each component of an application led to some unexpected behaviors for developers and their customers:</span></span>

* <span data-ttu-id="7c94e-130">Jednotlivé komponenty zobrazovaly jako samostatnou aplikaci v klientovi služby Azure Active Directory z každého zákazníka.</span><span class="sxs-lookup"><span data-stu-id="7c94e-130">Each component appeared as a separate app in the Azure Active Directory tenant of each customer.</span></span>
* <span data-ttu-id="7c94e-131">Při pokusu o použití zásad ke správě přístupu k nebo odstranit aplikaci správce klienta, mají by k tomu pro jednotlivé součásti aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-131">When a tenant administrator attempted to apply policy to, manage access to, or delete an app, they would have to do so for each component of the app.</span></span>
* <span data-ttu-id="7c94e-132">Když zákazníci dá souhlas k aplikaci, jednotlivé komponenty se zobrazí na obrazovce souhlasu jako různých aplikací.</span><span class="sxs-lookup"><span data-stu-id="7c94e-132">When customers consented to an application, each component would appear in the consent screen as a distinct application.</span></span>

<span data-ttu-id="7c94e-133">S koncovým bodem v2.0 teď můžete zaregistrovat všechny součásti projektu jako registrace jedna aplikace a použít jeden Id aplikace pro celý projekt.</span><span class="sxs-lookup"><span data-stu-id="7c94e-133">With the v2.0 endpoint, you can now register all components of your project as a single app registration, and use a single Application Id for your entire project.</span></span>  <span data-ttu-id="7c94e-134">Můžete přidat několik "platforem" pro každý projekt a zadejte příslušná data pro každou platformu, kterou přidáte.</span><span class="sxs-lookup"><span data-stu-id="7c94e-134">You can add several "platforms" to a each project, and provide the appropriate data for each platform you add.</span></span>  <span data-ttu-id="7c94e-135">Samozřejmě můžete vytvořit libovolný počet aplikací, jako třeba v závislosti na požadavcích, ale pro většině případů musí být nutné jenom jeden Id aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-135">Of course, you can create as many apps as you like depending on your requirements, but for the majority of cases only one Application Id should be necessary.</span></span>

<span data-ttu-id="7c94e-136">Naše cílem je, že to vést k více zjednodušené správy aplikací a vývojové prostředí a vytvořit více konsolidované zobrazení jeden projekt, který je možné, že pracujete na.</span><span class="sxs-lookup"><span data-stu-id="7c94e-136">Our aim is that this will lead to a more simplified app management and development experience, and create a more consolidated view of a single project that you might be working on.</span></span>

## <a name="scopes-not-resources"></a><span data-ttu-id="7c94e-137">Obory, ne prostředky</span><span class="sxs-lookup"><span data-stu-id="7c94e-137">Scopes, not resources</span></span>
<span data-ttu-id="7c94e-138">V Azure Active Directory, můžete aplikaci chovat jako **prostředků**, nebo příjemce tokenů.</span><span class="sxs-lookup"><span data-stu-id="7c94e-138">In Azure Active Directory, an app can behave as a **resource**, or a recipient of tokens.</span></span>  <span data-ttu-id="7c94e-139">Prostředek můžete definovat několika **obory** nebo **oAuth2Permissions** , že rozumí, umožňuje tak klientským aplikace k žádosti o tokeny pro tento prostředek pro určitou oborů.</span><span class="sxs-lookup"><span data-stu-id="7c94e-139">A resource can define a number of **scopes** or **oAuth2Permissions** that it understands, allowing client apps to request tokens to that resource for a certain set of scopes.</span></span>  <span data-ttu-id="7c94e-140">Vezměte v úvahu Azure AD Graph API jako příklad prostředku:</span><span class="sxs-lookup"><span data-stu-id="7c94e-140">Consider the Azure AD Graph API as an example of a resource:</span></span>

* <span data-ttu-id="7c94e-141">Identifikátor prostředku nebo `AppID URI`:`https://graph.windows.net/`</span><span class="sxs-lookup"><span data-stu-id="7c94e-141">Resource Identifier, or `AppID URI`: `https://graph.windows.net/`</span></span>
* <span data-ttu-id="7c94e-142">Obory, nebo `OAuth2Permissions`: `Directory.Read`, `Directory.Write`atd.</span><span class="sxs-lookup"><span data-stu-id="7c94e-142">Scopes, or `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, etc.</span></span>  

<span data-ttu-id="7c94e-143">Všechny tyto platí pro koncový bod v2.0.</span><span class="sxs-lookup"><span data-stu-id="7c94e-143">All of this holds true for the the v2.0 endpoint.</span></span>  <span data-ttu-id="7c94e-144">Aplikace může stále chovat jako prostředek, definovat obory a identifikovat podle identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="7c94e-144">An app can still behave as resource, define scopes, and be identified by a URI.</span></span>  <span data-ttu-id="7c94e-145">Klientské aplikace může stále požádat o přístup k tyto obory.</span><span class="sxs-lookup"><span data-stu-id="7c94e-145">Client apps can still request access to those scopes.</span></span>  <span data-ttu-id="7c94e-146">Ale způsob, ve kterém klient požaduje tato oprávnění se změnila.</span><span class="sxs-lookup"><span data-stu-id="7c94e-146">However, the way in which a client requests those permissions has changed.</span></span>  <span data-ttu-id="7c94e-147">V minulosti autorizace OAuth 2.0 požadavek do služby Azure AD může mít hledá jako:</span><span class="sxs-lookup"><span data-stu-id="7c94e-147">In the past, an OAuth 2.0 authorize request to Azure AD might have looked like:</span></span>

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

<span data-ttu-id="7c94e-148">kde **prostředků** parametr uvedené, který prostředek se klientská aplikace požaduje autorizaci pro.</span><span class="sxs-lookup"><span data-stu-id="7c94e-148">where the **resource** parameter indicated which resource the client app is requesting authorization for.</span></span>  <span data-ttu-id="7c94e-149">Azure AD vypočtená oprávnění požadovaná aplikace na základě statické konfigurace na portálu Azure a vystavené tokeny odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="7c94e-149">Azure AD computed the permissions required by the app based on static configuration in the Azure Portal, and issued tokens accordingly.</span></span>  <span data-ttu-id="7c94e-150">Nyní stejné OAuth 2.0 autorizace požadavku vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="7c94e-150">Now, the same OAuth 2.0 authorize request looks like:</span></span>

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

<span data-ttu-id="7c94e-151">kde **oboru** parametr určuje, který prostředek a oprávnění aplikace požaduje autorizaci pro.</span><span class="sxs-lookup"><span data-stu-id="7c94e-151">where the **scope** parameter indicates which resource and permissions the app is requesting authorization for.</span></span> <span data-ttu-id="7c94e-152">Požadovaný prostředek je stále velmi přítomné v žádosti – jednoduše je zahrnut v jednotlivých hodnot parametru oboru.</span><span class="sxs-lookup"><span data-stu-id="7c94e-152">The desired resource is still very present in the request - it is simply encompassed in each of the values of the scope parameter.</span></span>  <span data-ttu-id="7c94e-153">Pomocí parametru oboru tímto způsobem umožňuje koncového bodu v2.0 být více kompatibilní se specifikací OAuth 2.0 a přesněji zarovnaná s obvyklé postupy odvětví.</span><span class="sxs-lookup"><span data-stu-id="7c94e-153">Using the scope parameter in this manner allows the v2.0 endpoint to be more compliant with the OAuth 2.0 specification, and aligns more closely with common industry practices.</span></span>  <span data-ttu-id="7c94e-154">Umožňuje také aplikace k provedení [přírůstkové souhlasu](#incremental-and-dynamic-consent), který je popsaný v následující části.</span><span class="sxs-lookup"><span data-stu-id="7c94e-154">It also enables apps to perform [incremental consent](#incremental-and-dynamic-consent), which is described in the next section.</span></span>

## <a name="incremental-and-dynamic-consent"></a><span data-ttu-id="7c94e-155">Přírůstkové a dynamické souhlasu</span><span class="sxs-lookup"><span data-stu-id="7c94e-155">Incremental and dynamic consent</span></span>
<span data-ttu-id="7c94e-156">Aplikace zaregistrované ve službě Azure AD dříve potřeby k zadání jejich požadovaná oprávnění OAuth 2.0 na portálu Azure při vytváření aplikace:</span><span class="sxs-lookup"><span data-stu-id="7c94e-156">Apps registered in Azure AD previously needed to specify their required OAuth 2.0 permissions in the Azure Portal, at app creation time:</span></span>

![Registrace oprávnění uživatelského rozhraní](../../media/active-directory-v2-flows/app_reg_permissions.PNG)

<span data-ttu-id="7c94e-158">Oprávnění požadované aplikace byla nakonfigurována **staticky**.</span><span class="sxs-lookup"><span data-stu-id="7c94e-158">The permissions an app required were configured **statically**.</span></span>  <span data-ttu-id="7c94e-159">Když to povoleno konfigurace aplikace existovat v portálu Azure a uchovávat kód dobrý a jednoduché, uvede několik problémů pro vývojáře:</span><span class="sxs-lookup"><span data-stu-id="7c94e-159">While this allowed configuration of the app to exist in the Azure Portal and kept the code nice and simple, it presents a few issues for developers:</span></span>

* <span data-ttu-id="7c94e-160">Aplikace se museli vědět všechna oprávnění, která se někdy potřebovat při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-160">An app had to know all of the permissions it would ever need at app creation time.</span></span>  <span data-ttu-id="7c94e-161">Přidání oprávnění v čase bylo obtížné procesu.</span><span class="sxs-lookup"><span data-stu-id="7c94e-161">Adding permissions over time was a difficult process.</span></span>
* <span data-ttu-id="7c94e-162">Aplikace musela všechny prostředky, ke kterým by někdy přístup předem znát.</span><span class="sxs-lookup"><span data-stu-id="7c94e-162">An app had to know all of the resources it would ever access ahead of time.</span></span>  <span data-ttu-id="7c94e-163">Bylo obtížné vytvářet aplikace, které mají přístup libovolný počet prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c94e-163">It was difficult to create apps that could access an arbitrary number of resources.</span></span>
* <span data-ttu-id="7c94e-164">Aplikace musí požádat o všechna oprávnění, která by někdy potřebovat při uživatele první přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7c94e-164">An app had to request all the permissions it would ever need upon the user's first sign-in.</span></span>  <span data-ttu-id="7c94e-165">V některých případech to vedlo k velmi dlouhý seznam oprávnění, která se nedoporučuje. koncoví uživatelé z schválení aplikace přístup na počáteční přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7c94e-165">In some cases this led to a very long list of permissions, which discouraged end-users from approving the app's access on initial sign-in.</span></span>

<span data-ttu-id="7c94e-166">S koncovým bodem v2.0, můžete zadat oprávnění vašim potřebám aplikace **dynamicky**, v době běhu během regulární využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-166">With the v2.0 endpoint, you can specify the permissions your app needs **dynamically**, at runtime, during regular usage of your app.</span></span>  <span data-ttu-id="7c94e-167">Uděláte to tak, můžete zadat obory, vaše aplikace, musí v libovolném bodě v čase zahrnutím je do `scope` parametr požadavku ověřování:</span><span class="sxs-lookup"><span data-stu-id="7c94e-167">To do so, you can specify the scopes your app needs at any given point in time by including them in the `scope` parameter of an authorization request:</span></span>

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

<span data-ttu-id="7c94e-168">Výše uvedené požadavky oprávnění pro aplikaci pro čtení dat adresáře uživatele Azure AD, stejně jako zapsat data do svého adresáře.</span><span class="sxs-lookup"><span data-stu-id="7c94e-168">The above requests permission for the app to read an Azure AD user's directory data, as well as write data to their directory.</span></span>  <span data-ttu-id="7c94e-169">Pokud uživatel souhlasí s tato oprávnění v minulosti pro tuto konkrétní aplikaci, bude jednoduše zadat své přihlašovací údaje a být přihlášeni k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c94e-169">If the user has consented to those permissions in the past for this particular app, they will simply enter their credentials and be signed into the app.</span></span>  <span data-ttu-id="7c94e-170">Pokud uživatel nebyla přijata některé z těchto oprávnění, koncový bod v2.0 požádá uživatele o souhlas těchto oprávnění.</span><span class="sxs-lookup"><span data-stu-id="7c94e-170">If the user has not consented to any of these permissions, the v2.0 endpoint will ask the user for consent to those permissions.</span></span>  <span data-ttu-id="7c94e-171">Další informace si můžete přečíst na [oprávnění, souhlasu a obory](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="7c94e-171">To learn more, you can read up on [permissions, consent, and scopes](active-directory-v2-scopes.md).</span></span>

<span data-ttu-id="7c94e-172">Povolení aplikace a požádat o oprávnění dynamicky prostřednictvím `scope` parametr získáte plnou kontrolu nad možnosti pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c94e-172">Allowing an app to request permissions dynamically via the `scope` parameter gives you full control over your user's experience.</span></span>  <span data-ttu-id="7c94e-173">Pokud chcete, můžete frontload vašeho souhlasu prostředí a požádat o všechna oprávnění v jedné žádosti počáteční autorizace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-173">If you wish, you can choose to frontload your consent experience and ask for all permissions in one initial authorization request.</span></span>  <span data-ttu-id="7c94e-174">Nebo pokud vaše aplikace vyžaduje velký počet oprávnění, můžete získat tato oprávnění od uživatele postupně, jak se pokus o použití některých funkcí aplikace v čase.</span><span class="sxs-lookup"><span data-stu-id="7c94e-174">Or if your app requires a large number of permissions, you can choose to gather those permissions from the user incrementally, as they attempt to use certain features of your app over time.</span></span>

## <a name="well-known-scopes"></a><span data-ttu-id="7c94e-175">Známé oborů</span><span class="sxs-lookup"><span data-stu-id="7c94e-175">Well-known scopes</span></span>
#### <a name="offline-access"></a><span data-ttu-id="7c94e-176">Přístup v režimu offline</span><span class="sxs-lookup"><span data-stu-id="7c94e-176">Offline access</span></span>
<span data-ttu-id="7c94e-177">Aplikace používající koncového bodu v2.0 může vyžadovat použití nové dobře známé oprávnění pro aplikace - `offline_access` oboru.</span><span class="sxs-lookup"><span data-stu-id="7c94e-177">Apps using the v2.0 endpoint may require the use of a new well-known permission for apps - the `offline_access` scope.</span></span>  <span data-ttu-id="7c94e-178">Všechny aplikace bude muset požádat o toto oprávnění, pokud je nutné pro přístup k prostředkům jménem uživatele delší dobu dobu, i když uživatel nemusí být používá aktivně aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-178">All apps will need to request this permission if they need to access resources on the behalf of a user for a prolonged period of time, even when the user may not be actively using the app.</span></span>  <span data-ttu-id="7c94e-179">`offline_access` Obor se zobrazí uživateli v dialogových oknech souhlasu jako "Přístup k datům v režimu offline", který uživatel musí vyjádřit souhlas s.</span><span class="sxs-lookup"><span data-stu-id="7c94e-179">The `offline_access` scope will appear to the user in consent dialogs as "Access your data offline", which the user must agree to.</span></span>  <span data-ttu-id="7c94e-180">Požaduje `offline_access` oprávnění umožní vaší webové aplikace na příjem OAuth 2.0 refresh_tokens z koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="7c94e-180">Requesting the `offline_access` permission will enable your web app to receive OAuth 2.0 refresh_tokens from the v2.0 endpoint.</span></span>  <span data-ttu-id="7c94e-181">Refresh_tokens je dlouhodobé a může být vyměňují pro nové access_tokens OAuth 2.0 po delší dobu přístupu.</span><span class="sxs-lookup"><span data-stu-id="7c94e-181">Refresh_tokens are long-lived, and can be exchanged for new OAuth 2.0 access_tokens for extended periods of access.</span></span>  

<span data-ttu-id="7c94e-182">Pokud vaše aplikace neuvede v požadavku `offline_access` oboru, neobdrží refresh_tokens.</span><span class="sxs-lookup"><span data-stu-id="7c94e-182">If your app does not request the `offline_access` scope, it will not receive refresh_tokens.</span></span>  <span data-ttu-id="7c94e-183">To znamená, že po uplatníte authorization_code v toku kódu autorizace OAuth 2.0, se pouze zobrazí zpět access_token z `/token` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="7c94e-183">This means that when you redeem an authorization_code in the OAuth 2.0 authorization code flow, you will only receive back an access_token from the `/token` endpoint.</span></span>  <span data-ttu-id="7c94e-184">Tento access_token zůstane platná, a to krátkou dobu (obvykle jedna hodina), ale nakonec vyprší.</span><span class="sxs-lookup"><span data-stu-id="7c94e-184">That access_token will remain valid for a short period of time (typically one hour), but will eventually expire.</span></span>  <span data-ttu-id="7c94e-185">AT, který bod v čase, vaše aplikace bude potřeba přesměruje uživatele zpět na `/authorize` koncový bod k načtení nové authorization_code.</span><span class="sxs-lookup"><span data-stu-id="7c94e-185">At that point in time, your app will need to redirect the user back to the `/authorize` endpoint to retrieve a new authorization_code.</span></span>  <span data-ttu-id="7c94e-186">Během této přesměrování, uživatel může nebo nemusí být nutné znovu zadat své přihlašovací údaje nebo znovu souhlas oprávnění, v závislosti na typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c94e-186">During this redirect, the user may or may not need to enter their credentials again or re-consent to permissions, depending on the the type of app.</span></span>

<span data-ttu-id="7c94e-187">Další informace o protokolu OAuth 2.0, refresh_tokens a access_tokens, podívejte se [referenci na protokol v2.0](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="7c94e-187">To learn more about OAuth 2.0, refresh_tokens, and access_tokens, check out the [v2.0 protocol reference](active-directory-v2-protocols.md).</span></span>

#### <a name="openid-profile-and-email"></a><span data-ttu-id="7c94e-188">OpenID, profil a e-mailu</span><span class="sxs-lookup"><span data-stu-id="7c94e-188">OpenID, profile and email</span></span>
<span data-ttu-id="7c94e-189">Nejzákladnější tok OpenID Connect přihlášení s Azure Active Directory v minulosti by poskytují množství informací o uživateli ve výsledné požadavku id_token.</span><span class="sxs-lookup"><span data-stu-id="7c94e-189">Historically, the most basic OpenID Connect sign-in flow with Azure Active Directory would provide a wealth of information about the user in the resulting id_token.</span></span>  <span data-ttu-id="7c94e-190">Deklarací identity ve požadavku id_token může obsahovat uživatele název, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.</span><span class="sxs-lookup"><span data-stu-id="7c94e-190">The claims in an id_token can include the user's name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="7c94e-191">Nemůžeme se teď omezení informace, `openid` oboru dává vám přístup aplikace k.</span><span class="sxs-lookup"><span data-stu-id="7c94e-191">We are now restricting the information that the `openid` scope affords your app access to.</span></span>  <span data-ttu-id="7c94e-192">Obor, openid, bude pouze povolit přihlášení uživatele ve vaší aplikaci a přijímat identifikátor specifický pro aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c94e-192">The ‘openid’ scope will only allow your app to sign the user in, and receive an app-specific identifier for the user.</span></span>  <span data-ttu-id="7c94e-193">Pokud chcete získat identifikovatelné osobní údaje (PII) o uživateli ve vaší aplikaci, vaše aplikace bude muset požádat o další oprávnění od uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c94e-193">If you want to obtain personally identifiable information (PII) about the user in your app, your app will need to request additional permissions from the user.</span></span>  <span data-ttu-id="7c94e-194">Představujeme dva nové obory – `email` a `profile` obory – které umožňují učinit.</span><span class="sxs-lookup"><span data-stu-id="7c94e-194">We are introducing two new scopes – the `email` and `profile` scopes – which allow you to do so.</span></span>

<span data-ttu-id="7c94e-195">`email` Obor je velmi jednoduchá – umožňuje vám přístup aplikace k primární e-mailovou adresu uživatele prostřednictvím `email` deklarací identity v požadavku id_token.</span><span class="sxs-lookup"><span data-stu-id="7c94e-195">The `email` scope is very straightforward – it allows your app access to the user’s primary email address via the `email` claim in the id_token.</span></span>  <span data-ttu-id="7c94e-196">`profile` Oboru poskytuje přístup k vaší aplikaci pro všechny ostatní základní informace o uživateli – jejich název, upřednostňované uživatelské jméno, ID objektu a tak dále.</span><span class="sxs-lookup"><span data-stu-id="7c94e-196">The `profile` scope affords your app access to all other basic information about the user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="7c94e-197">To umožňuje kód aplikace způsobem minimální zpřístupnění – můžete požádat uživatele jenom pro sadu informací, že vaše aplikace vyžaduje pro své práci.</span><span class="sxs-lookup"><span data-stu-id="7c94e-197">This allows you to code your app in a minimal-disclosure fashion – you can only ask the user for the set of information that your app requires to do its job.</span></span>  <span data-ttu-id="7c94e-198">Další informace o těchto oborů, najdete v části [odkaz oboru v2.0](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="7c94e-198">For more information on these scopes, refer to [the v2.0 scope reference](active-directory-v2-scopes.md).</span></span>

## <a name="token-claims"></a><span data-ttu-id="7c94e-199">Token deklarací identity</span><span class="sxs-lookup"><span data-stu-id="7c94e-199">Token Claims</span></span>
<span data-ttu-id="7c94e-200">Deklarace identity v tokenech vystavený koncového bodu v2.0 nebudou shodovat s tokeny vydané obecně dostupná koncové body Azure AD – aplikace migraci na nové služby by neměl předpokládá konkrétní deklaraci identity bude existovat v id_tokens nebo access_tokens.</span><span class="sxs-lookup"><span data-stu-id="7c94e-200">The claims in tokens issued by the v2.0 endpoint will not be identical to tokens issued by the generally available Azure AD endpoints - apps migrating to the new service should not assume a particular claim will exist in id_tokens or access_tokens.</span></span> <span data-ttu-id="7c94e-201">Další informace o konkrétní vygenerované v v2.0 tokeny deklarací identity najdete v tématu [odkaz tokenu v2.0](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="7c94e-201">To learn about the specific claims emitted in v2.0 tokens, see the [v2.0 token reference](active-directory-v2-tokens.md).</span></span>

## <a name="limitations"></a><span data-ttu-id="7c94e-202">Omezení</span><span class="sxs-lookup"><span data-stu-id="7c94e-202">Limitations</span></span>
<span data-ttu-id="7c94e-203">Existuje několik omezení, která má být vědět, když používáte bod v2.0.</span><span class="sxs-lookup"><span data-stu-id="7c94e-203">There are a few restrictions to be aware of when using the v2.0 point.</span></span>  <span data-ttu-id="7c94e-204">Naleznete [v2.0 omezení doc](active-directory-v2-limitations.md) chcete zobrazit, pokud žádné z těchto omezení platí pro konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="7c94e-204">Please refer to the [v2.0 limitations doc](active-directory-v2-limitations.md) to see if any of these restrictions apply to your particular scenario.</span></span>