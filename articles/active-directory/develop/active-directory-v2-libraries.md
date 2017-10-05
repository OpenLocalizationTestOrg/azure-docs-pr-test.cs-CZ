---
title: "Knihovny ověřování Azure Active Directory v2.0 | Microsoft Docs"
description: "Kompatibilní klientské knihovny a knihovny middlewaru serveru a související knihovny, zdroje a odkazy ukázky pro koncový bod v2.0 Azure Active Directory."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: c8dc0fab1a4da2bf9ae1463cb5e17fc2c2b12e5c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-v20-authentication-libraries"></a><span data-ttu-id="aca2c-103">Knihovny ověřování Azure Active Directory v2.0</span><span class="sxs-lookup"><span data-stu-id="aca2c-103">Azure Active Directory v2.0 authentication libraries</span></span>
<span data-ttu-id="aca2c-104">Koncový bod v2.0 Azure Active Directory (Azure AD) podporuje standardní protokoly OAuth 2.0 a OpenID Connect 1.0.</span><span class="sxs-lookup"><span data-stu-id="aca2c-104">The Azure Active Directory (Azure AD) v2.0 endpoint supports the industry-standard OAuth 2.0 and OpenID Connect 1.0 protocols.</span></span> <span data-ttu-id="aca2c-105">Můžete použít různé knihovny od společnosti Microsoft a jiných organizací s koncovým bodem v2.0.</span><span class="sxs-lookup"><span data-stu-id="aca2c-105">You can use various libraries from Microsoft and other organizations with the v2.0 endpoint.</span></span>

<span data-ttu-id="aca2c-106">Když vytvoříte aplikaci, která používá koncový bod v2.0, doporučujeme používat knihovny, které jsou zapsány odborníky protokolu domény, kteří podle metodika životního cyklu SDL (Security Development), jako je třeba [ten následuje Microsoft][Microsoft-SDL].</span><span class="sxs-lookup"><span data-stu-id="aca2c-106">When you build an application that uses the v2.0 endpoint, we recommend that you use libraries that are written by protocol domain experts who follow a Security Development Lifecycle (SDL) methodology, like [the one followed by Microsoft][Microsoft-SDL].</span></span> <span data-ttu-id="aca2c-107">Pokud se rozhodnete k ruční kódu podporu pro protokoly, doporučujeme, postupujte podle SDL metodika a zaměřit na aspekty zabezpečení do specifikací standardy pro každý protokol.</span><span class="sxs-lookup"><span data-stu-id="aca2c-107">If you decide to hand-code support for the protocols, we recommend you follow SDL methodology and pay close attention to the security considerations in the standards specifications for each protocol.</span></span>

## <a name="types-of-libraries"></a><span data-ttu-id="aca2c-108">Knihovny typů</span><span class="sxs-lookup"><span data-stu-id="aca2c-108">Types of libraries</span></span>
<span data-ttu-id="aca2c-109">Azure AD v2.0 funguje s dva typy knihovny:</span><span class="sxs-lookup"><span data-stu-id="aca2c-109">Azure AD v2.0 works with two types of libraries:</span></span>

* <span data-ttu-id="aca2c-110">**Knihovny klienta**.</span><span class="sxs-lookup"><span data-stu-id="aca2c-110">**Client libraries**.</span></span> <span data-ttu-id="aca2c-111">Nativní klienty a servery používat k získání přístupových tokenů pro volání prostředku, jako je například Microsoft Graph knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="aca2c-111">Native clients and servers use client libraries to get access tokens for calling a resource, such as Microsoft Graph.</span></span>
* <span data-ttu-id="aca2c-112">**Server knihovny middleware**.</span><span class="sxs-lookup"><span data-stu-id="aca2c-112">**Server middleware libraries**.</span></span> <span data-ttu-id="aca2c-113">Webové aplikace používat server knihovny middleware pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="aca2c-113">Web apps use server middleware libraries for user sign-in.</span></span> <span data-ttu-id="aca2c-114">Webová rozhraní API slouží k ověření tokeny, které se odesílají klienti nativní nebo jinými servery knihovny middlewaru serveru.</span><span class="sxs-lookup"><span data-stu-id="aca2c-114">Web APIs use server middleware libraries to validate tokens that are sent by native clients or by other servers.</span></span>

