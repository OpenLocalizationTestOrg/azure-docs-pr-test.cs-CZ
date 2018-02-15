---
title: "Azure Active Directory B2C: Předdefinované zásady | Microsoft Docs"
description: "Téma na rozhraní rozšiřitelných zásad služby Azure Active Directory B2C a o tom, jak vytvořit různé typy zásad"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mtillman
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: f0aa3d19e15837b75888293f0cd19683b7621a6a
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-built-in-policies"></a><span data-ttu-id="75564-103">Azure Active Directory B2C: Předdefinované zásady</span><span class="sxs-lookup"><span data-stu-id="75564-103">Azure Active Directory B2C: Built-in policies</span></span>


<span data-ttu-id="75564-104">Rozhraní rozšiřitelných zásad služby Azure Active Directory (Azure AD) B2C je základní sílu služby.</span><span class="sxs-lookup"><span data-stu-id="75564-104">The extensible policy framework of Azure Active Directory (Azure AD) B2C is the core strength of the service.</span></span> <span data-ttu-id="75564-105">Zásady, jako plně popisují činnosti identity příjemce registrace, přihlášení nebo úpravy profilu.</span><span class="sxs-lookup"><span data-stu-id="75564-105">Policies fully describe consumer identity experiences such as sign-up, sign-in, or profile editing.</span></span> <span data-ttu-id="75564-106">Například registrační zásadě můžete řídit chování konfigurací následujícího nastavení:</span><span class="sxs-lookup"><span data-stu-id="75564-106">For instance, a sign-up policy allows you to control behaviors by configuring the following settings:</span></span>

* <span data-ttu-id="75564-107">Typy účtů (jako je Facebook sociálních účty) nebo místní účty například e-mailové adresy, které mohou příjemci použít se přihlásit k aplikaci</span><span class="sxs-lookup"><span data-stu-id="75564-107">Account types (social accounts such as Facebook or local accounts such as email addresses) that consumers can use to sign up for the application</span></span>
* <span data-ttu-id="75564-108">Atributy (například křestní jméno, poštovní směrovací číslo a velikosti), které se mají shromažďovat od příjemce během registrace</span><span class="sxs-lookup"><span data-stu-id="75564-108">Attributes (for example, first name, postal code, and shoe size) to be collected from the consumer during sign-up</span></span>
* <span data-ttu-id="75564-109">Použití Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="75564-109">Use of Azure Multi-Factor Authentication</span></span>
* <span data-ttu-id="75564-110">Vzhledu a chování registrace všechny stránky</span><span class="sxs-lookup"><span data-stu-id="75564-110">The look and feel of all sign-up pages</span></span>
* <span data-ttu-id="75564-111">Informace o (manifesty jako deklarace do tokenu), aplikace obdrží, kdy zásady spustit dokončení</span><span class="sxs-lookup"><span data-stu-id="75564-111">Information (which manifests as claims in a token) that the application receives when the policy run finishes</span></span>

<span data-ttu-id="75564-112">Můžete vytvořit několik zásad různých typů ve vašem klientovi a podle potřeby používat ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="75564-112">You can create multiple policies of different types in your tenant and use them in your applications as needed.</span></span> <span data-ttu-id="75564-113">Zásady můžete opětovně použít napříč aplikací.</span><span class="sxs-lookup"><span data-stu-id="75564-113">Policies can be reused across applications.</span></span> <span data-ttu-id="75564-114">Tato možnost umožňuje vývojářům definování a úprava činnosti identity uživatelů s minimální nebo žádný změny svůj kód.</span><span class="sxs-lookup"><span data-stu-id="75564-114">This flexibility enables developers to define and modify consumer identity experiences with minimal or no changes to their code.</span></span>

<span data-ttu-id="75564-115">Zásady jsou k dispozici pro použití prostřednictvím rozhraní jednoduché developer.</span><span class="sxs-lookup"><span data-stu-id="75564-115">Policies are available for use via a simple developer interface.</span></span> <span data-ttu-id="75564-116">Vaše aplikace aktivuje zásadu pomocí standardní požadavek ověřování HTTP (předání parametru zásad v žádosti o) a obdrží token přizpůsobené jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="75564-116">Your application triggers a policy by using a standard HTTP authentication request (passing a policy parameter in the request) and receives a customized token as response.</span></span> <span data-ttu-id="75564-117">Jediným rozdílem mezi požadavky, které vyvolají registrační zásadě a požadavky, které vyvolají zásad přihlašování je například název zásady, který se používá v parametru řetězce dotazu "p":</span><span class="sxs-lookup"><span data-stu-id="75564-117">For example, the only difference between requests that invoke a sign-up policy and requests that invoke a sign-in policy is the policy name that's used in the "p" query string parameter:</span></span>

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

