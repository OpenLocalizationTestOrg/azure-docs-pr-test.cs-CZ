---
title: "Podepisování výměna klíče ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje podpisového klíče výměny osvědčené postupy pro Azure Active Directory"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 228bb9058537af1e4eb38207c376c2eb86aee68c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a><span data-ttu-id="650d1-103">Podepisování výměna klíče v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="650d1-103">Signing key rollover in Azure Active Directory</span></span>
<span data-ttu-id="650d1-104">Toto téma popisuje, co potřebujete vědět o veřejné klíče, které se používají v Azure Active Directory (Azure AD) k podepisování tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="650d1-104">This topic discusses what you need to know about the public keys that are used in Azure Active Directory (Azure AD) to sign security tokens.</span></span> <span data-ttu-id="650d1-105">Je důležité si uvědomit, že tyto výměny klíčů v pravidelných intervalech a v případě nouze, může být převracet okamžitě.</span><span class="sxs-lookup"><span data-stu-id="650d1-105">It is important to note that these keys rollover on a periodic basis and, in an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="650d1-106">Všechny aplikace, které používají Azure AD by mohli prostřednictvím kódu programu zpracování procesu výměna klíče nebo vytvořit proces periodické ruční výměny.</span><span class="sxs-lookup"><span data-stu-id="650d1-106">All applications that use Azure AD should be able to programmatically handle the key rollover process or establish a periodic manual rollover process.</span></span> <span data-ttu-id="650d1-107">Materiály, abyste pochopili, jak funguje klíče, jak posoudit dopad výměny do vaší aplikace a jak aktualizovat aplikaci nebo vytvořit proces periodické ruční výměna pro zpracování výměny klíčů, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="650d1-107">Continue reading to understand how the keys work, how to assess the impact of the rollover to your application and how to update your application or establish a periodic manual rollover process to handle key rollover if necessary.</span></span>