## <a name="library-support"></a><span data-ttu-id="aca2c-115">Podpora knihovny</span><span class="sxs-lookup"><span data-stu-id="aca2c-115">Library support</span></span>
<span data-ttu-id="aca2c-116">Protože všechny standardům knihovny můžete vybrat, kdy používáte koncový bod v2.0, je důležité vědět, kde lze získat podporu.</span><span class="sxs-lookup"><span data-stu-id="aca2c-116">Because you can choose any standards-compliant library when you use the v2.0 endpoint, it’s important to know where to go for support.</span></span> <span data-ttu-id="aca2c-117">Pro problémy a žádosti o funkce v kódu knihovny obraťte se na vlastníka knihovny.</span><span class="sxs-lookup"><span data-stu-id="aca2c-117">For issues and feature requests in library code, contact the library owner.</span></span> <span data-ttu-id="aca2c-118">Problémy a žádosti o funkce v implementaci protokolu na straně služby od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="aca2c-118">For issues and feature requests in the service-side protocol implementation, contact Microsoft.</span></span>

<span data-ttu-id="aca2c-119">Knihovny se ve dvou kategoriích podpory:</span><span class="sxs-lookup"><span data-stu-id="aca2c-119">Libraries come in two support categories:</span></span>

* <span data-ttu-id="aca2c-120">**Podporované Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="aca2c-120">**Microsoft-supported**.</span></span> <span data-ttu-id="aca2c-121">Společnost Microsoft poskytuje opravy pro tyto knihovny a provedla SDL kvůli opatrností na tyto knihovny.</span><span class="sxs-lookup"><span data-stu-id="aca2c-121">Microsoft provides fixes for these libraries, and has done SDL due diligence on these libraries.</span></span>
* <span data-ttu-id="aca2c-122">**Kompatibilní**.</span><span class="sxs-lookup"><span data-stu-id="aca2c-122">**Compatible**.</span></span> <span data-ttu-id="aca2c-123">Společnost Microsoft tyto knihovny otestována základní scénáře a potvrdit, že fungují s koncovým bodem v2.0.</span><span class="sxs-lookup"><span data-stu-id="aca2c-123">Microsoft has tested these libraries in basic scenarios and confirmed that they work with the v2.0 endpoint.</span></span> <span data-ttu-id="aca2c-124">Společnost Microsoft neposkytuje opravy pro tyto knihovny a nebyl provádí kontrolu tyto knihovny.</span><span class="sxs-lookup"><span data-stu-id="aca2c-124">Microsoft does not provide fixes for these libraries and has not done a review of these libraries.</span></span> <span data-ttu-id="aca2c-125">Problémy a žádosti o funkce se mají směrovat knihovny open-source projekt.</span><span class="sxs-lookup"><span data-stu-id="aca2c-125">Issues and feature requests should be directed to the library’s open-source project.</span></span>

<span data-ttu-id="aca2c-126">Seznam knihovny, které pracují s koncovým bodem v2.0 naleznete v dalších částech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="aca2c-126">For a list of libraries that work with the v2.0 endpoint, see the next sections in this article.</span></span>


