---
title: "aaaSigning výměny klíče ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje hello podepisování výměna klíče osvědčené postupy pro Azure Active Directory"
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
ms.openlocfilehash: ac6ade7f3ba2fbd22ea6d447aa5d07a2d6bdd451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a><span data-ttu-id="7e601-103">Podepisování výměna klíče v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e601-103">Signing key rollover in Azure Active Directory</span></span>
<span data-ttu-id="7e601-104">Toto téma popisuje, co potřebujete tooknow o hello veřejných klíčů, které se používají v tokenech zabezpečení toosign Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7e601-104">This topic discusses what you need tooknow about hello public keys that are used in Azure Active Directory (Azure AD) toosign security tokens.</span></span> <span data-ttu-id="7e601-105">Je důležité toonote, který může být tyto výměny klíčů v pravidelných intervalech a v případě nouze převracet okamžitě.</span><span class="sxs-lookup"><span data-stu-id="7e601-105">It is important toonote that these keys rollover on a periodic basis and, in an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="7e601-106">Všechny aplikace, které používají Azure AD by měla být schopný tooprogrammatically popisovač hello výměna klíče procesu nebo vytvořit proces periodické ruční výměny.</span><span class="sxs-lookup"><span data-stu-id="7e601-106">All applications that use Azure AD should be able tooprogrammatically handle hello key rollover process or establish a periodic manual rollover process.</span></span> <span data-ttu-id="7e601-107">Pokračujte ve čtení toounderstand jak hello klíče fungovat, jak tooassess hello dopad hello výměny tooyour aplikace a jak tooupdate aplikace nebo vytvořit klíče výměny pravidelné ruční výměna proces toohandle v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="7e601-107">Continue reading toounderstand how hello keys work, how tooassess hello impact of hello rollover tooyour application and how tooupdate your application or establish a periodic manual rollover process toohandle key rollover if necessary.</span></span>

