---
title: "Azure AD v2 Windows Desktop získávání spuštěno – Úvod | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4a695c00fce4deb02261ba58ec95469746bb1486
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a><span data-ttu-id="50f01-103">Volání Microsoft Graph API z aplikace na ploše systému Windows</span><span class="sxs-lookup"><span data-stu-id="50f01-103">Call the Microsoft Graph API from a Windows Desktop app</span></span>

<span data-ttu-id="50f01-104">Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace Windows Desktop .NET (XAML).</span><span class="sxs-lookup"><span data-stu-id="50f01-104">This guide demonstrates how a native Windows Desktop .NET (XAML) application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="50f01-105">Na konci tohoto průvodce bude moci volat chráněné API používání osobních účtů (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má Azure Active Directory vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="50f01-105">At the end of this guide, your application will be able to call a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

> <span data-ttu-id="50f01-106">Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="50f01-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="50f01-107">Nemáte ho?</span><span class="sxs-lookup"><span data-stu-id="50f01-107">Don’t have it?</span></span> [<span data-ttu-id="50f01-108">Stáhněte si Visual Studio 2017 zdarma</span><span class="sxs-lookup"><span data-stu-id="50f01-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a><span data-ttu-id="50f01-109">Jak funguje tato příručka</span><span class="sxs-lookup"><span data-stu-id="50f01-109">How this guide works</span></span>

![Jak funguje tato příručka](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

<span data-ttu-id="50f01-111">Ukázkové aplikace vytvořené v této příručce umožňuje aplikace Windows Desktop dotazovat Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="50f01-111">The sample application created by this guide enables a Windows Desktop Application to query Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="50f01-112">V tomto scénáři se token přidá na požadavky HTTP přes autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="50f01-112">For this scenario, a token is added to HTTP requests via the Authorization header.</span></span> <span data-ttu-id="50f01-113">Získání tokenu a obnova zpracovává pomocí knihovny Microsoft ověřování (MSAL).</span><span class="sxs-lookup"><span data-stu-id="50f01-113">Token acquisition and renewal is handled by the Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="50f01-114">Zpracování získání tokenu pro přístup k chráněné webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="50f01-114">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="50f01-115">Jakmile se uživatel ověřuje, ukázkové aplikace obdrží token, který slouží k dotazu Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="50f01-115">After the user authenticates, the sample application receives a token that can be used to query Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="50f01-116">Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu umožňuje přístup ke konkrétním prostředkům – například čtení profilu uživatele, kalendáře přístup uživatele nebo poslat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="50f01-116">APIs such as Microsoft Graph require an access token to allow accessing specific resources – for example, to read a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="50f01-117">Aplikace může požadovat token přístupu pomocí MSAL k těmto prostředkům přistupovat tak, že zadáte obory rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="50f01-117">Your application can request an access token using MSAL to access these resources by specifying API scopes.</span></span> <span data-ttu-id="50f01-118">Tento přístupový token se pak přidá do hlavičky HTTP autorizace pro každé volání proti k chráněnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="50f01-118">This access token is then added to the HTTP Authorization header for every call made against the protected resource.</span></span> 

<span data-ttu-id="50f01-119">MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.</span><span class="sxs-lookup"><span data-stu-id="50f01-119">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="50f01-120">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="50f01-120">NuGet Packages</span></span>

<span data-ttu-id="50f01-121">Tato příručka používá následující balíčky NuGet:</span><span class="sxs-lookup"><span data-stu-id="50f01-121">This guide uses the following NuGet packages:</span></span>

|<span data-ttu-id="50f01-122">Knihovna</span><span class="sxs-lookup"><span data-stu-id="50f01-122">Library</span></span>|<span data-ttu-id="50f01-123">Popis</span><span class="sxs-lookup"><span data-stu-id="50f01-123">Description</span></span>|
|---|---|
|[<span data-ttu-id="50f01-124">Microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="50f01-124">Microsoft.Identity.Client</span></span>](https://www.nuget.org/packages/Microsoft.Identity.Client)|<span data-ttu-id="50f01-125">Knihovna Microsoft ověřování (MSAL)</span><span class="sxs-lookup"><span data-stu-id="50f01-125">Microsoft Authentication Library (MSAL)</span></span>|