## <a name="microsoft-supported-client-libraries"></a><span data-ttu-id="aca2c-127">Knihovny Microsoft podporované klienta</span><span class="sxs-lookup"><span data-stu-id="aca2c-127">Microsoft-supported client libraries</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aca2c-128">Knihovny preview MSAL jsou vhodné pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="aca2c-128">The MSAL preview libraries are suitable for use in a production environment.</span></span> <span data-ttu-id="aca2c-129">Poskytujeme produkční úrovni podporu pro tyto knihovny jako naše aktuální produkční knihovny (ADAL).</span><span class="sxs-lookup"><span data-stu-id="aca2c-129">We provide the same production level support for these libraries as we do our current production libraries (ADAL).</span></span> <span data-ttu-id="aca2c-130">Ve verzi Preview můžeme provést změny rozhraní API MSAL, formát vnitřní mezipaměti a další mechanismy tyto knihovny bez upozornění, která bude nutné provést společně s opravy chyb nebo vylepšení funkcí.</span><span class="sxs-lookup"><span data-stu-id="aca2c-130">During the preview we may make changes to the MSAL API, internal cache format, and other mechanisms of these libraries without notice, which you will be required to take along with bug fixes or feature improvements.</span></span> <span data-ttu-id="aca2c-131">To může mít vliv na vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="aca2c-131">This may impact your application.</span></span> <span data-ttu-id="aca2c-132">Ke změně formátu mezipaměti, může mít vliv uživatelům, jako je například požadavek, aby znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="aca2c-132">For instance, a change to the cache format may impact your users, such as requiring them to sign in again.</span></span> <span data-ttu-id="aca2c-133">O rozhraní API změnu může vyžadovat aktualizaci vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="aca2c-133">An API change may require you to update your code.</span></span> <span data-ttu-id="aca2c-134">Když jsme poskytují obecné dostupnosti verze, bude vyžadovat aktualizace na verzi obecné dostupnosti šest měsíců, jako aplikace napsané v náhledu verzi knihovny může přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="aca2c-134">When we provide the General Availability release we will require you to update to the General Availability version within six months, as applications written using a preview version of library may no longer work.</span></span>