## <a name="overview-of-signing-keys-in-azure-ad"></a><span data-ttu-id="7e601-108">Přehled podpisových klíčů ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="7e601-108">Overview of signing keys in Azure AD</span></span>
<span data-ttu-id="7e601-109">Azure AD používá šifrování veřejného klíče založený na oborových standardů tooestablish vztah důvěryhodnosti mezi samostatně a hello aplikace, které ho používají.</span><span class="sxs-lookup"><span data-stu-id="7e601-109">Azure AD uses public-key cryptography built on industry standards tooestablish trust between itself and hello applications that use it.</span></span> <span data-ttu-id="7e601-110">V praxi, toto funguje ve hello následujícím způsobem: Azure AD používá podpisový klíč, který se skládá z pár veřejného a privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="7e601-110">In practical terms, this works in hello following way: Azure AD uses a signing key that consists of a public and private key pair.</span></span> <span data-ttu-id="7e601-111">Pokud se uživatel přihlásí v aplikaci tooan, která používá Azure AD pro ověřování, Azure AD vytvoří token zabezpečení, který obsahuje informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-111">When a user signs in tooan application that uses Azure AD for authentication, Azure AD creates a security token that contains information about hello user.</span></span> <span data-ttu-id="7e601-112">Tento token je podepsána pomocí jeho privátní klíč před odesláním zpět toohello aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7e601-112">This token is signed by Azure AD using its private key before it is sent back toohello application.</span></span> <span data-ttu-id="7e601-113">tooverify, který hello token je platný, ve skutečnosti původu z Azure AD, hello aplikace musíte ověřit hello token podpis pomocí veřejného klíče hello vystavené Azure AD, která je součástí klienta hello [OpenID Connect zjišťování dokumentu](http://openid.net/specs/openid-connect-discovery-1_0.html) nebo SAML/WS-Fed [dokument federačních metadat](active-directory-federation-metadata.md).</span><span class="sxs-lookup"><span data-stu-id="7e601-113">tooverify that hello token is valid and actually originated from Azure AD, hello application must validate hello token’s signature using hello public key exposed by Azure AD that is contained in hello tenant’s [OpenID Connect discovery document](http://openid.net/specs/openid-connect-discovery-1_0.html) or SAML/WS-Fed [federation metadata document](active-directory-federation-metadata.md).</span></span>

<span data-ttu-id="7e601-114">Z bezpečnostních důvodů Azure AD podpisového klíče zobrazí souhrn v pravidelných intervalech a v případě hello nouze, může být převracet okamžitě.</span><span class="sxs-lookup"><span data-stu-id="7e601-114">For security purposes, Azure AD’s signing key rolls on a periodic basis and, in hello case of an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="7e601-115">Všechny aplikace, který se integruje s Azure AD měli připravit toohandle výměna klíče události bez ohledu na tom, jak často může dojít.</span><span class="sxs-lookup"><span data-stu-id="7e601-115">Any application that integrates with Azure AD should be prepared toohandle a key rollover event no matter how frequently it may occur.</span></span> <span data-ttu-id="7e601-116">Pokud tomu tak není, a aplikace pokusí toouse vypršenou platností klíče tooverify hello podpis na token, hello požádat o přihlášení selže.</span><span class="sxs-lookup"><span data-stu-id="7e601-116">If it doesn’t, and your application attempts toouse an expired key tooverify hello signature on a token, hello sign-in request will fail.</span></span>

<span data-ttu-id="7e601-117">Vždy není k dispozici v dokumentu zjišťování OpenID Connect hello a dokument federačních metadat hello více než jeden platný klíč.</span><span class="sxs-lookup"><span data-stu-id="7e601-117">There is always more than one valid key available in hello OpenID Connect discovery document and hello federation metadata document.</span></span> <span data-ttu-id="7e601-118">Je třeba připravit aplikaci toouse jakéhokoli hello klíčů zadaného v dokumentu hello vzhledem k tomu, že jeden klíč může být vrácena brzy, jiné můžou být jeho nahrazení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="7e601-118">Your application should be prepared toouse any of hello keys specified in hello document, since one key may be rolled soon, another may be its replacement, and so forth.</span></span>

## <a name="how-tooassess-if-your-application-will-be-affected-and-what-toodo-about-it"></a><span data-ttu-id="7e601-119">Jak tooassess, pokud vaše aplikace bude mít vliv na a jaké toodo o něm</span><span class="sxs-lookup"><span data-stu-id="7e601-119">How tooassess if your application will be affected and what toodo about it</span></span>
<span data-ttu-id="7e601-120">Jak vaše aplikace zpracovává výměny klíčů, závisí na proměnné například hello typů aplikací nebo jaké protokol identity a knihovna byl použit.</span><span class="sxs-lookup"><span data-stu-id="7e601-120">How your application handles key rollover depends on variables such as hello type of application or what identity protocol and library was used.</span></span> <span data-ttu-id="7e601-121">v níže uvedených částech Hello vyhodnocení, zda hello nejběžnějších typů aplikací je postiženo hello výměny klíčů a poskytovat informace o tom, jak tooupdate hello automatickou výměnu toosupport aplikace nebo ručně aktualizovat klíč hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-121">hello sections below assess whether hello most common types of applications are impacted by hello key rollover and provide guidance on how tooupdate hello application toosupport automatic rollover or manually update hello key.</span></span>

* [<span data-ttu-id="7e601-122">Nativní klientské aplikace přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="7e601-122">Native client applications accessing resources</span></span>](#nativeclient)
* [<span data-ttu-id="7e601-123">Webové aplikace / rozhraní API pro přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="7e601-123">Web applications / APIs accessing resources</span></span>](#webclient)
* [<span data-ttu-id="7e601-124">Webové aplikace / rozhraní API Ochrana prostředků a vyvíjené v Azure App Services</span><span class="sxs-lookup"><span data-stu-id="7e601-124">Web applications / APIs protecting resources and built using Azure App Services</span></span>](#appservices)
* [<span data-ttu-id="7e601-125">Webové aplikace / Ochrana prostředků pomocí rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7e601-125">Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>](#owin)
* [<span data-ttu-id="7e601-126">Webové aplikace / Ochrana prostředků pomocí rozhraní .NET Core OpenID Connect nebo JwtBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7e601-126">Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>](#owincore)
* [<span data-ttu-id="7e601-127">Webové aplikace / rozhraní API Ochrana prostředků pomocí modulu passport-azure-ad Node.js</span><span class="sxs-lookup"><span data-stu-id="7e601-127">Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>](#passport)
* [<span data-ttu-id="7e601-128">Webové aplikace / rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7e601-128">Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>](#vs2015)
* [<span data-ttu-id="7e601-129">Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7e601-129">Web applications protecting resources and created with Visual Studio 2013</span></span>](#vs2013)
* [<span data-ttu-id="7e601-130">Webová rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7e601-130">Web APIs protecting resources and created with Visual Studio 2013</span></span>](#vs2013_webapi)
* [<span data-ttu-id="7e601-131">Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7e601-131">Web applications protecting resources and created with Visual Studio 2012</span></span>](#vs2012)
* [<span data-ttu-id="7e601-132">Webové aplikace Ochrana prostředků a vytvořit s Visual Studio 2010, o 2008 pomocí technologie Windows Identity Foundation</span><span class="sxs-lookup"><span data-stu-id="7e601-132">Web applications protecting resources and created with Visual Studio 2010, 2008 o using Windows Identity Foundation</span></span>](#vs2010)
* [<span data-ttu-id="7e601-133">Webové aplikace / rozhraní API Ochrana prostředků pomocí kterékoli jiné knihovny nebo ručně implementací hello podporované protokoly</span><span class="sxs-lookup"><span data-stu-id="7e601-133">Web applications / APIs protecting resources using any other libraries or manually implementing any of hello supported protocols</span></span>](#other)

<span data-ttu-id="7e601-134">V tomto návodu je **není** platí pro:</span><span class="sxs-lookup"><span data-stu-id="7e601-134">This guidance is **not** applicable for:</span></span>

* <span data-ttu-id="7e601-135">Aplikace přidána z Azure AD Application Gallery (včetně vlastní) mají zvláštní pokyny namapoval toosigning klíče.</span><span class="sxs-lookup"><span data-stu-id="7e601-135">Applications added from Azure AD Application Gallery (including Custom) have separate guidance with regards toosigning keys.</span></span> [<span data-ttu-id="7e601-136">Další informace.</span><span class="sxs-lookup"><span data-stu-id="7e601-136">More information.</span></span>](../active-directory-sso-certs.md)
* <span data-ttu-id="7e601-137">Místní aplikacích publikovaných prostřednictvím proxy aplikace nemáte tooworry o podpisových klíčů.</span><span class="sxs-lookup"><span data-stu-id="7e601-137">On-premises applications published via application proxy don't have tooworry about signing keys.</span></span>

### <span data-ttu-id="7e601-138"><a name="nativeclient"></a>Nativní klientské aplikace přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="7e601-138"><a name="nativeclient"></a>Native client applications accessing resources</span></span>
<span data-ttu-id="7e601-139">Aplikace, které jsou pouze přístup k prostředkům (tj</span><span class="sxs-lookup"><span data-stu-id="7e601-139">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="7e601-140">Microsoft Graph, KeyVault, rozhraní API aplikace Outlook a další APIs Microsoft) obecně pouze získat token a předají toohello vlastníka prostředku.</span><span class="sxs-lookup"><span data-stu-id="7e601-140">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along toohello resource owner.</span></span> <span data-ttu-id="7e601-141">Vzhledem k tomu, že se nechrání žádné prostředky, není zkontrolovat hello token a proto není nutné tooensure, které je správně podepsaný.</span><span class="sxs-lookup"><span data-stu-id="7e601-141">Given that they are not protecting any resources, they do not inspect hello token and therefore do not need tooensure it is properly signed.</span></span>

<span data-ttu-id="7e601-142">Nativní klientské aplikace, zda desktop či mobile, do této kategorie patří a nejsou proto vliv hello výměny.</span><span class="sxs-lookup"><span data-stu-id="7e601-142">Native client applications, whether desktop or mobile, fall into this category and are thus not impacted by hello rollover.</span></span>

### <span data-ttu-id="7e601-143"><a name="webclient"></a>Webové aplikace / rozhraní API pro přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="7e601-143"><a name="webclient"></a>Web applications / APIs accessing resources</span></span>
<span data-ttu-id="7e601-144">Aplikace, které jsou pouze přístup k prostředkům (tj</span><span class="sxs-lookup"><span data-stu-id="7e601-144">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="7e601-145">Microsoft Graph, KeyVault, rozhraní API aplikace Outlook a další APIs Microsoft) obecně pouze získat token a předají toohello vlastníka prostředku.</span><span class="sxs-lookup"><span data-stu-id="7e601-145">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along toohello resource owner.</span></span> <span data-ttu-id="7e601-146">Vzhledem k tomu, že se nechrání žádné prostředky, není zkontrolovat hello token a proto není nutné tooensure, které je správně podepsaný.</span><span class="sxs-lookup"><span data-stu-id="7e601-146">Given that they are not protecting any resources, they do not inspect hello token and therefore do not need tooensure it is properly signed.</span></span>

<span data-ttu-id="7e601-147">Webové aplikace a webové rozhraní API, která používají toku jen aplikace hello (pověření klienta nebo certifikát klienta), do této kategorie patří a nejsou proto vliv hello výměny.</span><span class="sxs-lookup"><span data-stu-id="7e601-147">Web applications and web APIs that are using hello app-only flow (client credentials / client certificate), fall into this category and are thus not impacted by hello rollover.</span></span>

### <span data-ttu-id="7e601-148"><a name="appservices"></a>Webové aplikace / rozhraní API Ochrana prostředků a vyvíjené v Azure App Services</span><span class="sxs-lookup"><span data-stu-id="7e601-148"><a name="appservices"></a>Web applications / APIs protecting resources and built using Azure App Services</span></span>
<span data-ttu-id="7e601-149">Azure App Services ověřování / autorizace (EasyAuth) funkce automaticky již má hello potřebné logiky toohandle výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="7e601-149">Azure App Services' Authentication / Authorization (EasyAuth) functionality already has hello necessary logic toohandle key rollover automatically.</span></span>

### <span data-ttu-id="7e601-150"><a name="owin"></a>Webové aplikace / Ochrana prostředků pomocí rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7e601-150"><a name="owin"></a>Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>
<span data-ttu-id="7e601-151">Pokud vaše aplikace používá hello .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware, už je výměna klíče toohandle hello potřebné logiky automaticky.</span><span class="sxs-lookup"><span data-stu-id="7e601-151">If your application is using hello .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="7e601-152">Můžete potvrdit, že vaše aplikace používá některý z těchto tak, že vyhledá všechny hello následující fragmenty kódu v souboru Startup.cs nebo Startup.Auth.cs vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7e601-152">You can confirm that your application is using any of these by looking for any of hello following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

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

### <span data-ttu-id="7e601-153"><a name="owincore"></a>Webové aplikace / Ochrana prostředků pomocí rozhraní .NET Core OpenID Connect nebo JwtBearerAuthentication middleware rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7e601-153"><a name="owincore"></a>Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>
<span data-ttu-id="7e601-154">Pokud vaše aplikace používá hello .NET Core OWIN OpenID Connect nebo JwtBearerAuthentication middleware, už je výměna klíče toohandle hello potřebné logiky automaticky.</span><span class="sxs-lookup"><span data-stu-id="7e601-154">If your application is using hello .NET Core OWIN OpenID Connect  or JwtBearerAuthentication middleware, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="7e601-155">Můžete potvrdit, že vaše aplikace používá některý z těchto tak, že vyhledá všechny hello následující fragmenty kódu v souboru Startup.cs nebo Startup.Auth.cs vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7e601-155">You can confirm that your application is using any of these by looking for any of hello following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

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

### <span data-ttu-id="7e601-156"><a name="passport"></a>Webové aplikace / rozhraní API Ochrana prostředků pomocí modulu passport-azure-ad Node.js</span><span class="sxs-lookup"><span data-stu-id="7e601-156"><a name="passport"></a>Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>
<span data-ttu-id="7e601-157">Pokud vaše aplikace používá modulu passport-ad Node.js hello, už je výměna klíče toohandle hello potřebné logiky automaticky.</span><span class="sxs-lookup"><span data-stu-id="7e601-157">If your application is using hello Node.js passport-ad module, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="7e601-158">Potvrďte, že vaše aplikace passport-ad vyhledáním hello následující fragment kódu v app.js vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7e601-158">You can confirm that your application passport-ad by searching for hello following snippet in your application's app.js</span></span>

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <span data-ttu-id="7e601-159"><a name="vs2015"></a>Webové aplikace / rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7e601-159"><a name="vs2015"></a>Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>
<span data-ttu-id="7e601-160">Pokud vaše aplikace byla vytvořena pomocí šablony webové aplikace v sadě Visual Studio 2015 nebo Visual Studio 2017 a jste vybrali **pracovní a školní účty** z hello **změna ověřování** nabídce již Výměna klíče toohandle hello potřebné logiky má automaticky.</span><span class="sxs-lookup"><span data-stu-id="7e601-160">If your application was built using a web application template in Visual Studio 2015 or Visual Studio 2017 and you selected **Work And School Accounts** from hello **Change Authentication** menu, it already has hello necessary logic toohandle key rollover automatically.</span></span> <span data-ttu-id="7e601-161">Tuto logiku vložených v middlewaru OWIN OpenID Connect hello, načítá a ukládá do mezipaměti klíče hello z hello OpenID Connect zjišťování dokumentu a je pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="7e601-161">This logic, embedded in hello OWIN OpenID Connect middleware, retrieves and caches hello keys from hello OpenID Connect discovery document and periodically refreshes them.</span></span>

<span data-ttu-id="7e601-162">Pokud jste ručně přidali řešení tooyour ověřování, nemusí mít aplikaci logiky hello potřeby výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="7e601-162">If you added authentication tooyour solution manually, your application might not have hello necessary key rollover logic.</span></span> <span data-ttu-id="7e601-163">Budete potřebovat toowrite ho sami nebo hello postupujte podle kroků v [webové aplikace / rozhraní API pomocí jiné knihovny nebo ručně implementací hello podporované protokoly.](#other).</span><span class="sxs-lookup"><span data-stu-id="7e601-163">You will need toowrite it yourself, or follow hello steps in [Web applications / APIs using any other libraries or manually implementing any of hello supported protocols.](#other).</span></span>

### <span data-ttu-id="7e601-164"><a name="vs2013"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7e601-164"><a name="vs2013"></a>Web applications protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="7e601-165">Pokud aplikace byla vytvořena pomocí šablony webové aplikace v sadě Visual Studio 2013 a jste vybrali **účty organizace** z hello **změna ověřování** nabídce už je nezbytné hello Logika toohandle klíče výměny automaticky.</span><span class="sxs-lookup"><span data-stu-id="7e601-165">If your application was built using a web application template in Visual Studio 2013 and you selected **Organizational Accounts** from hello **Change Authentication** menu, it already has hello necessary logic toohandle key rollover automatically.</span></span> <span data-ttu-id="7e601-166">Tato logika ukládá jedinečný identifikátor vaší organizace a hello podepisování klíčové informace do dvou tabulek databáze přidružený hello projektu.</span><span class="sxs-lookup"><span data-stu-id="7e601-166">This logic stores your organization’s unique identifier and hello signing key information in two database tables associated with hello project.</span></span> <span data-ttu-id="7e601-167">Hello připojovacího řetězce pro databázi hello najdete v souboru Web.config hello projektu.</span><span class="sxs-lookup"><span data-stu-id="7e601-167">You can find hello connection string for hello database in hello project’s Web.config file.</span></span>

<span data-ttu-id="7e601-168">Pokud jste ručně přidali řešení tooyour ověřování, nemusí mít aplikaci logiky hello potřeby výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="7e601-168">If you added authentication tooyour solution manually, your application might not have hello necessary key rollover logic.</span></span> <span data-ttu-id="7e601-169">Budete potřebovat toowrite ho sami nebo hello postupujte podle kroků v [webové aplikace / rozhraní API pomocí jiné knihovny nebo ručně implementací hello podporované protokoly.](#other).</span><span class="sxs-lookup"><span data-stu-id="7e601-169">You will need toowrite it yourself, or follow hello steps in [Web applications / APIs using any other libraries or manually implementing any of hello supported protocols.](#other).</span></span>

<span data-ttu-id="7e601-170">Hello následující kroky vám pomůže ověřte, zda text hello logiku správně funguje ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7e601-170">hello following steps will help you verify that hello logic is working properly in your application.</span></span>

1. <span data-ttu-id="7e601-171">V sadě Visual Studio 2013, otevřete hello řešení a potom klikněte na hello **Průzkumníka serveru** karty v pravém podokně okna hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-171">In Visual Studio 2013, open hello solution, and then click on hello **Server Explorer** tab on hello right window.</span></span>
2. <span data-ttu-id="7e601-172">Rozbalte položku **připojení dat**, **objekt DefaultConnection**a potom **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="7e601-172">Expand **Data Connections**, **DefaultConnection**, and then **Tables**.</span></span> <span data-ttu-id="7e601-173">Vyhledejte hello **IssuingAuthorityKeys** tabulky, klikněte pravým tlačítkem ji a pak klikněte na tlačítko **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="7e601-173">Locate hello **IssuingAuthorityKeys** table, right-click it, and then click **Show Table Data**.</span></span>
3. <span data-ttu-id="7e601-174">V hello **IssuingAuthorityKeys** tabulky, budou existovat alespoň jeden řádek, který odpovídá toohello kryptografický otisk hodnotu pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-174">In hello **IssuingAuthorityKeys** table, there will be at least one row, which corresponds toohello thumbprint value for hello key.</span></span> <span data-ttu-id="7e601-175">Odstraňte všechny řádky v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-175">Delete any rows in hello table.</span></span>
4. <span data-ttu-id="7e601-176">Klikněte pravým tlačítkem na hello **klienty** tabulky a potom klikněte na **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="7e601-176">Right-click hello **Tenants** table, and then click **Show Table Data**.</span></span>
5. <span data-ttu-id="7e601-177">V hello **klienty** tabulky, budou existovat alespoň jeden řádek, který odpovídá identifikátor klienta tooa jedinečný adresáře.</span><span class="sxs-lookup"><span data-stu-id="7e601-177">In hello **Tenants** table, there will be at least one row, which corresponds tooa unique directory tenant identifier.</span></span> <span data-ttu-id="7e601-178">Odstraňte všechny řádky v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-178">Delete any rows in hello table.</span></span> <span data-ttu-id="7e601-179">Pokud nemáte odstraňování řádků hello v obou hello **klienty** tabulky a **IssuingAuthorityKeys** tabulky, bude dojde k chybě za běhu.</span><span class="sxs-lookup"><span data-stu-id="7e601-179">If you don't delete hello rows in both hello **Tenants** table and **IssuingAuthorityKeys** table, you will get an error at runtime.</span></span>
6. <span data-ttu-id="7e601-180">Sestavení a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-180">Build and run hello application.</span></span> <span data-ttu-id="7e601-181">Po přihlášení v účtu tooyour můžete zastavit aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-181">After you have logged in tooyour account, you can stop hello application.</span></span>
7. <span data-ttu-id="7e601-182">Vrátí toohello **Průzkumníka serveru** a prohlédněte si hello hodnoty v hello **IssuingAuthorityKeys** a **klienty** tabulky.</span><span class="sxs-lookup"><span data-stu-id="7e601-182">Return toohello **Server Explorer** and look at hello values in hello **IssuingAuthorityKeys** and **Tenants** table.</span></span> <span data-ttu-id="7e601-183">Můžete si všimnout, že budou mít byla automaticky jeho plnění znovu hello příslušné informace z dokument federačních metadat hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-183">You’ll notice that they have been automatically repopulated with hello appropriate information from hello federation metadata document.</span></span>

### <span data-ttu-id="7e601-184"><a name="vs2013"></a>Webová rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7e601-184"><a name="vs2013"></a>Web APIs protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="7e601-185">Pokud jste vytvořili webové aplikace rozhraní API v sadě Visual Studio 2013 pomocí šablony hello webového rozhraní API a pak vybrali **účty organizace** z hello **změna ověřování** nabídky, které již mají hello potřebné logiky aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e601-185">If you created a web API application in Visual Studio 2013 using hello Web API template, and then selected **Organizational Accounts** from hello **Change Authentication** menu, you already have hello necessary logic in your application.</span></span>

<span data-ttu-id="7e601-186">Pokud ručně nakonfigurované ověřování, postupujte podle pokynů hello níže toolearn jak tooconfigure vašeho webového rozhraní API tooautomatically aktualizovat informace o jeho klíči.</span><span class="sxs-lookup"><span data-stu-id="7e601-186">If you manually configured authentication, follow hello instructions below toolearn how tooconfigure your Web API tooautomatically update its key information.</span></span>

<span data-ttu-id="7e601-187">Hello následující fragment kódu ukazuje, jak tooget hello nejnovější klíče z hello dokument metadat federace a pak použít hello [obslužná rutina tokenu JWT](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello token.</span><span class="sxs-lookup"><span data-stu-id="7e601-187">hello following code snippet demonstrates how tooget hello latest keys from hello federation metadata document, and then use hello [JWT Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello token.</span></span> <span data-ttu-id="7e601-188">fragment kódu Hello předpokládá, že budete používat vlastní ukládání do mezipaměti mechanismus pro zachování budoucí klíče toovalidate hello tokeny z Azure AD, ať to do databáze, konfigurační soubor nebo jinde.</span><span class="sxs-lookup"><span data-stu-id="7e601-188">hello code snippet assumes that you will use your own caching mechanism for persisting hello key toovalidate future tokens from Azure AD, whether it be in a database, configuration file, or elsewhere.</span></span>

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

        // Validates hello JWT Token that's part of hello Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in hello Azure Classic Portal]",
                ValidIssuer = "[hello issuer for hello token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache hello signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from hello specified metadata document.
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
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in hello metadata");
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

### <span data-ttu-id="7e601-189"><a name="vs2012"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7e601-189"><a name="vs2012"></a>Web applications protecting resources and created with Visual Studio 2012</span></span>
<span data-ttu-id="7e601-190">Pokud vaše aplikace byla vytvořena v sadě Visual Studio 2012, pravděpodobně použijete hello identit a přístupu nástroj tooconfigure vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e601-190">If your application was built in Visual Studio 2012, you probably used hello Identity and Access Tool tooconfigure your application.</span></span> <span data-ttu-id="7e601-191">Je pravděpodobné, že používáte hello [ověřování vystavitele název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e601-191">It’s also likely that you are using hello [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span></span> <span data-ttu-id="7e601-192">Hello VINR zodpovídá za údržbu informace o poskytovatelích důvěryhodné identity (Azure AD) a používají klíče hello toovalidate tokeny vydané jimi.</span><span class="sxs-lookup"><span data-stu-id="7e601-192">hello VINR is responsible for maintaining information about trusted identity providers (Azure AD) and hello keys used toovalidate tokens issued by them.</span></span> <span data-ttu-id="7e601-193">Hello VINR také umožňuje snadno tooautomatically aktualizace hello klíčové informace uložené v souboru Web.config stažením hello nejnovější dokument metadat federation spojené s adresářem, kontrole, jestli se hello konfigurace je zastaralá s hello nejnovější dokument a aktualizuje hello aplikace toouse hello nového klíče podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="7e601-193">hello VINR also makes it easy tooautomatically update hello key information stored in a Web.config file by downloading hello latest federation metadata document associated with your directory, checking if hello configuration is out of date with hello latest document, and updating hello application toouse hello new key as necessary.</span></span>

<span data-ttu-id="7e601-194">Pokud jste vytvořili vaší aplikace pomocí některé z hello ukázky kódu nebo návod dokumentaci od společnosti Microsoft, logiku výměna klíče hello je již zahrnut ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="7e601-194">If you created your application using any of hello code samples or walkthrough documentation provided by Microsoft, hello key rollover logic is already included in your project.</span></span> <span data-ttu-id="7e601-195">Si všimnete, že hello kódu níže v projektu již existuje.</span><span class="sxs-lookup"><span data-stu-id="7e601-195">You will notice that hello code below already exists in your project.</span></span> <span data-ttu-id="7e601-196">Pokud vaše aplikace již nemá tuto logiku, postupujte podle hello kroků tooadd a tooverify, zda pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="7e601-196">If your application does not already have this logic, follow hello steps below tooadd it and tooverify that it’s working correctly.</span></span>

1. <span data-ttu-id="7e601-197">V **Průzkumníku řešení**, přidat odkaz na toohello **System.IdentityModel** sestavení pro příslušné projekt hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-197">In **Solution Explorer**, add a reference toohello **System.IdentityModel** assembly for hello appropriate project.</span></span>
2. <span data-ttu-id="7e601-198">Otevřete hello **Global.asax.cs** souboru a přidejte následující hello direktivy using:</span><span class="sxs-lookup"><span data-stu-id="7e601-198">Open hello **Global.asax.cs** file and add hello following using directives:</span></span>
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. <span data-ttu-id="7e601-199">Přidejte následující metodu toohello hello **Global.asax.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="7e601-199">Add hello following method toohello **Global.asax.cs** file:</span></span>
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. <span data-ttu-id="7e601-200">Vyvolání hello **RefreshValidationSettings()** metoda v hello **Application_Start()** metoda v **Global.asax.cs** znázorněné:</span><span class="sxs-lookup"><span data-stu-id="7e601-200">Invoke hello **RefreshValidationSettings()** method in hello **Application_Start()** method in **Global.asax.cs** as shown:</span></span>
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

<span data-ttu-id="7e601-201">Jakmile jste postupovali podle těchto kroků, souboru Web.config vaší aplikace bude aktualizována hello nejnovější informace z hello federační metadata dokumentu, včetně hello nejnovější klíče.</span><span class="sxs-lookup"><span data-stu-id="7e601-201">Once you have followed these steps, your application’s Web.config will be updated with hello latest information from hello federation metadata document, including hello latest keys.</span></span> <span data-ttu-id="7e601-202">Tato aktualizace se objeví pokaždé, když recykluje fond aplikací ve službě IIS; ve výchozím nastavení je služba IIS hodnotu toorecycle aplikace každých 29 hodin.</span><span class="sxs-lookup"><span data-stu-id="7e601-202">This update will occur every time your application pool recycles in IIS; by default IIS is set toorecycle applications every 29 hours.</span></span>

<span data-ttu-id="7e601-203">Postupujte podle kroků hello tooverify, zda pracuje logiku hello výměny klíčů.</span><span class="sxs-lookup"><span data-stu-id="7e601-203">Follow hello steps below tooverify that hello key rollover logic is working.</span></span>

1. <span data-ttu-id="7e601-204">Po ověření, že vaše aplikace používá hello kód výše, otevřete hello **Web.config** souboru a přejděte toohello  **<issuerNameRegistry>**  bloku, konkrétně hledá hello následující několika řádků:</span><span class="sxs-lookup"><span data-stu-id="7e601-204">After you have verified that your application is using hello code above, open hello **Web.config** file and navigate toohello **<issuerNameRegistry>** block, specifically looking for hello following few lines:</span></span>
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. <span data-ttu-id="7e601-205">V hello  **<add thumbprint=””>**  nastavení, změňte hodnotu kryptografický otisk hello nahrazením libovolný znak jiný.</span><span class="sxs-lookup"><span data-stu-id="7e601-205">In hello **<add thumbprint=””>** setting, change hello thumbprint value by replacing any character with a different one.</span></span> <span data-ttu-id="7e601-206">Uložit hello **Web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="7e601-206">Save hello **Web.config** file.</span></span>
3. <span data-ttu-id="7e601-207">Vytvoření aplikace hello a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="7e601-207">Build hello application, and then run it.</span></span> <span data-ttu-id="7e601-208">Pokud dokončíte proces přihlášení hello, vaše aplikace úspěšně aktualizuje klíč hello stažením hello vyžaduje informace ze svého adresáře na dokument metadat federace.</span><span class="sxs-lookup"><span data-stu-id="7e601-208">If you can complete hello sign-in process, your application is successfully updating hello key by downloading hello required information from your directory’s federation metadata document.</span></span> <span data-ttu-id="7e601-209">Pokud máte problémy s přihlášením, zkontrolujte hello změny ve vaší aplikaci jsou správné načtením hello [tooYour přidání přihlašování webové aplikace pomocí Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) tématu nebo stahování a zkontrolujete hello následující ukázka kódu: [ Víceklientská Cloudová aplikace pro Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span><span class="sxs-lookup"><span data-stu-id="7e601-209">If you are having issues signing in, ensure hello changes in your application are correct by reading hello [Adding Sign-On tooYour Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) topic, or downloading and inspecting hello following code sample: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span></span>

### <span data-ttu-id="7e601-210"><a name="vs2010"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2008 nebo 2010 a Windows Identity Foundation (WIF) verze 1.0 pro rozhraní .NET 3.5</span><span class="sxs-lookup"><span data-stu-id="7e601-210"><a name="vs2010"></a>Web applications protecting resources and created with Visual Studio 2008 or 2010 and Windows Identity Foundation (WIF) v1.0 for .NET 3.5</span></span>
<span data-ttu-id="7e601-211">Pokud jste vytvořili aplikaci na verzi WIF verze 1.0, není žádná aktualizace tooautomatically zadaný mechanismus toouse konfigurace vaší aplikace nový klíč.</span><span class="sxs-lookup"><span data-stu-id="7e601-211">If you built an application on WIF v1.0, there is no provided mechanism tooautomatically refresh your application’s configuration toouse a new key.</span></span>

* <span data-ttu-id="7e601-212">*Nejjednodušším způsobem, jak* pomocí nástrojů FedUtil hello součástí hello WIF SDK, která můžete načíst nejnovější dokument metadat hello a aktualizovat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7e601-212">*Easiest way* Use hello FedUtil tooling included in hello WIF SDK, which can retrieve hello latest metadata document and update your configuration.</span></span>
* <span data-ttu-id="7e601-213">Aktualizujte vaše aplikace too.NET 4.5, který obsahuje nejnovější verzi nachází v oboru názvů systému hello WIF hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-213">Update your application too.NET 4.5, which includes hello newest version of WIF located in hello System namespace.</span></span> <span data-ttu-id="7e601-214">Pak můžete použít hello [ověřování vystavitele název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform aplikace hello Konfigurace automatických aktualizací.</span><span class="sxs-lookup"><span data-stu-id="7e601-214">You can then use hello [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform automatic updates of hello application’s configuration.</span></span>
* <span data-ttu-id="7e601-215">Proveďte ruční výměna podle pokynů hello na konci hello tohoto dokumentu pokyny.</span><span class="sxs-lookup"><span data-stu-id="7e601-215">Perform a manual rollover as per hello instructions at hello end of this guidance document.</span></span>

<span data-ttu-id="7e601-216">Pokyny toouse hello FedUtil tooupdate konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="7e601-216">Instructions toouse hello FedUtil tooupdate your configuration:</span></span>

1. <span data-ttu-id="7e601-217">Ověřte, zda máte hello WIF v1.0 SDK pro Visual Studio 2008 nebo 2010 nainstalována na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="7e601-217">Verify that you have hello WIF v1.0 SDK installed on your development machine for Visual Studio 2008 or 2010.</span></span> <span data-ttu-id="7e601-218">Můžete [stáhnout odsud](https://www.microsoft.com/en-us/download/details.aspx?id=4451) Pokud jste ještě nenainstalovali ho.</span><span class="sxs-lookup"><span data-stu-id="7e601-218">You can [download it from here](https://www.microsoft.com/en-us/download/details.aspx?id=4451) if you have not yet installed it.</span></span>
2. <span data-ttu-id="7e601-219">V sadě Visual Studio otevřete hello řešení a potom klikněte pravým tlačítkem na příslušné projektu hello a vyberte **aktualizace federačních metadat**.</span><span class="sxs-lookup"><span data-stu-id="7e601-219">In Visual Studio, open hello solution, and then right-click hello applicable project and select **Update federation metadata**.</span></span> <span data-ttu-id="7e601-220">Pokud tato možnost není k dispozici, FedUtil nebo hello WIF verze 1.0 SDK nebyl nainstalován.</span><span class="sxs-lookup"><span data-stu-id="7e601-220">If this option is not available, FedUtil and/or hello WIF v1.0 SDK has not been installed.</span></span>
3. <span data-ttu-id="7e601-221">Z příkazového řádku hello vyberte **aktualizace** toobegin aktualizace federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="7e601-221">From hello prompt, select **Update** toobegin updating your federation metadata.</span></span> <span data-ttu-id="7e601-222">Pokud máte prostředí serveru toohello přístupu je hostitelem aplikace hello, můžete volitelně použít na FedUtil [scheduler automatické metadata aktualizace](https://msdn.microsoft.com/library/ee517272.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e601-222">If you have access toohello server environment where hello application is hosted, you can optionally use FedUtil’s [automatic metadata update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).</span></span>
4. <span data-ttu-id="7e601-223">Klikněte na tlačítko **Dokončit** procesu aktualizace toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-223">Click **Finish** toocomplete hello update process.</span></span>

### <span data-ttu-id="7e601-224"><a name="other"></a>Webové aplikace / rozhraní API Ochrana prostředků pomocí kterékoli jiné knihovny nebo ručně implementací hello podporované protokoly</span><span class="sxs-lookup"><span data-stu-id="7e601-224"><a name="other"></a>Web applications / APIs protecting resources using any other libraries or manually implementing any of hello supported protocols</span></span>
<span data-ttu-id="7e601-225">Pokud používáte některé jiné knihovny nebo ručně implementované všechny hello podporované protokoly, budete potřebovat tooreview hello knihovně nebo tooensure vaší implementace, která hello klíč je načítány z hello OpenID Connect zjišťování dokumentu nebo hello Dokument metadat federace.</span><span class="sxs-lookup"><span data-stu-id="7e601-225">If you are using some other library or manually implemented any of hello supported protocols, you'll need tooreview hello library or your implementation tooensure that hello key is being retrieved from either hello OpenID Connect discovery document or hello federation metadata document.</span></span> <span data-ttu-id="7e601-226">Jedním ze způsobů toocheck pro tento je toodo vyhledávání v kódu nebo kód hello knihovny pro všechny hovory se tooeither hello OpenID zjišťování dokument nebo dokument federačních metadat hello.</span><span class="sxs-lookup"><span data-stu-id="7e601-226">One way toocheck for this is toodo a search in your code or hello library's code for any calls out tooeither hello OpenID discovery document or hello federation metadata document.</span></span>

<span data-ttu-id="7e601-227">Pokud se klíče ukládají někde nebo pevně zakódované ve vaší aplikaci, můžete ručně načíst klíč hello a aktualizovat ji odpovídajícím způsobem podle provést ruční výměna podle pokynů hello na konci hello tohoto dokumentu pokyny.</span><span class="sxs-lookup"><span data-stu-id="7e601-227">If they key is being stored somewhere or hardcoded in your application, you can manually retrieve hello key and update it accordingly by perform a manual rollover as per hello instructions at hello end of this guidance document.</span></span> <span data-ttu-id="7e601-228">**Důrazně doporučujeme, můžete zvýšit vaše aplikace toosupport automatické výměny** pomocí kteréhokoli hello blíží obrysu v tomto článku tooavoid budoucí přerušení a režijní náklady, pokud Azure AD zvyšuje výměny cadence nebo má Nouzový out-of-band výměny.</span><span class="sxs-lookup"><span data-stu-id="7e601-228">**It is strongly encouraged that you enhance your application toosupport automatic rollover** using any of hello approaches outline in this article tooavoid future disruptions and overhead if Azure AD increases it's rollover cadence or has an emergency out-of-band rollover.</span></span>

## <a name="how-tootest-your-application-toodetermine-if-it-will-be-affected"></a><span data-ttu-id="7e601-229">Jak tootest toodetermine vaší aplikace, pokud bude mít vliv</span><span class="sxs-lookup"><span data-stu-id="7e601-229">How tootest your application toodetermine if it will be affected</span></span>
<span data-ttu-id="7e601-230">Můžete ověřit, jestli aplikace podporuje automatickou výměnu klíče stažením hello skripty a pokynů hello v [toto úložiště GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span><span class="sxs-lookup"><span data-stu-id="7e601-230">You can validate whether your application supports automatic key rollover by downloading hello scripts and following hello instructions in [this GitHub repository.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span></span>

## <a name="how-tooperform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a><span data-ttu-id="7e601-231">Jak tooperform ruční výměna, pokud je aplikace nepodporuje automatické výměny</span><span class="sxs-lookup"><span data-stu-id="7e601-231">How tooperform a manual rollover if you application does not support automatic rollover</span></span>
<span data-ttu-id="7e601-232">Pokud aplikace nemá **není** podporují automatické výměny, budete potřebovat tooestablish proces, který pravidelně monitorování Azure AD podepisovací klíče a provede ruční výměna odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="7e601-232">If your application does **not** support automatic rollover, you will need tooestablish a process that periodically monitors Azure AD's signing keys and performs a manual rollover accordingly.</span></span> <span data-ttu-id="7e601-233">[Toto úložiště GitHub](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) obsahuje skripty a pokyny, jak toodo to.</span><span class="sxs-lookup"><span data-stu-id="7e601-233">[This GitHub repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) contains scripts and instructions on how toodo this.</span></span>

