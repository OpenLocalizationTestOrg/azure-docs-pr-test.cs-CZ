---
title: "Přehled – Azure AD B2C | Dokumentace Microsoftu"
description: "Vývoj aplikací určených uživatelům pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeedakhter-msft
ms.openlocfilehash: 44d5d31d49c375c802a67511d1f962df20656559
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a><span data-ttu-id="d1fb5-103">Azure AD B2C: Soustřeďte se na svoji aplikaci, o registraci a přihlašování se postaráme my</span><span class="sxs-lookup"><span data-stu-id="d1fb5-103">Azure AD B2C: Focus on your app, let us worry about sign-up and sign-in</span></span>

<span data-ttu-id="d1fb5-104">Azure AD B2C je cloudové řešení správy identit pro webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-104">Azure AD B2C is a cloud identity management solution for your web and mobile applications.</span></span> <span data-ttu-id="d1fb5-105">Jedná se o vysoce dostupnou globální službu, která je škálovatelná pro stovky milionů identit.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-105">It is a highly available global service that scales to hundreds of millions of identities.</span></span> <span data-ttu-id="d1fb5-106">Řešení Azure AD B2C je postavené na platformě zabezpečené na podnikové úrovni a chrání tak vaše aplikace, firmu i zákazníky.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-106">Built on an enterprise-grade secure platform, Azure AD B2C keeps your applications, your business, and your customers protected.</span></span>

<span data-ttu-id="d1fb5-107">Při minimální konfiguraci řešení Azure AD B2C umožňuje vaší aplikaci ověřovat tyto typy účtů:</span><span class="sxs-lookup"><span data-stu-id="d1fb5-107">With minimal configuration, Azure AD B2C enables your application to authenticate:</span></span>

* <span data-ttu-id="d1fb5-108">**Sociální účty** (jako je Facebook, Google, LinkedIn a další)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-108">**Social Accounts** (such as Facebook, Google, LinkedIn, and more)</span></span>
* <span data-ttu-id="d1fb5-109">**Účty organizací** (využívající protokoly s otevřenými standardy, OpenID Connect nebo SAML)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-109">**Enterprise Accounts** (using open standard protocols, OpenID Connect or SAML)</span></span>
* <span data-ttu-id="d1fb5-110">**Místní účty** (e-mailová adresa a heslo nebo uživatelské jméno a heslo)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-110">**Local Accounts** (email address and password, or username and password)</span></span>

## <a name="get-started"></a><span data-ttu-id="d1fb5-111">Začínáme</span><span class="sxs-lookup"><span data-stu-id="d1fb5-111">Get started</span></span>

<span data-ttu-id="d1fb5-112">Nejdřív pomocí návodu v tématu [Vytvoření tenanta Azure AD B2C](active-directory-b2c-get-started.md) získejte vlastního tenanta.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-112">First, get your own tenant by using the steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="d1fb5-113">Zvolte svůj scénář vývoje aplikací:</span><span class="sxs-lookup"><span data-stu-id="d1fb5-113">Then choose your application development scenario:</span></span>