| <span data-ttu-id="aca2c-135">Platforma</span><span class="sxs-lookup"><span data-stu-id="aca2c-135">Platform</span></span> | <span data-ttu-id="aca2c-136">Knihovna</span><span class="sxs-lookup"><span data-stu-id="aca2c-136">Library</span></span> | <span data-ttu-id="aca2c-137">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="aca2c-137">Download</span></span> | <span data-ttu-id="aca2c-138">Zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="aca2c-138">Source Code</span></span> | <span data-ttu-id="aca2c-139">Ukázka</span><span class="sxs-lookup"><span data-stu-id="aca2c-139">Sample</span></span> | <span data-ttu-id="aca2c-140">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="aca2c-140">Reference</span></span>
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="aca2c-141">Klient .NET, Windows Store, UWP Xamarin iOS a Android</span><span class="sxs-lookup"><span data-stu-id="aca2c-141">.NET Client, Windows Store, UWP, Xamarin iOS and Android</span></span> | <span data-ttu-id="aca2c-142">MSAL .NET (Preview)</span><span class="sxs-lookup"><span data-stu-id="aca2c-142">MSAL .NET (Preview)</span></span> |[<span data-ttu-id="aca2c-143">NuGet</span><span class="sxs-lookup"><span data-stu-id="aca2c-143">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Identity.Client) |[<span data-ttu-id="aca2c-144">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-144">GitHub</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [<span data-ttu-id="aca2c-145">Aplikace na ploše</span><span class="sxs-lookup"><span data-stu-id="aca2c-145">Desktop App</span></span>](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| <span data-ttu-id="aca2c-146">JavaScript</span><span class="sxs-lookup"><span data-stu-id="aca2c-146">JavaScript</span></span> | <span data-ttu-id="aca2c-147">MSAL.js (Preview)</span><span class="sxs-lookup"><span data-stu-id="aca2c-147">MSAL.js (Preview)</span></span> | [<span data-ttu-id="aca2c-148">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-148">GitHub</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [<span data-ttu-id="aca2c-149">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-149">GitHub</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [<span data-ttu-id="aca2c-150">Jedna stránka aplikace</span><span class="sxs-lookup"><span data-stu-id="aca2c-150">Single Page App</span></span>](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| <span data-ttu-id="aca2c-151">iOS, systému macOS</span><span class="sxs-lookup"><span data-stu-id="aca2c-151">iOS, macOS</span></span> | <span data-ttu-id="aca2c-152">MSAL (Preview)</span><span class="sxs-lookup"><span data-stu-id="aca2c-152">MSAL (Preview)</span></span> | [<span data-ttu-id="aca2c-153">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-153">GitHub</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[<span data-ttu-id="aca2c-154">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-154">GitHub</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [<span data-ttu-id="aca2c-155">Aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="aca2c-155">iOS App</span></span>](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| <span data-ttu-id="aca2c-156">Android</span><span class="sxs-lookup"><span data-stu-id="aca2c-156">Android</span></span> | <span data-ttu-id="aca2c-157">MSAL (Preview)</span><span class="sxs-lookup"><span data-stu-id="aca2c-157">MSAL (Preview)</span></span> | [<span data-ttu-id="aca2c-158">Centrální úložiště</span><span class="sxs-lookup"><span data-stu-id="aca2c-158">The Central Repository</span></span>](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[<span data-ttu-id="aca2c-159">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-159">GitHub</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [<span data-ttu-id="aca2c-160">Aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="aca2c-160">Android App</span></span>](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [<span data-ttu-id="aca2c-161">JavaDocs</span><span class="sxs-lookup"><span data-stu-id="aca2c-161">JavaDocs</span></span>](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a><span data-ttu-id="aca2c-162">Knihovny Microsoft podporované server middleware</span><span class="sxs-lookup"><span data-stu-id="aca2c-162">Microsoft-supported server middleware libraries</span></span>

| <span data-ttu-id="aca2c-163">Platforma</span><span class="sxs-lookup"><span data-stu-id="aca2c-163">Platform</span></span> | <span data-ttu-id="aca2c-164">Knihovna</span><span class="sxs-lookup"><span data-stu-id="aca2c-164">Library</span></span> | <span data-ttu-id="aca2c-165">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="aca2c-165">Download</span></span> | <span data-ttu-id="aca2c-166">Zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="aca2c-166">Source Code</span></span> | <span data-ttu-id="aca2c-167">Ukázka</span><span class="sxs-lookup"><span data-stu-id="aca2c-167">Sample</span></span> | <span data-ttu-id="aca2c-168">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="aca2c-168">Reference</span></span>
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="aca2c-169">Rozhraní .NET 4.x</span><span class="sxs-lookup"><span data-stu-id="aca2c-169">.NET 4.x</span></span> | <span data-ttu-id="aca2c-170">Middleware OWIN OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="aca2c-170">OWIN OpenID Connect middleware</span></span> |[<span data-ttu-id="aca2c-171">NuGet</span><span class="sxs-lookup"><span data-stu-id="aca2c-171">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[<span data-ttu-id="aca2c-172">Webu CodePlex</span><span class="sxs-lookup"><span data-stu-id="aca2c-172">CodePlex</span></span>](http://katanaproject.codeplex.com) |[<span data-ttu-id="aca2c-173">Aplikace MVC</span><span class="sxs-lookup"><span data-stu-id="aca2c-173">MVC App</span></span>](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| <span data-ttu-id="aca2c-174">Rozhraní .NET 4.x</span><span class="sxs-lookup"><span data-stu-id="aca2c-174">.NET 4.x</span></span> | <span data-ttu-id="aca2c-175">Middleware OWIN nosiče OAuth pro AzureAD</span><span class="sxs-lookup"><span data-stu-id="aca2c-175">OWIN OAuth Bearer middleware for AzureAD</span></span> |[<span data-ttu-id="aca2c-176">NuGet</span><span class="sxs-lookup"><span data-stu-id="aca2c-176">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[<span data-ttu-id="aca2c-177">Webu CodePlex</span><span class="sxs-lookup"><span data-stu-id="aca2c-177">CodePlex</span></span>](http://katanaproject.codeplex.com) |  | |
| <span data-ttu-id="aca2c-178">Rozhraní .NET 4.x</span><span class="sxs-lookup"><span data-stu-id="aca2c-178">.NET 4.x</span></span> | <span data-ttu-id="aca2c-179">Obslužná rutina JWT pro rozhraní .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="aca2c-179">JWT Handler for .NET 4.5</span></span> | [<span data-ttu-id="aca2c-180">NuGet</span><span class="sxs-lookup"><span data-stu-id="aca2c-180">NuGet</span></span>](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [<span data-ttu-id="aca2c-181">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-181">GitHub</span></span>](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| <span data-ttu-id="aca2c-182">.NET Core</span><span class="sxs-lookup"><span data-stu-id="aca2c-182">.NET Core</span></span> | <span data-ttu-id="aca2c-183">Middleware OpenID Connect ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aca2c-183">ASP.NET OpenID Connect middleware</span></span> |<span data-ttu-id="aca2c-184">[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib]</span><span class="sxs-lookup"><span data-stu-id="aca2c-184">[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib]</span></span> |<span data-ttu-id="aca2c-185">[Zabezpečení technologie ASP.NET (Githubu)][ServerLib-NetCore-Owin-Oidc-Repo]</span><span class="sxs-lookup"><span data-stu-id="aca2c-185">[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo]</span></span> |[<span data-ttu-id="aca2c-186">Aplikace MVC</span><span class="sxs-lookup"><span data-stu-id="aca2c-186">MVC app</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| <span data-ttu-id="aca2c-187">.NET Core</span><span class="sxs-lookup"><span data-stu-id="aca2c-187">.NET Core</span></span> | <span data-ttu-id="aca2c-188">Middleware nosiče OAuth ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aca2c-188">ASP.NET OAuth Bearer middleware</span></span> |<span data-ttu-id="aca2c-189">[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib]</span><span class="sxs-lookup"><span data-stu-id="aca2c-189">[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib]</span></span> |<span data-ttu-id="aca2c-190">[Zabezpečení technologie ASP.NET (Githubu)][ServerLib-NetCore-Owin-Oauth-Repo]</span><span class="sxs-lookup"><span data-stu-id="aca2c-190">[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo]</span></span> |  |
| <span data-ttu-id="aca2c-191">.NET Core</span><span class="sxs-lookup"><span data-stu-id="aca2c-191">.NET Core</span></span> | <span data-ttu-id="aca2c-192">Obslužná rutina JWT pro .NET Core</span><span class="sxs-lookup"><span data-stu-id="aca2c-192">JWT Handler for .NET Core</span></span>  |[<span data-ttu-id="aca2c-193">NuGet</span><span class="sxs-lookup"><span data-stu-id="aca2c-193">NuGet</span></span>](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[<span data-ttu-id="aca2c-194">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-194">GitHub</span></span>](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| <span data-ttu-id="aca2c-195">Node.js</span><span class="sxs-lookup"><span data-stu-id="aca2c-195">Node.js</span></span> |<span data-ttu-id="aca2c-196">Azure AD Passport</span><span class="sxs-lookup"><span data-stu-id="aca2c-196">Azure AD Passport</span></span> |[<span data-ttu-id="aca2c-197">npm</span><span class="sxs-lookup"><span data-stu-id="aca2c-197">npm</span></span>](https://www.npmjs.com/package/passport-azure-ad) |[<span data-ttu-id="aca2c-198">GitHub</span><span class="sxs-lookup"><span data-stu-id="aca2c-198">GitHub</span></span>](https://github.com/AzureAD/passport-azure-ad) | [<span data-ttu-id="aca2c-199">Webové aplikace</span><span class="sxs-lookup"><span data-stu-id="aca2c-199">Web app</span></span>](active-directory-v2-devquickstarts-node-web.md)| |

## <a name="compatible-client-libraries"></a><span data-ttu-id="aca2c-200">Kompatibilní klient knihovny</span><span class="sxs-lookup"><span data-stu-id="aca2c-200">Compatible client libraries</span></span>
| <span data-ttu-id="aca2c-201">Platforma</span><span class="sxs-lookup"><span data-stu-id="aca2c-201">Platform</span></span> | <span data-ttu-id="aca2c-202">Název knihovny</span><span class="sxs-lookup"><span data-stu-id="aca2c-202">Library name</span></span> | <span data-ttu-id="aca2c-203">Otestované verze</span><span class="sxs-lookup"><span data-stu-id="aca2c-203">Tested version</span></span> | <span data-ttu-id="aca2c-204">Zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="aca2c-204">Source code</span></span> | <span data-ttu-id="aca2c-205">Ukázka</span><span class="sxs-lookup"><span data-stu-id="aca2c-205">Sample</span></span> |
|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="aca2c-206">Android</span><span class="sxs-lookup"><span data-stu-id="aca2c-206">Android</span></span> |[<span data-ttu-id="aca2c-207">OIDCAndroidLib</span><span class="sxs-lookup"><span data-stu-id="aca2c-207">OIDCAndroidLib</span></span>](https://github.com/kalemontes/OIDCAndroidLib/wiki) |<span data-ttu-id="aca2c-208">0.2.1</span><span class="sxs-lookup"><span data-stu-id="aca2c-208">0.2.1</span></span> |[<span data-ttu-id="aca2c-209">OIDCAndroidLib</span><span class="sxs-lookup"><span data-stu-id="aca2c-209">OIDCAndroidLib</span></span>](https://github.com/kalemontes/OIDCAndroidLib) |[<span data-ttu-id="aca2c-210">Ukázka nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="aca2c-210">Native app sample</span></span>](active-directory-v2-devquickstarts-android.md) |
| <span data-ttu-id="aca2c-211">iOS</span><span class="sxs-lookup"><span data-stu-id="aca2c-211">iOS</span></span> |[<span data-ttu-id="aca2c-212">NXOAuth2Client</span><span class="sxs-lookup"><span data-stu-id="aca2c-212">NXOAuth2Client</span></span>](https://github.com/nxtbgthng/OAuth2Client) |<span data-ttu-id="aca2c-213">1.2.8</span><span class="sxs-lookup"><span data-stu-id="aca2c-213">1.2.8</span></span> |[<span data-ttu-id="aca2c-214">NXOAuth2Client</span><span class="sxs-lookup"><span data-stu-id="aca2c-214">NXOAuth2Client</span></span>](https://github.com/nxtbgthng/OAuth2Client) |[<span data-ttu-id="aca2c-215">Ukázka nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="aca2c-215">Native app sample</span></span>](active-directory-v2-devquickstarts-ios.md) |
| <span data-ttu-id="aca2c-216">JavaScript</span><span class="sxs-lookup"><span data-stu-id="aca2c-216">JavaScript</span></span> |[<span data-ttu-id="aca2c-217">Hello.js</span><span class="sxs-lookup"><span data-stu-id="aca2c-217">Hello.js</span></span>](https://adodson.com/hello.js/) |<span data-ttu-id="aca2c-218">1.13.5</span><span class="sxs-lookup"><span data-stu-id="aca2c-218">1.13.5</span></span> |[<span data-ttu-id="aca2c-219">Hello.js</span><span class="sxs-lookup"><span data-stu-id="aca2c-219">Hello.js</span></span>](https://github.com/MrSwitch/hello.js) |[<span data-ttu-id="aca2c-220">SPA</span><span class="sxs-lookup"><span data-stu-id="aca2c-220">SPA</span></span>](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |

## <a name="compatible-server-middleware-libraries"></a><span data-ttu-id="aca2c-221">Server kompatibilní middleware knihovny</span><span class="sxs-lookup"><span data-stu-id="aca2c-221">Compatible server middleware libraries</span></span>
| <span data-ttu-id="aca2c-222">Platforma</span><span class="sxs-lookup"><span data-stu-id="aca2c-222">Platform</span></span> | <span data-ttu-id="aca2c-223">Název knihovny</span><span class="sxs-lookup"><span data-stu-id="aca2c-223">Library name</span></span> | <span data-ttu-id="aca2c-224">Otestované verze</span><span class="sxs-lookup"><span data-stu-id="aca2c-224">Tested version</span></span> | <span data-ttu-id="aca2c-225">Zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="aca2c-225">Source code</span></span> | <span data-ttu-id="aca2c-226">Ukázka</span><span class="sxs-lookup"><span data-stu-id="aca2c-226">Sample</span></span> |
|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="aca2c-227">Java</span><span class="sxs-lookup"><span data-stu-id="aca2c-227">Java</span></span> | [<span data-ttu-id="aca2c-228">Scribejava Scribe Java</span><span class="sxs-lookup"><span data-stu-id="aca2c-228">Scribe Java scribejava</span></span>](https://github.com/scribejava/scribejava) | [<span data-ttu-id="aca2c-229">Verze 3.2.0</span><span class="sxs-lookup"><span data-stu-id="aca2c-229">Version 3.2.0</span></span>](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [<span data-ttu-id="aca2c-230">ScribeJava</span><span class="sxs-lookup"><span data-stu-id="aca2c-230">ScribeJava</span></span>](https://github.com/scribejava/scribejava/archive/scribejava-3.2.0.zip) | |
| <span data-ttu-id="aca2c-231">PHP</span><span class="sxs-lookup"><span data-stu-id="aca2c-231">PHP</span></span> | [<span data-ttu-id="aca2c-232">PHP ligy oauth2 klienta</span><span class="sxs-lookup"><span data-stu-id="aca2c-232">The PHP League oauth2-client</span></span>](https://github.com/thephpleague/oauth2-client) | [<span data-ttu-id="aca2c-233">Verze 1.4.2</span><span class="sxs-lookup"><span data-stu-id="aca2c-233">Version 1.4.2</span></span>](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [<span data-ttu-id="aca2c-234">oauth2 klienta</span><span class="sxs-lookup"><span data-stu-id="aca2c-234">oauth2-client</span></span>](https://github.com/thephpleague/oauth2-client/archive/1.4.2.zip) | |
| <span data-ttu-id="aca2c-235">Python Flask</span><span class="sxs-lookup"><span data-stu-id="aca2c-235">Python-Flask</span></span> |[<span data-ttu-id="aca2c-236">Flask OAuthlib</span><span class="sxs-lookup"><span data-stu-id="aca2c-236">Flask-OAuthlib</span></span>](https://github.com/lepture/flask-oauthlib) |<span data-ttu-id="aca2c-237">0.9.3</span><span class="sxs-lookup"><span data-stu-id="aca2c-237">0.9.3</span></span> |[<span data-ttu-id="aca2c-238">Flask OAuthlib</span><span class="sxs-lookup"><span data-stu-id="aca2c-238">Flask-OAuthlib</span></span>](https://github.com/lepture/flask-oauthlib) |[<span data-ttu-id="aca2c-239">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="aca2c-239">Web App</span></span>](https://github.com/Azure-Samples/active-directory-python-flask-graphapi-web-v2) |
| <span data-ttu-id="aca2c-240">Ruby</span><span class="sxs-lookup"><span data-stu-id="aca2c-240">Ruby</span></span> |[<span data-ttu-id="aca2c-241">OmniAuth</span><span class="sxs-lookup"><span data-stu-id="aca2c-241">OmniAuth</span></span>](https://github.com/omniauth/omniauth/wiki) |<span data-ttu-id="aca2c-242">omniauth:1.3.1</span><span class="sxs-lookup"><span data-stu-id="aca2c-242">omniauth:1.3.1</span></span></br><span data-ttu-id="aca2c-243">omniauth-oauth2:1.4.0</span><span class="sxs-lookup"><span data-stu-id="aca2c-243">omniauth-oauth2:1.4.0</span></span> |[<span data-ttu-id="aca2c-244">OmniAuth</span><span class="sxs-lookup"><span data-stu-id="aca2c-244">OmniAuth</span></span>](https://github.com/omniauth/omniauth)</br>[<span data-ttu-id="aca2c-245">OmniAuth OAuth2</span><span class="sxs-lookup"><span data-stu-id="aca2c-245">OmniAuth OAuth2</span></span>](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a><span data-ttu-id="aca2c-246">Související obsah</span><span class="sxs-lookup"><span data-stu-id="aca2c-246">Related content</span></span>
<span data-ttu-id="aca2c-247">Další informace o koncový bod v2.0 Azure AD, najdete v článku [přehled v2.0 modelu aplikace Azure AD][AAD-App-Model-V2-Overview].</span><span class="sxs-lookup"><span data-stu-id="aca2c-247">For more information about the Azure AD v2.0 endpoint, see the [Azure AD app model v2.0 overview][AAD-App-Model-V2-Overview].</span></span>

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/