## <a name="overview-of-signing-keys-in-azure-ad"></a><span data-ttu-id="650d1-108">Přehled podpisových klíčů ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="650d1-108">Overview of signing keys in Azure AD</span></span>
<span data-ttu-id="650d1-109">Azure AD používá šifrování veřejného klíče založený na oborových standardů k navázání vztahu důvěryhodnosti mezi samostatně a aplikace, které ho používají.</span><span class="sxs-lookup"><span data-stu-id="650d1-109">Azure AD uses public-key cryptography built on industry standards to establish trust between itself and the applications that use it.</span></span> <span data-ttu-id="650d1-110">V praxi, tento postup funguje následujícím způsobem: Azure AD používá podpisový klíč, který se skládá z pár veřejného a privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="650d1-110">In practical terms, this works in the following way: Azure AD uses a signing key that consists of a public and private key pair.</span></span> <span data-ttu-id="650d1-111">Když se uživatel přihlásí aplikaci, která používá Azure AD pro ověřování, Azure AD vytvoří token zabezpečení, který obsahuje informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="650d1-111">When a user signs in to an application that uses Azure AD for authentication, Azure AD creates a security token that contains information about the user.</span></span> <span data-ttu-id="650d1-112">Tento token je podepsána pomocí jeho privátní klíč, než budou odeslána zpět do aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="650d1-112">This token is signed by Azure AD using its private key before it is sent back to the application.</span></span> <span data-ttu-id="650d1-113">Pokud chcete ověřit, zda je token platný a ve skutečnosti původu z Azure AD, musí aplikace ověření podpisu tokenu pomocí veřejný klíč vystavený službou Azure AD, která je součástí klienta [OpenID Connect dokumentu zjišťování](http://openid.net/specs/openid-connect-discovery-1_0.html) nebo SAML/WS-Fed [dokument federačních metadat](active-directory-federation-metadata.md).</span><span class="sxs-lookup"><span data-stu-id="650d1-113">To verify that the token is valid and actually originated from Azure AD, the application must validate the token’s signature using the public key exposed by Azure AD that is contained in the tenant’s [OpenID Connect discovery document](http://openid.net/specs/openid-connect-discovery-1_0.html) or SAML/WS-Fed [federation metadata document](active-directory-federation-metadata.md).</span></span>

<span data-ttu-id="650d1-114">Z bezpečnostních důvodů Azure AD podpisového klíče zobrazí souhrn v pravidelných intervalech a v případě nouze, může být převracet okamžitě.</span><span class="sxs-lookup"><span data-stu-id="650d1-114">For security purposes, Azure AD’s signing key rolls on a periodic basis and, in the case of an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="650d1-115">Všechny aplikace, který se integruje s Azure AD měli připravit zpracování události výměna klíče bez ohledu na to, jak často může dojít.</span><span class="sxs-lookup"><span data-stu-id="650d1-115">Any application that integrates with Azure AD should be prepared to handle a key rollover event no matter how frequently it may occur.</span></span> <span data-ttu-id="650d1-116">Pokud tomu tak není, a aplikace se pokusí použít vypršela platnost klíč k ověření podpisu na token, požádat o přihlášení selže.</span><span class="sxs-lookup"><span data-stu-id="650d1-116">If it doesn’t, and your application attempts to use an expired key to verify the signature on a token, the sign-in request will fail.</span></span>

<span data-ttu-id="650d1-117">Vždy není k dispozici v dokumentu OpenID Connect zjišťování a dokument federačních metadat více než jeden platný klíč.</span><span class="sxs-lookup"><span data-stu-id="650d1-117">There is always more than one valid key available in the OpenID Connect discovery document and the federation metadata document.</span></span> <span data-ttu-id="650d1-118">Aplikace by měly být připravené k používání některé z klíče určené v dokumentu, vzhledem k tomu, že jeden klíč může být vrácena brzy, jiné může být jeho nahrazení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="650d1-118">Your application should be prepared to use any of the keys specified in the document, since one key may be rolled soon, another may be its replacement, and so forth.</span></span>

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a><span data-ttu-id="650d1-119">Postup vyhodnocení, pokud vaše aplikace bude mít vliv na a co dělat, o něm</span><span class="sxs-lookup"><span data-stu-id="650d1-119">How to assess if your application will be affected and what to do about it</span></span>
<span data-ttu-id="650d1-120">Jak vaše aplikace zpracovává výměny klíčů, závisí na proměnné, jako je typ aplikace nebo jaké protokol identity a knihovna byl použit.</span><span class="sxs-lookup"><span data-stu-id="650d1-120">How your application handles key rollover depends on variables such as the type of application or what identity protocol and library was used.</span></span> <span data-ttu-id="650d1-121">V níže uvedených částech vyhodnocení, zda nejběžnějších typů aplikací je postiženo výměny klíčů a poskytují pokyny o tom, jak aktualizovat aplikaci pro podporu automatického výměny nebo ručně aktualizovat klíč.</span><span class="sxs-lookup"><span data-stu-id="650d1-121">The sections below assess whether the most common types of applications are impacted by the key rollover and provide guidance on how to update the application to support automatic rollover or manually update the key.</span></span>

* [<span data-ttu-id="650d1-122">Nativní klientské aplikace přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="650d1-122">Native client applications accessing resources</span></span>](#nativeclient)
* [<span data-ttu-id="650d1-123">Webové aplikace / rozhraní API pro přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="650d1-123">Web applications / APIs accessing resources</span></span>](#webclient)
* [<span data-ttu-id="650d1-124">Webové aplikace / rozhraní API Ochrana prostředků a vyvíjené v Azure App Services</span><span class="sxs-lookup"><span data-stu-id="650d1-124">Web applications / APIs protecting resources and built using Azure App Services</span></span>](#appservices)
* [<span data-ttu-id="650d1-125">Webové aplikace / Ochrana prostředků pomocí rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="650d1-125">Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>](#owin)
* [<span data-ttu-id="650d1-126">Webové aplikace / Ochrana prostředků pomocí rozhraní .NET Core OpenID Connect nebo JwtBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="650d1-126">Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>](#owincore)
* [<span data-ttu-id="650d1-127">Webové aplikace / rozhraní API Ochrana prostředků pomocí modulu passport-azure-ad Node.js</span><span class="sxs-lookup"><span data-stu-id="650d1-127">Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>](#passport)
* [<span data-ttu-id="650d1-128">Webové aplikace / rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="650d1-128">Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>](#vs2015)
* [<span data-ttu-id="650d1-129">Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="650d1-129">Web applications protecting resources and created with Visual Studio 2013</span></span>](#vs2013)
* [<span data-ttu-id="650d1-130">Webová rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="650d1-130">Web APIs protecting resources and created with Visual Studio 2013</span></span>](#vs2013_webapi)
* [<span data-ttu-id="650d1-131">Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="650d1-131">Web applications protecting resources and created with Visual Studio 2012</span></span>](#vs2012)
* [<span data-ttu-id="650d1-132">Webové aplikace Ochrana prostředků a vytvořit s Visual Studio 2010, o 2008 pomocí technologie Windows Identity Foundation</span><span class="sxs-lookup"><span data-stu-id="650d1-132">Web applications protecting resources and created with Visual Studio 2010, 2008 o using Windows Identity Foundation</span></span>](#vs2010)
* [<span data-ttu-id="650d1-133">Webové aplikace / Ochrana prostředků pomocí jiné knihovny nebo některé z podporovaných protokolů ručně implementace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="650d1-133">Web applications / APIs protecting resources using any other libraries or manually implementing any of the supported protocols</span></span>](#other)

<span data-ttu-id="650d1-134">V tomto návodu je **není** platí pro:</span><span class="sxs-lookup"><span data-stu-id="650d1-134">This guidance is **not** applicable for:</span></span>

* <span data-ttu-id="650d1-135">Aplikace přidána z Azure AD Application Gallery (včetně vlastní) mají zvláštní pokyny s ohledem na podpisových klíčů.</span><span class="sxs-lookup"><span data-stu-id="650d1-135">Applications added from Azure AD Application Gallery (including Custom) have separate guidance with regards to signing keys.</span></span> [<span data-ttu-id="650d1-136">Další informace.</span><span class="sxs-lookup"><span data-stu-id="650d1-136">More information.</span></span>](../active-directory-sso-certs.md)
* <span data-ttu-id="650d1-137">Místní aplikacích publikovaných prostřednictvím proxy aplikace nemusíte si dělat starosti o podpisových klíčů.</span><span class="sxs-lookup"><span data-stu-id="650d1-137">On-premises applications published via application proxy don't have to worry about signing keys.</span></span>

### <span data-ttu-id="650d1-138"><a name="nativeclient"></a>Nativní klientské aplikace přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="650d1-138"><a name="nativeclient"></a>Native client applications accessing resources</span></span>
<span data-ttu-id="650d1-139">Aplikace, které jsou pouze přístup k prostředkům (tj</span><span class="sxs-lookup"><span data-stu-id="650d1-139">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="650d1-140">Microsoft Graph, KeyVault, rozhraní API aplikace Outlook a další APIs Microsoft) obecně pouze získat token a jejich předávání podél vlastníka prostředku.</span><span class="sxs-lookup"><span data-stu-id="650d1-140">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along to the resource owner.</span></span> <span data-ttu-id="650d1-141">Vzhledem k tomu, že se nechrání žádné prostředky, není zkontrolujte token a proto není nutné, aby Ujistěte se, že je správně podepsaný.</span><span class="sxs-lookup"><span data-stu-id="650d1-141">Given that they are not protecting any resources, they do not inspect the token and therefore do not need to ensure it is properly signed.</span></span>

<span data-ttu-id="650d1-142">Nativní klientské aplikace, zda desktop či mobile, do této kategorie patří a nejsou proto vliv výměny.</span><span class="sxs-lookup"><span data-stu-id="650d1-142">Native client applications, whether desktop or mobile, fall into this category and are thus not impacted by the rollover.</span></span>

### <span data-ttu-id="650d1-143"><a name="webclient"></a>Webové aplikace / rozhraní API pro přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="650d1-143"><a name="webclient"></a>Web applications / APIs accessing resources</span></span>
<span data-ttu-id="650d1-144">Aplikace, které jsou pouze přístup k prostředkům (tj</span><span class="sxs-lookup"><span data-stu-id="650d1-144">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="650d1-145">Microsoft Graph, KeyVault, rozhraní API aplikace Outlook a další APIs Microsoft) obecně pouze získat token a jejich předávání podél vlastníka prostředku.</span><span class="sxs-lookup"><span data-stu-id="650d1-145">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along to the resource owner.</span></span> <span data-ttu-id="650d1-146">Vzhledem k tomu, že se nechrání žádné prostředky, není zkontrolujte token a proto není nutné, aby Ujistěte se, že je správně podepsaný.</span><span class="sxs-lookup"><span data-stu-id="650d1-146">Given that they are not protecting any resources, they do not inspect the token and therefore do not need to ensure it is properly signed.</span></span>

<span data-ttu-id="650d1-147">Webové aplikace a webové rozhraní API, která používají toku jen aplikace (pověření klienta nebo certifikát klienta), do této kategorie patří a nejsou proto vliv výměny.</span><span class="sxs-lookup"><span data-stu-id="650d1-147">Web applications and web APIs that are using the app-only flow (client credentials / client certificate), fall into this category and are thus not impacted by the rollover.</span></span>

### <span data-ttu-id="650d1-148"><a name="appservices"></a>Webové aplikace / rozhraní API Ochrana prostředků a vyvíjené v Azure App Services</span><span class="sxs-lookup"><span data-stu-id="650d1-148"><a name="appservices"></a>Web applications / APIs protecting resources and built using Azure App Services</span></span>
<span data-ttu-id="650d1-149">Azure App Services ověřování / autorizace (EasyAuth) funkce již pomocí potřebné logiky pro zpracování výměna klíče automaticky.</span><span class="sxs-lookup"><span data-stu-id="650d1-149">Azure App Services' Authentication / Authorization (EasyAuth) functionality already has the necessary logic to handle key rollover automatically.</span></span>

### <span data-ttu-id="650d1-150"><a name="owin"></a>Webové aplikace / Ochrana prostředků pomocí rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="650d1-150"><a name="owin"></a>Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>
<span data-ttu-id="650d1-151">Pokud vaše aplikace používá rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware, už je pomocí potřebné logiky pro zpracování výměna klíče automaticky.</span><span class="sxs-lookup"><span data-stu-id="650d1-151">If your application is using the .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="650d1-152">Můžete potvrdit, že vaše aplikace používá některý z těchto tak, že vyhledá všechny následující fragmenty kódu v souboru Startup.cs nebo Startup.Auth.cs vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="650d1-152">You can confirm that your application is using any of these by looking for any of the following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <span data-ttu-id="650d1-153"><a name="owincore"></a>Webové aplikace / Ochrana prostředků pomocí rozhraní .NET Core OpenID Connect nebo JwtBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="650d1-153"><a name="owincore"></a>Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>
<span data-ttu-id="650d1-154">Pokud vaše aplikace používá rozhraní .NET Core OWIN OpenID Connect nebo JwtBearerAuthentication middleware, už je pomocí potřebné logiky pro zpracování výměna klíče automaticky.</span><span class="sxs-lookup"><span data-stu-id="650d1-154">If your application is using the .NET Core OWIN OpenID Connect  or JwtBearerAuthentication middleware, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="650d1-155">Můžete potvrdit, že vaše aplikace používá některý z těchto tak, že vyhledá všechny následující fragmenty kódu v souboru Startup.cs nebo Startup.Auth.cs vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="650d1-155">You can confirm that your application is using any of these by looking for any of the following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <span data-ttu-id="650d1-156"><a name="passport"></a>Webové aplikace / rozhraní API Ochrana prostředků pomocí modulu passport-azure-ad Node.js</span><span class="sxs-lookup"><span data-stu-id="650d1-156"><a name="passport"></a>Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>
<span data-ttu-id="650d1-157">Pokud vaše aplikace používá modul Node.js ve službě Active Directory passport, už je pomocí potřebné logiky pro zpracování výměna klíče automaticky.</span><span class="sxs-lookup"><span data-stu-id="650d1-157">If your application is using the Node.js passport-ad module, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="650d1-158">Potvrďte, že vaše aplikace passport-ad vyhledáním následující fragment kódu v app.js vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="650d1-158">You can confirm that your application passport-ad by searching for the following snippet in your application's app.js</span></span>

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <span data-ttu-id="650d1-159"><a name="vs2015"></a>Webové aplikace / rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="650d1-159"><a name="vs2015"></a>Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>
<span data-ttu-id="650d1-160">Pokud vaše aplikace byla vytvořena pomocí šablony webové aplikace v sadě Visual Studio 2015 nebo Visual Studio 2017 a jste vybrali **pracovní a školní účty** z **změna ověřování** nabídce už je pomocí potřebné logiky pro zpracování výměna klíče automaticky.</span><span class="sxs-lookup"><span data-stu-id="650d1-160">If your application was built using a web application template in Visual Studio 2015 or Visual Studio 2017 and you selected **Work And School Accounts** from the **Change Authentication** menu, it already has the necessary logic to handle key rollover automatically.</span></span> <span data-ttu-id="650d1-161">Tato logika vložených v middlewaru OWIN OpenID Connect načítá a ukládá do mezipaměti klíče ze zjišťování dokumentu OpenID Connect a je pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="650d1-161">This logic, embedded in the OWIN OpenID Connect middleware, retrieves and caches the keys from the OpenID Connect discovery document and periodically refreshes them.</span></span>

<span data-ttu-id="650d1-162">Pokud jste přidali ověřování pro vaše řešení ručně, nemusí mít aplikaci logiky potřeby výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="650d1-162">If you added authentication to your solution manually, your application might not have the necessary key rollover logic.</span></span> <span data-ttu-id="650d1-163">Budete muset napsat sami, nebo postupujte podle kroků v [webové aplikace / rozhraní API pomocí jiné knihovny nebo ručně implementace některé z podporovaných protokolů.](#other).</span><span class="sxs-lookup"><span data-stu-id="650d1-163">You will need to write it yourself, or follow the steps in [Web applications / APIs using any other libraries or manually implementing any of the supported protocols.](#other).</span></span>

### <span data-ttu-id="650d1-164"><a name="vs2013"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="650d1-164"><a name="vs2013"></a>Web applications protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="650d1-165">Pokud aplikace byla vytvořena pomocí šablony webové aplikace v sadě Visual Studio 2013 a jste vybrali **účty organizace** z **změna ověřování** nabídce už je pomocí potřebné logiky pro zpracování výměna klíče automaticky.</span><span class="sxs-lookup"><span data-stu-id="650d1-165">If your application was built using a web application template in Visual Studio 2013 and you selected **Organizational Accounts** from the **Change Authentication** menu, it already has the necessary logic to handle key rollover automatically.</span></span> <span data-ttu-id="650d1-166">Tato logika ukládá jedinečný identifikátor vaší organizace a podpisový klíč ve dvou tabulkách databáze přidružený k projektu.</span><span class="sxs-lookup"><span data-stu-id="650d1-166">This logic stores your organization’s unique identifier and the signing key information in two database tables associated with the project.</span></span> <span data-ttu-id="650d1-167">Připojovací řetězec databáze najdete v souboru Web.config projektu.</span><span class="sxs-lookup"><span data-stu-id="650d1-167">You can find the connection string for the database in the project’s Web.config file.</span></span>

<span data-ttu-id="650d1-168">Pokud jste přidali ověřování pro vaše řešení ručně, nemusí mít aplikaci logiky potřeby výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="650d1-168">If you added authentication to your solution manually, your application might not have the necessary key rollover logic.</span></span> <span data-ttu-id="650d1-169">Budete muset napsat sami, nebo postupujte podle kroků v [webové aplikace / rozhraní API pomocí jiné knihovny nebo ručně implementace některé z podporovaných protokolů.](#other).</span><span class="sxs-lookup"><span data-stu-id="650d1-169">You will need to write it yourself, or follow the steps in [Web applications / APIs using any other libraries or manually implementing any of the supported protocols.](#other).</span></span>

<span data-ttu-id="650d1-170">Následující kroky vám pomohou ověřte, zda je správně funguje logiku ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="650d1-170">The following steps will help you verify that the logic is working properly in your application.</span></span>

1. <span data-ttu-id="650d1-171">V sadě Visual Studio 2013, otevřete řešení a potom klikněte na **Průzkumníka serveru** karty v pravém podokně okna.</span><span class="sxs-lookup"><span data-stu-id="650d1-171">In Visual Studio 2013, open the solution, and then click on the **Server Explorer** tab on the right window.</span></span>
2. <span data-ttu-id="650d1-172">Rozbalte položku **připojení dat**, **objekt DefaultConnection**a potom **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="650d1-172">Expand **Data Connections**, **DefaultConnection**, and then **Tables**.</span></span> <span data-ttu-id="650d1-173">Vyhledejte **IssuingAuthorityKeys** tabulky, klikněte pravým tlačítkem ji a pak klikněte na tlačítko **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="650d1-173">Locate the **IssuingAuthorityKeys** table, right-click it, and then click **Show Table Data**.</span></span>
3. <span data-ttu-id="650d1-174">V **IssuingAuthorityKeys** tabulky, budou existovat alespoň jeden řádek, který odpovídá hodnotě kryptografický otisk pro klíč.</span><span class="sxs-lookup"><span data-stu-id="650d1-174">In the **IssuingAuthorityKeys** table, there will be at least one row, which corresponds to the thumbprint value for the key.</span></span> <span data-ttu-id="650d1-175">Odstraňte všechny řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="650d1-175">Delete any rows in the table.</span></span>
4. <span data-ttu-id="650d1-176">Klikněte pravým tlačítkem myši **klienty** tabulky a potom klikněte na **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="650d1-176">Right-click the **Tenants** table, and then click **Show Table Data**.</span></span>
5. <span data-ttu-id="650d1-177">V **klienty** tabulky, budou existovat alespoň jeden řádek, který odpovídá identifikátor jedinečný directory klienta.</span><span class="sxs-lookup"><span data-stu-id="650d1-177">In the **Tenants** table, there will be at least one row, which corresponds to a unique directory tenant identifier.</span></span> <span data-ttu-id="650d1-178">Odstraňte všechny řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="650d1-178">Delete any rows in the table.</span></span> <span data-ttu-id="650d1-179">Pokud nemáte odstranit řádky v obou **klienty** tabulky a **IssuingAuthorityKeys** tabulky, bude dojde k chybě za běhu.</span><span class="sxs-lookup"><span data-stu-id="650d1-179">If you don't delete the rows in both the **Tenants** table and **IssuingAuthorityKeys** table, you will get an error at runtime.</span></span>
6. <span data-ttu-id="650d1-180">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="650d1-180">Build and run the application.</span></span> <span data-ttu-id="650d1-181">Poté, co jste přihlášení k účtu, můžete zastavit aplikace.</span><span class="sxs-lookup"><span data-stu-id="650d1-181">After you have logged in to your account, you can stop the application.</span></span>
7. <span data-ttu-id="650d1-182">Vraťte se do **Průzkumníka serveru** a podívejte se na hodnoty v **IssuingAuthorityKeys** a **klienty** tabulky.</span><span class="sxs-lookup"><span data-stu-id="650d1-182">Return to the **Server Explorer** and look at the values in the **IssuingAuthorityKeys** and **Tenants** table.</span></span> <span data-ttu-id="650d1-183">Můžete si všimnout, že se mají byla automaticky jeho plnění znovu s příslušnými informacemi z dokument federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="650d1-183">You’ll notice that they have been automatically repopulated with the appropriate information from the federation metadata document.</span></span>

### <span data-ttu-id="650d1-184"><a name="vs2013"></a>Webová rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="650d1-184"><a name="vs2013"></a>Web APIs protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="650d1-185">Pokud jste vytvořili pro webové aplikace rozhraní API v sadě Visual Studio 2013 pomocí šablony webového rozhraní API a pak vybrali **účty organizace** z **změna ověřování** nabídky, které již mají nezbytné Logika ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="650d1-185">If you created a web API application in Visual Studio 2013 using the Web API template, and then selected **Organizational Accounts** from the **Change Authentication** menu, you already have the necessary logic in your application.</span></span>

<span data-ttu-id="650d1-186">Pokud ručně nakonfigurované ověřování, postupujte podle pokynů níže se dozvíte, jak nakonfigurovat webové rozhraní API se automaticky aktualizovat informace o jeho klíči.</span><span class="sxs-lookup"><span data-stu-id="650d1-186">If you manually configured authentication, follow the instructions below to learn how to configure your Web API to automatically update its key information.</span></span>

<span data-ttu-id="650d1-187">Následující fragment kódu ukazuje, jak získat nejnovější klíče z dokument federačních metadat a potom pomocí [obslužná rutina tokenu JWT](https://msdn.microsoft.com/library/dn205065.aspx) k ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="650d1-187">The following code snippet demonstrates how to get the latest keys from the federation metadata document, and then use the [JWT Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) to validate the token.</span></span> <span data-ttu-id="650d1-188">Fragment kódu předpokládá, že použijete vlastní ukládání do mezipaměti mechanismus pro zachování klíč k ověření budoucí tokeny z Azure AD, zda byl v databázi, konfigurační soubor nebo jinde.</span><span class="sxs-lookup"><span data-stu-id="650d1-188">The code snippet assumes that you will use your own caching mechanism for persisting the key to validate future tokens from Azure AD, whether it be in a database, configuration file, or elsewhere.</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <span data-ttu-id="650d1-189"><a name="vs2012"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="650d1-189"><a name="vs2012"></a>Web applications protecting resources and created with Visual Studio 2012</span></span>
<span data-ttu-id="650d1-190">Pokud vaše aplikace byla vytvořena v sadě Visual Studio 2012, pravděpodobně použili identita a přístup ke konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="650d1-190">If your application was built in Visual Studio 2012, you probably used the Identity and Access Tool to configure your application.</span></span> <span data-ttu-id="650d1-191">Je pravděpodobné, že používáte [ověřování vystavitele název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span><span class="sxs-lookup"><span data-stu-id="650d1-191">It’s also likely that you are using the [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span></span> <span data-ttu-id="650d1-192">VINR zodpovídá za údržbu informace o poskytovatelích důvěryhodné identity (Azure AD) a slouží k ověření, tokeny vydané podle jejich klíče.</span><span class="sxs-lookup"><span data-stu-id="650d1-192">The VINR is responsible for maintaining information about trusted identity providers (Azure AD) and the keys used to validate tokens issued by them.</span></span> <span data-ttu-id="650d1-193">VINR také usnadňuje automaticky aktualizovat klíče uložené v souboru Web.config stáhněte nejnovější dokument metadat federace spojené s adresářem, informace o kontrole, zda je aktuální pomocí nejnovější dokumentů, konfigurace a aktualizace aplikace pro používání nového klíče podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="650d1-193">The VINR also makes it easy to automatically update the key information stored in a Web.config file by downloading the latest federation metadata document associated with your directory, checking if the configuration is out of date with the latest document, and updating the application to use the new key as necessary.</span></span>

<span data-ttu-id="650d1-194">Pokud jste vytvořili vaší aplikace pomocí některé z ukázky kódu nebo návod dokumentaci od společnosti Microsoft, logice výměna klíče je již zahrnut ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="650d1-194">If you created your application using any of the code samples or walkthrough documentation provided by Microsoft, the key rollover logic is already included in your project.</span></span> <span data-ttu-id="650d1-195">Si všimnete, že následující kód v projektu již existuje.</span><span class="sxs-lookup"><span data-stu-id="650d1-195">You will notice that the code below already exists in your project.</span></span> <span data-ttu-id="650d1-196">Pokud vaše aplikace již tuto logiku nemá, použijte následující postup je přidat a ověřit, zda pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="650d1-196">If your application does not already have this logic, follow the steps below to add it and to verify that it’s working correctly.</span></span>

1. <span data-ttu-id="650d1-197">V **Průzkumníku řešení**, přidejte odkaz na **System.IdentityModel** sestavení pro příslušný projekt.</span><span class="sxs-lookup"><span data-stu-id="650d1-197">In **Solution Explorer**, add a reference to the **System.IdentityModel** assembly for the appropriate project.</span></span>
2. <span data-ttu-id="650d1-198">Otevřete **Global.asax.cs** souboru a přidejte následující direktivy using:</span><span class="sxs-lookup"><span data-stu-id="650d1-198">Open the **Global.asax.cs** file and add the following using directives:</span></span>
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. <span data-ttu-id="650d1-199">Přidejte následující metodu do **Global.asax.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="650d1-199">Add the following method to the **Global.asax.cs** file:</span></span>
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. <span data-ttu-id="650d1-200">Vyvolání **RefreshValidationSettings()** metoda v **Application_Start()** metoda v **Global.asax.cs** znázorněné:</span><span class="sxs-lookup"><span data-stu-id="650d1-200">Invoke the **RefreshValidationSettings()** method in the **Application_Start()** method in **Global.asax.cs** as shown:</span></span>
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

<span data-ttu-id="650d1-201">Jakmile jste postupovali podle těchto kroků, souboru Web.config vaší aplikace bude aktualizováno o nejnovější informace z dokument federačních metadat, včetně nejnovější klíčů.</span><span class="sxs-lookup"><span data-stu-id="650d1-201">Once you have followed these steps, your application’s Web.config will be updated with the latest information from the federation metadata document, including the latest keys.</span></span> <span data-ttu-id="650d1-202">Tato aktualizace se objeví pokaždé, když recykluje fond aplikací ve službě IIS; ve výchozím nastavení nastavení služba IIS recyklovat aplikace každých 29 hodin.</span><span class="sxs-lookup"><span data-stu-id="650d1-202">This update will occur every time your application pool recycles in IIS; by default IIS is set to recycle applications every 29 hours.</span></span>

<span data-ttu-id="650d1-203">Postupujte podle následujících kroků a ověřte, zda je funkční logice výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="650d1-203">Follow the steps below to verify that the key rollover logic is working.</span></span>

1. <span data-ttu-id="650d1-204">Po ověření, že vaše aplikace používá výše uvedený kód, otevřete **Web.config** souboru a přejděte do  **<issuerNameRegistry>**  bloku, konkrétně hledá následující několika řádků:</span><span class="sxs-lookup"><span data-stu-id="650d1-204">After you have verified that your application is using the code above, open the **Web.config** file and navigate to the **<issuerNameRegistry>** block, specifically looking for the following few lines:</span></span>
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. <span data-ttu-id="650d1-205">V  **<add thumbprint=””>**  nahrazením libovolný znak jiné nastavení, změňte hodnotu kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="650d1-205">In the **<add thumbprint=””>** setting, change the thumbprint value by replacing any character with a different one.</span></span> <span data-ttu-id="650d1-206">Uložit **Web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="650d1-206">Save the **Web.config** file.</span></span>
3. <span data-ttu-id="650d1-207">Sestavte aplikaci a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="650d1-207">Build the application, and then run it.</span></span> <span data-ttu-id="650d1-208">Pokud dokončíte proces přihlášení, je vaše aplikace úspěšně aktualizace stáhnout požadované informace z dokument federačních metadat svého adresáře na klíč.</span><span class="sxs-lookup"><span data-stu-id="650d1-208">If you can complete the sign-in process, your application is successfully updating the key by downloading the required information from your directory’s federation metadata document.</span></span> <span data-ttu-id="650d1-209">Pokud máte problémy s přihlášením, zkontrolujte změny v aplikaci jsou správné načtením [přidání přihlašování k vaší webové aplikace pomocí Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) tématu nebo stahování a zkontrolujete následující ukázka kódu: [ Víceklientská Cloudová aplikace pro Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span><span class="sxs-lookup"><span data-stu-id="650d1-209">If you are having issues signing in, ensure the changes in your application are correct by reading the [Adding Sign-On to Your Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) topic, or downloading and inspecting the following code sample: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span></span>

### <span data-ttu-id="650d1-210"><a name="vs2010"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2008 nebo 2010 a Windows Identity Foundation (WIF) verze 1.0 pro rozhraní .NET 3.5</span><span class="sxs-lookup"><span data-stu-id="650d1-210"><a name="vs2010"></a>Web applications protecting resources and created with Visual Studio 2008 or 2010 and Windows Identity Foundation (WIF) v1.0 for .NET 3.5</span></span>
<span data-ttu-id="650d1-211">Pokud jste vytvořili aplikaci na verzi WIF verze 1.0, neexistuje žádný zadaný mechanismus automaticky aktualizovat konfiguraci vaší aplikace pomocí nového klíče.</span><span class="sxs-lookup"><span data-stu-id="650d1-211">If you built an application on WIF v1.0, there is no provided mechanism to automatically refresh your application’s configuration to use a new key.</span></span>

* <span data-ttu-id="650d1-212">*Nejjednodušším způsobem, jak* pomocí nástrojů FedUtil zahrnutý v sadě SDK WIF, které můžete načíst nejnovější dokument metadat a aktualizovat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="650d1-212">*Easiest way* Use the FedUtil tooling included in the WIF SDK, which can retrieve the latest metadata document and update your configuration.</span></span>
* <span data-ttu-id="650d1-213">Aktualizace aplikace .NET 4.5, který obsahuje nejnovější verzi WIF nachází v oboru názvů systému.</span><span class="sxs-lookup"><span data-stu-id="650d1-213">Update your application to .NET 4.5, which includes the newest version of WIF located in the System namespace.</span></span> <span data-ttu-id="650d1-214">Pak můžete použít [ověřování vystavitele název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) provádění automatických aktualizací na konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="650d1-214">You can then use the [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) to perform automatic updates of the application’s configuration.</span></span>
* <span data-ttu-id="650d1-215">Proveďte ruční výměna podle pokynů na konci tohoto dokumentu pokyny.</span><span class="sxs-lookup"><span data-stu-id="650d1-215">Perform a manual rollover as per the instructions at the end of this guidance document.</span></span>

<span data-ttu-id="650d1-216">Pokyny, jak pomocí FedUtil aktualizovat konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="650d1-216">Instructions to use the FedUtil to update your configuration:</span></span>

1. <span data-ttu-id="650d1-217">Ověřte, zda máte verze 1.0 WIF SDK pro Visual Studio 2008 nebo 2010 nainstalována na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="650d1-217">Verify that you have the WIF v1.0 SDK installed on your development machine for Visual Studio 2008 or 2010.</span></span> <span data-ttu-id="650d1-218">Můžete [stáhnout odsud](https://www.microsoft.com/en-us/download/details.aspx?id=4451) Pokud jste ještě nenainstalovali ho.</span><span class="sxs-lookup"><span data-stu-id="650d1-218">You can [download it from here](https://www.microsoft.com/en-us/download/details.aspx?id=4451) if you have not yet installed it.</span></span>
2. <span data-ttu-id="650d1-219">V sadě Visual Studio otevřete řešení a potom klikněte pravým tlačítkem na příslušné projekt a vyberte **aktualizace federačních metadat**.</span><span class="sxs-lookup"><span data-stu-id="650d1-219">In Visual Studio, open the solution, and then right-click the applicable project and select **Update federation metadata**.</span></span> <span data-ttu-id="650d1-220">Pokud tato možnost není k dispozici, nebyl nainstalován FedUtil nebo verze 1.0 WIF SDK.</span><span class="sxs-lookup"><span data-stu-id="650d1-220">If this option is not available, FedUtil and/or the WIF v1.0 SDK has not been installed.</span></span>
3. <span data-ttu-id="650d1-221">Na řádku vyberte **aktualizace** zahájíte aktualizace federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="650d1-221">From the prompt, select **Update** to begin updating your federation metadata.</span></span> <span data-ttu-id="650d1-222">Pokud máte přístup k prostředí serveru, který je hostitelem aplikace, můžete volitelně použít na FedUtil [scheduler automatické metadata aktualizace](https://msdn.microsoft.com/library/ee517272.aspx).</span><span class="sxs-lookup"><span data-stu-id="650d1-222">If you have access to the server environment where the application is hosted, you can optionally use FedUtil’s [automatic metadata update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).</span></span>
4. <span data-ttu-id="650d1-223">Klikněte na tlačítko **Dokončit** k dokončení procesu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="650d1-223">Click **Finish** to complete the update process.</span></span>

### <span data-ttu-id="650d1-224"><a name="other"></a>Webové aplikace / Ochrana prostředků pomocí jiné knihovny nebo některé z podporovaných protokolů ručně implementace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="650d1-224"><a name="other"></a>Web applications / APIs protecting resources using any other libraries or manually implementing any of the supported protocols</span></span>
<span data-ttu-id="650d1-225">Pokud používáte některé jiné knihovny nebo ručně implementována některé z podporovaných protokolů, budete muset zkontrolovat knihovny nebo implementaci zajistit, že klíč je načítány ze zjišťování dokumentu OpenID Connect nebo federačních metadat dokument.</span><span class="sxs-lookup"><span data-stu-id="650d1-225">If you are using some other library or manually implemented any of the supported protocols, you'll need to review the library or your implementation to ensure that the key is being retrieved from either the OpenID Connect discovery document or the federation metadata document.</span></span> <span data-ttu-id="650d1-226">Jeden způsob kontroly pro tento je hledání v kódu nebo knihovny kódu pro volání na dokument zjišťování OpenID nebo dokument federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="650d1-226">One way to check for this is to do a search in your code or the library's code for any calls out to either the OpenID discovery document or the federation metadata document.</span></span>

<span data-ttu-id="650d1-227">Pokud se klíče ukládají někde nebo pevně zakódované ve vaší aplikaci, můžete ručně načíst klíč a aktualizovat ji odpovídajícím způsobem podle provést ruční výměna podle pokynů na konci tohoto dokumentu pokyny.</span><span class="sxs-lookup"><span data-stu-id="650d1-227">If they key is being stored somewhere or hardcoded in your application, you can manually retrieve the key and update it accordingly by perform a manual rollover as per the instructions at the end of this guidance document.</span></span> <span data-ttu-id="650d1-228">**Důrazně doporučujeme, můžete zvýšit vaši aplikaci, aby podporovala automatické výměny** pomocí všechny přístupy obrysu v tomto článku, aby se zabránilo režii a budoucí přerušení, pokud Azure AD zvyšuje výměny cadence nebo má naléhavém Out-of-band výměny.</span><span class="sxs-lookup"><span data-stu-id="650d1-228">**It is strongly encouraged that you enhance your application to support automatic rollover** using any of the approaches outline in this article to avoid future disruptions and overhead if Azure AD increases it's rollover cadence or has an emergency out-of-band rollover.</span></span>

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a><span data-ttu-id="650d1-229">Postup testování vaší aplikace k určení, pokud bude mít vliv</span><span class="sxs-lookup"><span data-stu-id="650d1-229">How to test your application to determine if it will be affected</span></span>
<span data-ttu-id="650d1-230">Můžete ověřit, jestli aplikace podporuje automatickou výměnu klíče stažením skripty a podle pokynů v [toto úložiště GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span><span class="sxs-lookup"><span data-stu-id="650d1-230">You can validate whether your application supports automatic key rollover by downloading the scripts and following the instructions in [this GitHub repository.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span></span>

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a><span data-ttu-id="650d1-231">Jak provést ruční výměna, pokud je aplikace nepodporuje automatické výměny</span><span class="sxs-lookup"><span data-stu-id="650d1-231">How to perform a manual rollover if you application does not support automatic rollover</span></span>
<span data-ttu-id="650d1-232">Pokud aplikace nemá **není** podporuje automatickou výměnu, budete muset vytvořit proces, který pravidelně monitorování Azure AD podepisovací klíče a provede ruční výměna odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="650d1-232">If your application does **not** support automatic rollover, you will need to establish a process that periodically monitors Azure AD's signing keys and performs a manual rollover accordingly.</span></span> <span data-ttu-id="650d1-233">[Toto úložiště GitHub](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) obsahuje skripty a pokyny o tom, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="650d1-233">[This GitHub repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) contains scripts and instructions on how to do this.</span></span>