|  |  |  |  |
| --- | --- | --- | --- |
| <span data-ttu-id="d1fb5-114"><center>![Mobilní aplikace a aplikace počítače](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-114"><center>![Mobile & Desktop Apps](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)</span></span><br /><span data-ttu-id="d1fb5-115">Mobilní aplikace a aplikace počítače</center></span><span class="sxs-lookup"><span data-stu-id="d1fb5-115">Mobile & Desktop Apps</center></span></span> | <span data-ttu-id="d1fb5-116">[Přehled](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="d1fb5-116">[Overview](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span><br /><br />[<span data-ttu-id="d1fb5-117">iOS</span><span class="sxs-lookup"><span data-stu-id="d1fb5-117">iOS</span></span>](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[<span data-ttu-id="d1fb5-118">Android</span><span class="sxs-lookup"><span data-stu-id="d1fb5-118">Android</span></span>](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [<span data-ttu-id="d1fb5-119">.NET</span><span class="sxs-lookup"><span data-stu-id="d1fb5-119">.NET</span></span>](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[<span data-ttu-id="d1fb5-120">Xamarin</span><span class="sxs-lookup"><span data-stu-id="d1fb5-120">Xamarin</span></span>](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <span data-ttu-id="d1fb5-121"><center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-121"><center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)</span></span><br /><span data-ttu-id="d1fb5-122">Web Apps</center></span><span class="sxs-lookup"><span data-stu-id="d1fb5-122">Web Apps</center></span></span> | [<span data-ttu-id="d1fb5-123">Přehled</span><span class="sxs-lookup"><span data-stu-id="d1fb5-123">Overview</span></span>](active-directory-b2c-reference-oidc.md)<br /><br />[<span data-ttu-id="d1fb5-124">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d1fb5-124">ASP.NET</span></span>](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[<span data-ttu-id="d1fb5-125">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d1fb5-125">ASP.NET Core</span></span>](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [<span data-ttu-id="d1fb5-126">Node.js</span><span class="sxs-lookup"><span data-stu-id="d1fb5-126">Node.js</span></span>](active-directory-b2c-devquickstarts-web-node.md) |  |
| <span data-ttu-id="d1fb5-127"><center>![Jednostránkové aplikace](../active-directory/develop/media/active-directory-developers-guide/SPA.png)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-127"><center>![Single Page Apps](../active-directory/develop/media/active-directory-developers-guide/SPA.png)</span></span><br /><span data-ttu-id="d1fb5-128">Jednostránkové aplikace</center></span><span class="sxs-lookup"><span data-stu-id="d1fb5-128">Single Page Apps</center></span></span> | [<span data-ttu-id="d1fb5-129">Přehled</span><span class="sxs-lookup"><span data-stu-id="d1fb5-129">Overview</span></span>](active-directory-b2c-reference-spa.md)<br /><br />[<span data-ttu-id="d1fb5-130">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d1fb5-130">JavaScript</span></span>](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <span data-ttu-id="d1fb5-131"><center>![Webová rozhraní API](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)</span><span class="sxs-lookup"><span data-stu-id="d1fb5-131"><center>![Web APIs](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)</span></span><br /><span data-ttu-id="d1fb5-132">Webová rozhraní API</center></span><span class="sxs-lookup"><span data-stu-id="d1fb5-132">Web APIs</center></span></span> | [<span data-ttu-id="d1fb5-133">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d1fb5-133">ASP.NET</span></span>](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [<span data-ttu-id="d1fb5-134">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d1fb5-134">ASP.NET Core</span></span>](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [<span data-ttu-id="d1fb5-135">Node.js</span><span class="sxs-lookup"><span data-stu-id="d1fb5-135">Node.js</span></span>](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [<span data-ttu-id="d1fb5-136">Volání webového rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="d1fb5-136">Call a .NET Web API</span></span>](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a><span data-ttu-id="d1fb5-137">Co je nového</span><span class="sxs-lookup"><span data-stu-id="d1fb5-137">What's new</span></span>

<span data-ttu-id="d1fb5-138">Pravidelně kontrolujte tuto stránku, abyste se dovědět o budoucích změnách Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-138">Check back here often to learn about future changes to the Azure Active Directory B2C.</span></span> <span data-ttu-id="d1fb5-139">O všech aktualizacích také tweetujeme na @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-139">We also tweet about any updates by using @AzureAD.</span></span>

* <span data-ttu-id="d1fb5-140">Vedle funkce Předdefinované zásady (všeobecná dostupnost) je teď ve veřejné verzi Preview dostupná funkce [Vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-140">In addition to "Built-in Policies" (General Availability), the ["Custom Policies"](active-directory-b2c-overview-custom.md) feature is now available in public preview.</span></span>  <span data-ttu-id="d1fb5-141">Vlastní zásady jsou pro profesionály pracující identitami, kteří potřebují mít kontrolu nad složením jejich prostředí identit.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-141">Custom policies are for identity pros that need control over the composition of their identity experience.</span></span>
* <span data-ttu-id="d1fb5-142">Ve veřejné verzi Preview je teď dostupná funkce [Přístupový token](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-142">The [Access Token](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) feature is now available in public preview.</span></span>
* <span data-ttu-id="d1fb5-143">Proběhlo oznámení [všeobecné dostupnosti služby Azure AD B2C pro Evropu](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-143">[General Availability of Europe-based Azure AD B2C](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/) directories has been announced.</span></span>
* <span data-ttu-id="d1fb5-144">Projděte si naši rostoucí knihovnu [ukázek kódů na GitHubu](https://github.com/Azure-Samples?q=b2c)!</span><span class="sxs-lookup"><span data-stu-id="d1fb5-144">Check out our growing library of [code samples on Github](https://github.com/Azure-Samples?q=b2c)!</span></span>

## <a name="how-to-articles"></a><span data-ttu-id="d1fb5-145">Články s návody</span><span class="sxs-lookup"><span data-stu-id="d1fb5-145">How-to articles</span></span>

<span data-ttu-id="d1fb5-146">Další informace o použití konkrétních funkcí Azure Active Directory B2C:</span><span class="sxs-lookup"><span data-stu-id="d1fb5-146">Learn how to use specific Azure Active Directory B2C features:</span></span>

* <span data-ttu-id="d1fb5-147">Nastavení [Facebooku](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [účtu Microsoft](active-directory-b2c-setup-msa-app.md), účtů [Amazon](active-directory-b2c-setup-amzn-app.md), a [inkedIn](active-directory-b2c-setup-li-app.md) pro použití v aplikacích určených uživatelům.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-147">Configure [Facebook](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [Microsoft account](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md), and [LinkedIn](active-directory-b2c-setup-li-app.md) accounts for use in your consumer-facing applications.</span></span>
* <span data-ttu-id="d1fb5-148">[Použití vlastních atributů ke sběru informací o uživatelích](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-148">[Use custom attributes to collect information about your consumers](active-directory-b2c-reference-custom-attr.md).</span></span>
* <span data-ttu-id="d1fb5-149">[Povolení Azure Multi-Factor Authentication v aplikacích určených uživatelům](active-directory-b2c-reference-mfa.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-149">[Enable Azure Multi-Factor Authentication in your consumer-facing applications](active-directory-b2c-reference-mfa.md).</span></span>
* <span data-ttu-id="d1fb5-150">[Nastavení samoobslužného resetování hesla pro uživatele](active-directory-b2c-reference-sspr.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-150">[Set up self-service password reset for your consumers](active-directory-b2c-reference-sspr.md).</span></span>
* <span data-ttu-id="d1fb5-151">[Přizpůsobení vzhledu a chování registrace a přihlášení a dalších stránek určených uživatelům](active-directory-b2c-reference-ui-customization.md), které obsluhuje Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-151">[Customize the look and feel of sign-up, sign in, and other consumer-facing pages](active-directory-b2c-reference-ui-customization.md) that are served by Azure Active Directory B2C.</span></span>
* <span data-ttu-id="d1fb5-152">[Využijte Azure Active Directory Graph API k programovému vytváření, čtení, aktualizaci a mazání uživatelů](active-directory-b2c-devquickstarts-graph-dotnet.md) ve svém klientu Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-152">[Use the Azure Active Directory Graph API to programmatically create, read, update, and delete consumers](active-directory-b2c-devquickstarts-graph-dotnet.md) in your Azure Active Directory B2C tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1fb5-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1fb5-153">Next steps</span></span>

<span data-ttu-id="d1fb5-154">Následující odkazy jsou užitečné při prozkoumávání služby do hloubky:</span><span class="sxs-lookup"><span data-stu-id="d1fb5-154">These links are useful for exploring the service in depth:</span></span>

* <span data-ttu-id="d1fb5-155">Viz [Informace o cenách Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-155">See the [Azure Active Directory B2C pricing information](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span></span>
* <span data-ttu-id="d1fb5-156">Projděte si naše [ukázky kódu](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) pro Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-156">Review our [code samples](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) for Azure Active Directory B2C.</span></span> 
* <span data-ttu-id="d1fb5-157">Získejte pomoc na Stack Overflow pomocí značky [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-157">Get help on Stack Overflow by using the [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c) tag.</span></span>
* <span data-ttu-id="d1fb5-158">Sdělte nám své myšlenky pomocí [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) chceme je slyšet!</span><span class="sxs-lookup"><span data-stu-id="d1fb5-158">Give us your thoughts by using [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c), we want to hear them!</span></span>
* <span data-ttu-id="d1fb5-159">Přečtěte si [Referenci protokolu Azure AD B2C](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-159">Review the [Azure AD B2C Protocol Reference](active-directory-b2c-reference-protocols.md).</span></span>
* <span data-ttu-id="d1fb5-160">Přečtěte si [Referenci tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-160">Review the [Azure AD B2C Token Reference](active-directory-b2c-reference-tokens.md).</span></span>
* <span data-ttu-id="d1fb5-161">Přečtěte si [Nejčastější dotazy k Azure Active Directory B2C](active-directory-b2c-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-161">Read the [Azure Active Directory B2C FAQs](active-directory-b2c-faqs.md).</span></span>
* <span data-ttu-id="d1fb5-162">[Podávejte požadavky na podporu pro Azure Active Directory B2C](active-directory-b2c-support.md).</span><span class="sxs-lookup"><span data-stu-id="d1fb5-162">[File support requests for Azure Active Directory B2C](active-directory-b2c-support.md).</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="d1fb5-163">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="d1fb5-163">Get security updates for our products</span></span>

<span data-ttu-id="d1fb5-164">Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="d1fb5-164">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>