<span data-ttu-id="75564-118">Další informace o rozhraní zásad najdete v tématu [tomto příspěvku na blogu o Azure AD B2C na blogu Enterprise Mobility and Security Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span><span class="sxs-lookup"><span data-stu-id="75564-118">For more information about the policy framework, see [this blog post about Azure AD B2C on the Enterprise Mobility and Security Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span></span>

## <a name="create-a-sign-up-or-sign-in-policy"></a><span data-ttu-id="75564-119">Vytvoření zásady registrace nebo přihlášení</span><span class="sxs-lookup"><span data-stu-id="75564-119">Create a sign-up or sign-in policy</span></span>

<span data-ttu-id="75564-120">Tato zásada zpracuje obě příjemce s konfigurací jedné možnosti registrace a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="75564-120">This policy handles both consumer sign-up & sign-in experiences with a single configuration.</span></span> <span data-ttu-id="75564-121">Příjemci knihovny jsou vedla dolů správné cesty (registrace nebo přihlášení) v závislosti na kontextu.</span><span class="sxs-lookup"><span data-stu-id="75564-121">Consumers are led down the right path (sign-up or sign-in) depending on the context.</span></span> <span data-ttu-id="75564-122">Také popisuje obsah tokeny, které aplikace se zobrazí po úspěšné zápisy nebo přihlášení.  Ukázka kódu pro zásady registrace nebo přihlášení je [k dispozici zde](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="75564-122">It also describes the contents of tokens that the application will receive upon successful sign-ups or sign-ins.  A code sample for the sign-up or sign-in policy is [available here](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>  <span data-ttu-id="75564-123">Je doporučeno použití této zásady prostřednictvím zásad přihlašování a registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="75564-123">It is recommened that you use this policy over a sign-up policy and sign-in policy.</span></span>  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a><span data-ttu-id="75564-124">Vytvořit zásadu registrace</span><span class="sxs-lookup"><span data-stu-id="75564-124">Create a sign-up policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a><span data-ttu-id="75564-125">Vytvoření zásady přihlášení</span><span class="sxs-lookup"><span data-stu-id="75564-125">Create a sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a><span data-ttu-id="75564-126">Vytvoření profilu úpravy zásad</span><span class="sxs-lookup"><span data-stu-id="75564-126">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a><span data-ttu-id="75564-127">Vytvoření zásady resetování hesel</span><span class="sxs-lookup"><span data-stu-id="75564-127">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a><span data-ttu-id="75564-128">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="75564-128">Frequently asked questions</span></span>

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a><span data-ttu-id="75564-129">Jak lze propojit zásady registrace nebo přihlášení se zásady resetování hesel?</span><span class="sxs-lookup"><span data-stu-id="75564-129">How do I link a sign-up or sign-in policy with a password reset policy?</span></span>
<span data-ttu-id="75564-130">Při vytváření zásad registrace nebo přihlášení (s místní účty), uvidíte **zapomněli jste heslo?** odkaz na první stránce prostředí.</span><span class="sxs-lookup"><span data-stu-id="75564-130">When you create a sign-up or sign-in policy (with local accounts), you see a **Forgot password?** link on the first page of the experience.</span></span> <span data-ttu-id="75564-131">Kliknutím na tento odkaz není automaticky aktivační událost heslo resetovat zásady.</span><span class="sxs-lookup"><span data-stu-id="75564-131">Clicking this link doesn't automatically trigger a password reset policy.</span></span> 

<span data-ttu-id="75564-132">Místo toho kód chyby  **`AADB2C90118`**  se vrátí do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="75564-132">Instead, the error code **`AADB2C90118`** is returned to your app.</span></span> <span data-ttu-id="75564-133">Aplikace je potřeba zpracovat tomto kódu chyby vyvoláním zásad resetování hesel konkrétní.</span><span class="sxs-lookup"><span data-stu-id="75564-133">Your app needs to handle this error code by invoking a specific password reset policy.</span></span> <span data-ttu-id="75564-134">Další informace najdete v tématu [vzorku, který ukazuje přístup propojení zásad](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span><span class="sxs-lookup"><span data-stu-id="75564-134">For more information, see a [sample that demonstrates the approach of linking policies](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span></span>

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a><span data-ttu-id="75564-135">Použít zásadu registrace nebo přihlášení nebo registraci zásady a zásady přihlášení?</span><span class="sxs-lookup"><span data-stu-id="75564-135">Should I use a sign-up or sign-in policy or a sign-up policy and a sign-in policy?</span></span>
<span data-ttu-id="75564-136">Doporučujeme použití registrace nebo přihlášení zásady prostřednictvím registrace zásad a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="75564-136">We recommend that you use a sign-up or sign-in policy over a sign-up policy and a sign-in policy.</span></span>  

<span data-ttu-id="75564-137">Zásady registrace nebo přihlášení k dispozici další možnosti než zásady přihlášení.</span><span class="sxs-lookup"><span data-stu-id="75564-137">The sign-up or sign-in policy has more capabilities than the sign-in policy.</span></span> <span data-ttu-id="75564-138">Také umožňuje používat přizpůsobení uživatelského rozhraní stránky a má lepší podporu pro lokalizaci.</span><span class="sxs-lookup"><span data-stu-id="75564-138">It also enables you to use page UI customization and has better support for localization.</span></span> 

<span data-ttu-id="75564-139">Zásady přihlášení se doporučuje, pokud nepotřebujete k lokalizaci zásad, pouze nutnost branding schopnosti menší přizpůsobení a mají heslo resetovat integrovaný do ní.</span><span class="sxs-lookup"><span data-stu-id="75564-139">The sign-in policy is recommended if you don't need to localize your policies, only need minor customization capabilities for branding, and want password reset built into it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75564-140">Další postup</span><span class="sxs-lookup"><span data-stu-id="75564-140">Next steps</span></span>
* [<span data-ttu-id="75564-141">Token, relace a jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="75564-141">Token, session, and single sign-on configuration</span></span>](active-directory-b2c-token-session-sso.md)
* [<span data-ttu-id="75564-142">Zakázání e-mailu ověření během registrace příjemce</span><span class="sxs-lookup"><span data-stu-id="75564-142">Disable email verification during consumer sign-up</span></span>](active-directory-b2c-reference-disable-ev.md)

